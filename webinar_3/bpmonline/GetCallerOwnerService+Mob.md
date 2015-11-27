{
  "Version": "7.5.0.1122",
  "UId": "82de7c60-5af9-4973-908f-2b4cf540db33",
  "ManagerName": "SourceCodeSchemaManager",
  "Name": "GetCallerOwnerService",
  "Caption": "GetCallerOwnerService",
  "ExtendParent": false,
  "Description": "",
  "MetaData": "{\r
\n  \"MetaData\": {\r
\n    \"Schema\": {\r
\n      \"ManagerName\": \"SourceCodeSchemaManager\",\r
\n      \"UId\": \"82de7c60-5af9-4973-908f-2b4cf540db33\",\r
\n      \"A2\": \"GetCallerOwnerService\",\r
\n      \"A5\": \"b202bd04-f7e6-4063-bfa4-1f9785c352e8\",\r
\n      \"B1\": [],\r
\n      \"B2\": [],\r
\n      \"B3\": [],\r
\n      \"B6\": \"b0920380-eaa2-43ae-bead-05c975724212\",\r
\n      \"HD1\": \"50e3acc0-26fc-4237-a095-849a1d534bd3\",\r
\n      \"HD2\": \"namespace Terrasoft.Configuration.GetCallerOwnerService\\r\
\n{\\r\
\n\\tusing System;\\r\
\n\\tusing System.Linq;\\r\
\n\\tusing System.ServiceModel;\\r\
\n\\tusing System.ServiceModel.Web;\\r\
\n\\tusing System.ServiceModel.Activation;\\r\
\n\\tusing System.Web;\\r\
\n\\tusing Terrasoft.Core;\\r\
\n\\tusing Terrasoft.Core.DB;\\r\
\n\\tusing Terrasoft.Core.Entities;\\r\
\n\\tusing Newtonsoft.Json;\\r\
\n\\r\
\n\\t[ServiceContract]\\r\
\n\\t[AspNetCompatibilityRequirements(RequirementsMode = AspNetCompatibilityRequirementsMode.Required)]\\r\
\n\\tpublic class GetCallerOwnerService\\r\
\n\\t{\\r\
\n\\r\
\n\\t\\t[OperationContract]\\r\
\n\\t\\t[WebInvoke(Method = \\\"OPTIONS\\\", UriTemplate = \\\"GetCallerOwner\\\")]\\r\
\n\\t\\tpublic void GetCallerOwnerRequestOptions()\\r\
\n\\t\\t{\\r\
\n\\t\\t\\tvar OutgoingResponseHeaders = WebOperationContext.Current.OutgoingResponse.Headers;\\r\
\n\\t\\t\\tOutgoingResponseHeaders.Add(\\\"Access-Control-Allow-Origin\\\", \\\"*\\\");\\r\
\n\\t\\t\\tOutgoingResponseHeaders.Add(\\\"Access-Control-Allow-Methods\\\", \\\"POST\\\");\\r\
\n\\t\\t\\tOutgoingResponseHeaders.Add(\\\"Access-Control-Allow-Headers\\\", \\\"Content-Type, Accept\\\");\\r\
\n\\t\\t}\\r\
\n\\r\
\n\\t\\t[OperationContract]\\r\
\n\\t\\t[WebInvoke(Method = \\\"POST\\\", UriTemplate = \\\"GetCallerOwner\\\",\\r\
\n\\t\\t\\tBodyStyle = WebMessageBodyStyle.WrappedRequest, RequestFormat = WebMessageFormat.Json,\\r\
\n\\t\\t\\tResponseFormat = WebMessageFormat.Json)]\\r\
\n\\t\\tpublic SearchResult GetCallerOwner(string callerIdNumber)\\r\
\n\\t\\t{\\r\
\n\\t\\t\\tvar CurrentWebOperationContext = WebOperationContext.Current; \\r\
\n\\t\\t\\tvar OutgoingResponseHeaders = CurrentWebOperationContext.OutgoingResponse.Headers;\\r\
\n\\t\\t\\tvar incomingRequestOrigin = CurrentWebOperationContext.IncomingRequest.Headers[\\\"Origin\\\"];\\r\
\n\\t\\t\\tOutgoingResponseHeaders.Add(\\\"Access-Control-Allow-Origin\\\", incomingRequestOrigin);\\r\
\n\\r\
\n\\t\\t\\tstring callerIdDigits = new String(callerIdNumber.Where(Char.IsDigit).ToArray());\\r\
\n\\t\\t\\tif (string.IsNullOrEmpty(callerIdDigits))\\r\
\n\\t\\t\\t{\\r\
\n\\t\\t\\t\\treturn new SearchResult() { info = \\\"Wrong Number\\\"};\\r\
\n\\t\\t\\t}\\r\
\n\\t\\t\\t\\r\
\n\\t\\t\\tstring searchNumber = new String(callerIdDigits.Reverse().ToArray());\\r\
\n\\t\\t\\tvar appConnection = HttpContext.Current.Application[\\\"AppConnection\\\"] as AppConnection;\\r\
\n\\t\\t\\tvar userConnection = appConnection.SystemUserConnection;\\r\
\n\\r\
\n\\t\\t\\tvar query = new CustomQuery(userConnection);\\r\
\n\\t\\t\\tquery.SqlText= @\\\"\\r\
\nselect top 1 \\r\
\n\\t'{\\\"\\\"callerIdName\\\"\\\": \\\"\\\"' + Name + '\\\"\\\"' +\\r\
\n\\tisnull(\\r\
\n\\t\\t(select top 1 ', \\\"\\\"callerIdOwner\\\"\\\": \\\"\\\"' + Login + '\\\"\\\"' \\r\
\n\\t\\tfrom WSysAccount where ContactId = OwnerID), '') +\\r\
\n\\tisnull(\\r\
\n\\t\\t(select top 1 ', \\\"\\\"callerIdOwnerMob\\\"\\\": \\\"\\\"' + REVERSE(SearchNumber) + '\\\"\\\"'\\r\
\n\\t\\tfrom ContactCommunication \\r\
\n\\t\\twhere ContactId = OwnerID \\r\
\n\\t\\t\\tand CommunicationTypeId = 'D4A2DC80-30CA-DF11-9B2A-001D60E938C6'), '') +\\r\
\n\\t'}'\\r\
\nfrom (\\r\
\n\\tselect top 1\\r\
\n\\t\\tc.Name Name, \\r\
\n\\t\\tc.OwnerId OwnerID\\r\
\n\\tfrom ContactCommunication cc\\r\
\n\\t\\tjoin Contact c on cc.ContactId = c.Id\\r\
\n\\twhere cc.SearchNumber like '\\\" + searchNumber + \\\"%'\\\" +\\r\
\n\\t@\\\"union all\\r\
\n\\tselect top 1\\r\
\n\\t\\ta.Name Name, \\r\
\n\\t\\ta.OwnerId OwnerID\\r\
\n\\tfrom AccountCommunication ac\\r\
\n\\t\\tjoin Account a on ac.AccountId = a.Id\\r\
\n\\twhere ac.SearchNumber like '\\\" + searchNumber + \\\"%'\\\" + \\r\
\n\\\") ContactAccount\\\";\\r\
\n\\t\\t\\t\\r\
\n\\t\\t\\t\\r\
\n\\t\\t\\tstring result = query.ExecuteScalar<string>();\\r\
\n\\t\\t\\t\\r\
\n\\t\\t\\tif (string.IsNullOrEmpty(result))\\r\
\n\\t\\t\\t{\\r\
\n\\t\\t\\t\\treturn new SearchResult() { info = \\\"Not Found\\\"};\\r\
\n\\t\\t\\t}\\r\
\n\\r\
\n\\t\\t\\treturn JsonConvert.DeserializeObject<SearchResult>(result);\\r\
\n\\t\\t}\\r\
\n\\t}\\r\
\n\\t\\r\
\n\\tpublic class SearchResult\\r\
\n\\t{\\r\
\n\\t\\tpublic SearchResult() \\r\
\n\\t\\t{\\r\
\n\\t\\t\\tinfo = \\\"OK\\\";\\r\
\n\\t\\t}\\r\
\n\\t\\t\\r\
\n\\t\\tpublic string callerIdName {\\r\
\n\\t\\t\\tget;\\r\
\n\\t\\t\\tset;\\r\
\n\\t\\t}\\r\
\n\\r\
\n\\t\\tpublic string callerIdOwner {\\r\
\n\\t\\t\\tget;\\r\
\n\\t\\t\\tset;\\r\
\n\\t\\t}\\r\
\n\\t\\t\\r\
\n\\t\\tpublic string callerIdOwnerMob {\\r\
\n\\t\\t\\tget;\\r\
\n\\t\\t\\tset;\\r\
\n\\t\\t}\\r\
\n\\t\\t\\r\
\n\\t\\tpublic string info {\\r\
\n\\t\\t\\tget;\\r\
\n\\t\\t\\tset;\\r\
\n\\t\\t}\\r\
\n\\t}\\r\
\n} \"\r
\n    }\r
\n  }",
  "Resources": [
    {
      "Culture": "en-GB",
      "Source": "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r
\n<Resources Culture=\"en-GB\">\r
\n\t<Group Type=\"String\" />\r
\n</Resources>"
    },
    {
      "Culture": "en-US",
      "Source": "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r
\n<Resources Culture=\"en-US\">\r
\n\t<Group Type=\"String\" />\r
\n</Resources>"
    },
    {
      "Culture": "ru-RU",
      "Source": "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r
\n<Resources Culture=\"ru-RU\">\r
\n\t<Group Type=\"String\">\r
\n\t\t<Items>\r
\n\t\t\t<Item Name=\"Caption\" Value=\"GetCallerOwnerService\" />\r
\n\t\t</Items>\r
\n\t</Group>\r
\n</Resources>"
    }
  ],
  "Properties": []
}