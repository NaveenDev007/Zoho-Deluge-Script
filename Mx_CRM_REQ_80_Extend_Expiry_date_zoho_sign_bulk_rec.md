string button.REQ_80_Extend_Expiry_date_zoho_sign_bulk_rec(String BenIds)
{
E_Date = zoho.currentdate.addBusinessDay(7).toString("dd MMMM yyyy");
info E_Date;
Lis = BenIds.toList("|||");
BenIdList = Lis.toList();
if(BenIdList.isEmpty())
{
	Msg = "Id's Not found";
}
for each  BenId in BenIdList
{
	zohosignDocInfo = zoho.crm.getRelatedRecords("Zoho_Sign_Documents","Ben_Engaments",BenId);
	if(zohosignDocInfo.isEmpty())
	{
		Msg = "Zoho Sign Documents are Not found";
	}
	for each  doc in zohosignDocInfo
	{
		DocStatus = doc.get("zohosign__Document_Status");
		info DocStatus;
		if(DocStatus == "Expired")
		{
			Request_Id = doc.get("zohosign__ZohoSign_Document_ID");
			info "Request_Id: " + Request_Id;
			paramsMap = {"expire_by":E_Date};
			resp = invokeurl
			[
				url :"https://sign.zoho.sa/api/v1/requests/" + Request_Id + "/extend"
				type :PUT
				parameters:paramsMap
				connection:"zsign"
			];
			info "resp: " + resp;
			if(resp.get("status") == "success")
			{
				Msg = "Expire date has extended successfully";
			}
		}
		else
		{
			Msg = "Documents are not Expired";
		}
	}
}
return Msg;
}
