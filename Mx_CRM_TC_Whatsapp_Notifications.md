string standalone.MX_TC_Whatsapp_Notifications(String key)
{
if(key == "sendconfirmationlinkoffline")
{
	templateInfo = "280809~" + "#customerName" + "~" + "#courseName" + "~" + "#VenueLink" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#ARSessionLink" + "~" + "#Duration" + "~" + "#VenueLink" + "~" + "#Link";
	return templateInfo;
}
if(key == "SendConfirmationLinkOnline")
{
	templateInfo = "280808~" + "#customerName" + "~" + "#courseName" + "~" + "#RegNo" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#Duration،" + "~" + "#ARSessionLink" + "~" + "#Link";
	return templateInfo;
}
if(key == "OnEnrollmentCreationOnline")
{
	templateInfo = "280807~" + "#customerName" + "~" + "#courseName" + "~" + "#RegNo" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#Duration" + "~" + "#ArSessionLink";
	return templateInfo;
}
if(key == "OnEnrollmentCreationOffline")
{
	templateInfo = "280806~" + "#customerName" + "~" + "#courseName" + "~" + "#courseMode" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#ARSessionLink" + "~" + "#Duration" + "~" + "#VenueLink" + "~" + "#VenueLink";
	return templateInfo;
}
if(key == "InstanceStartReminderBefore1dayOnline")
{
	templateInfo = "280805~" + "#customerName" + "~" + "#courseName" + "~" + "#RegNo" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#Duration" + "~" + "#ARSessionLink";
	return templateInfo;
}
if(key == "InstanceStartReminderBefore1dayOffline")
{
	templateInfo = "280804~" + "#customerName" + "~" + "#courseName" + "~" + "#courseMode" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#ARSessionLink" + "~" + "#Duration" + "~" + "#VenueLink";
	return templateInfo;
}
if(key == "OnEnrollmentRescheduleOnline")
{
	templateInfo = "280816~" + "#customerName" + "~" + "#courseName" + "~" + "#RegNo" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#Duration" + "~" + "#ARSessionLink";
	return templateInfo;
}
if(key == "OnEnrollmentRescheduleOffline")
{
	templateInfo = "280815~" + "#customerName" + "~" + "#courseName" + "~" + "#VenueLink" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#ARSessionLink" + "~" + "#Duration" + "~" + "#VenueLink";
	return templateInfo;
}
if(key == "OnInstanceCancel")
{
	templateInfo = "280814~" + "#customerName" + "~" + "#courseName";
	return templateInfo;
}
if(key == "OnEnrollCancel")
{
	templateInfo = "280813~" + "#customerName" + "~" + "#courseName";
	return templateInfo;
}
if(key == "OnEnrollmentFail")
{
	templateInfo = "280812~" + "#customerName" + "~" + "#courseName" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#Link";
	return templateInfo;
}
if(key == "SessionOTP")
{
	templateInfo = "280803~" + "#otp";
	return templateInfo;
}
if(key == "SessionReminderOnline")
{
	templateInfo = "280811~" + "#customerName" + "~" + "#courseName" + "~" + "#RegNo" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#link";
	return templateInfo;
}
if(key == "SessionReminderOffline")
{
	templateInfo = "280810~" + "#customerName" + "~" + "#courseName" + "~" + "#VenueLink" + "~" + "#StartDate" + "~" + "#EndDate" + "~" + "#VenueLink";
	return templateInfo;
}
return "";
}
