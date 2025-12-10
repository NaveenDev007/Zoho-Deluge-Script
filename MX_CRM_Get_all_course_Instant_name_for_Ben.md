string standalone.Get_all_course_Instant_name_for_Ben(String benID)
{
/* FETCH BENEFICIARY ENGAGEMENT DETAILS */
BenInfo = zoho.crm.getRecordById("Ben_Engaments",benID);
Intervention_Id = BenInfo.get("Intervention_Name").get("id");
/* FETCH RELATED COURSE CATALOG */
Course_Cat = zoho.crm.getRelatedRecords("Course_Catalog","Interventions",Intervention_Id);
if(Course_Cat.isEmpty())
{
	return "No Course Catalog found for this Intervention.";
}
Course_Cat_Id = Course_Cat.get(0).get("id");
/* FETCH ALL RELATED COURSE INSTANCES VIA API */
allrelatedInstance = invokeurl
[
	url :"https://www.zohoapis.sa/crm/v8/Products/" + Course_Cat_Id + "/Course_Instance?fields=TC_Office,Name,Start_Date,End_Date,Capacity,Available_Seats,Status_TC,Venue_Type"
	type :GET
	connection:"zcrm"
];
Course_Ins = allrelatedInstance.get("data");
info "TOTAL Course_Ins COUNT: " + Course_Ins.size();
if(Course_Ins.isEmpty())
{
	return "No Course Instances found for this Course Catalog.";
}
/* GET BENEFICIARY'S OFFICE */
getContact = zoho.crm.getRecordById("Contacts",BenInfo.get("Beneficiary").get("id"));
getContactOffice = getContact.get("Office");
info "Contact Office: " + getContactOffice;
/* LIST AND COUNTS */
Instance_list = List(); // ADD ALL INSTANCE DETAILS
officeList = List(); //All ALL INSTANCE OFFICE
criteriacount = 0; 
officeMismatchCount = 0; 
noSessionsCount = 0; 
ValideInstanceSize = 0; 
Start_Time_fail_Count = 0;
/* COURSE INSTANCE LOOP */
for each  rec in Course_Ins
{
	info "recID: " + rec.get("id");
	instanceData = invokeurl
	[
		url :"https://www.zohoapis.sa/crm/v8/Course_Instance/" + rec.get("id")
		type :GET
		connection:"zcrm"
	];
	getinstanceData = instanceData.get("data").get(0);
	Venue_Type = getinstanceData.get("Venue_Type");
	Available_seats = getinstanceData.get("Available_Seats");
	info "Venue_Type: " + Venue_Type;
	officeListData = getinstanceData.get("TC_Office");
	/* EXTRACT OFFICE NAME */
	for each  office_rec in officeListData
	{
		officeList.add(office_rec.get("TC_Office").get("name"));
	}
	
	/* SKIP OFFICE CHECK IF VENUE_TYPE = "ONLINE" OR OFFICELIST IS EMPTY */
	skipOfficeCheck = Venue_Type == "Online" || officeList.isEmpty();
	/* ELSE CHECK IF OFFICE MATCHES CONTACT'S OFFICE */
	if(skipOfficeCheck || Venue_Type != "Online" && officeList.contains(getContactOffice))
	{
		Available_Seats = getinstanceData.get("Available_Seats");
		Status_TC = getinstanceData.get("Status_TC");
		if(Available_Seats != null && Available_Seats > 0 && Status_TC == "Published")
		{
			/* EXTRACT VALID INSTANCE COUNT BY CRITERIA */
			ValideInstanceSize = ValideInstanceSize+1;
			/* VALID INSTANC SESSIONS */
			sessions = zoho.crm.getRelatedRecords("Sessions_TC","Course_Instance",rec.get("id"));
			info "sessions size: " + sessions.size();
			if(sessions.size() != 0)
			{
				/* VALID INSTANC SESSIONS LOOP */
				for each  session in sessions
				{
					SessionNo = session.get("Session_Number");
					Session_Name = session.get("Name");
					/* EXTRACT THE FIRST SESSION OF EACH INSTANCE */
					if(SessionNo == 1 || SessionNo = null)
					{
						StartTimeVal = session.get("Start_Time");
						if(StartTimeVal != null)
						{
							thirtyFromNow = zoho.currenttime.addMinutes(30);
							info "thirtyFromNow: " + thirtyFromNow;
							Start_Time = StartTimeVal.toTime();
							info "Start_Time: " + Start_Time;
							/* SHOW ONLY INSTANCE SESSION START TIME > 30 MINS */
							if(Start_Time > thirtyFromNow)
							{
								info Start_Time.toString("dd-MMM-yyyy HH:mm:ss");
						insID = rec.get("id");
						instanceName = getinstanceData.get("Name");
						instanceStartDate = getinstanceData.get("Start_Date");
						instanceEndDate = getinstanceData.get("End_Date");
						capacity = getinstanceData.get("Capacity");
						Instance_list.add(insID + ":-(" + instanceStartDate + " To " + instanceEndDate + ") - " + capacity + " - " + Venue_Type + " - " + "Available Seats: " + Available_seats);
							}
							else{Start_Time_fail_Count=Start_Time_fail_Count+1;}
						}
					}
				}
			}
			else
			{
				noSessionsCount = noSessionsCount + 1;
			}
		}
		else
		{
			criteriacount = criteriacount + 1;
		}
	}
	else
	{
		officeMismatchCount = officeMismatchCount + 1;
	}
}
info "Office List: " + officeList;
info "officeList size: " + officeList.size();
info "Start_TimeCount: "+Start_Time_fail_Count;
info "ValideInstanceSize: "+ValideInstanceSize;
// FINAL MESSAGE LOGIC
if(criteriacount == Course_Ins.size())
{
	return "Kindly check Available Seats and Status TC";
}
else if(officeMismatchCount == officeList.size())
{
	return "Office name mismatch between Course and Contact module";
}
else if(noSessionsCount == sessions.size())
{
	return "No Sessions are available for this course";
}
else if(Start_Time_fail_Count == ValideInstanceSize)
{
	return "Start time for the first session of Instance must be at least 30 minutes from now";
}
else
{
	return Instance_list.toString();
}
}
