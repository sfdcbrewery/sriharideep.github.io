---
layout: post
title:  Salesforce content security - File scanning with OPSWAT & F-Secure (Blog-3)
---
![_config.yml]({{ site.baseurl }}/images/Content/security.jpg)

Salesforce at the moment, doesn’t provide a way to scan attachments, document uploads, or chatter for viruses or malicious content. This is a huge security risk for all Salesforce users, especially when a Salesforce application is publicly exposed through communities and sites.com. . From the info-sec perspective, desktop virus scanners or BYOD installations do not meet the cut for Salesforce, especially if using communities or sites. Simply put, any file that is uploaded to Salesforce brings with it the opportunity to carry a virus, malware, or unintended active content. There are four main avenues for delivery of these types of files within Salesforce:
1. Attachments associated with cases, leads, opportunities, etc.
2. Documents
3. Content Files, including chatter attachments
4. Static Resources

Salesforce at this point doesn’t offer security, and is relying on third party vendors to provide solutions for their platform. There are currently few solutions available on the marketplace like, OPSWAT,  EZProtect and F-Secure. EZProtect was the first solution offered on the market, and was designed by certified Salesforce achitects by US-based Adaptus. F-Secure Cloud Protection for Salesforce was created by New Zealand-based F-Secure.  While the product comparison is out of scope for this blog post, we will try to uncover how we can leverage security scanning using Apex and Metadefender cloud API.

Lets first discuss the OPSWAT Metadefender Cloud API. MetaDefender is a cyber security platform for preventing and detecting cyber security threats on multiple data channels. It offers a free API which lets you scan your files uploaded in Salesforce with 40+ security softwares. Let’s get started by registering for a free account in Metadefender Cloud [here](https://metadefender.opswat.com/account#!/). Once you have the API key, you ready to get started. According to the content data model explained in my previous blog post, we are going to write a after insert trigger on ContentVersion which is the child of ContentDocument. This will allow us to scan the file each time a new document or document version is uploaded into salesforce. 

```
trigger contentVersionTriggerScan on ContentVersion (after insert) {
    List<Id> cond = new List<Id>();
    List<ContentDistribution> cd = new List<ContentDistribution>(); 
    List<Id> CDID = new List<Id>();
    if (trigger.isAfter){
        if (trigger.isInsert){
            for (ContentVersion coD : trigger.new){
                
                cond.add(coD.ContentDocumentId);
            }
            for(ContentVersion cv : [select id, contentdocumentid from contentversion where contentdocumentid = :cond]){
                ContentDistribution cd1 = new ContentDistribution();
                cd1.Name = 'Test'+ cv.Id;
                cd1.ContentVersionId = cv.Id;
                cd1.PreferencesAllowViewInBrowser= true;
                cd1.PreferencesLinkLatestVersion=true;
                cd1.PreferencesNotifyOnVisit=false;
                cd1.PreferencesPasswordRequired=false;
                cd1.PreferencesAllowOriginalDownload= true;
                cd.add(cd1);
                CDID.add(cv.ContentDocumentId);
            }      
        }
        insert cd;
        metaCallouts.makePostFutureCallout(CDID);
    }
}

```

We are calling the Metadefender API from the trigger to initiate the scan. This involves a two step process involving one POST and one GET call. First make a POST call to the API with the file body and API key in the header. In the response, we get the data_Id which will be used to get the scan status that is used in the next step. The response is as below:

```
{
    "data_id": "ZDE4MTAxNnJ5dml1OW9tc21CeU9qT2NqWGpt",
    "status": "inqueue",
    "in_queue": 3,
    "queue_priority": "normal",
    "rest_ip": "api.metadefender.com/v2"
}

```
Once we have the data_id, we will make a GET request to Metadefender API which will return scan status in the JSON response as below:

```
{
    "file_id": "ZDE4MTAxNnJ5dml1OW9tc20",
    "data_id": "ZDE4MTAxNnJ5dml1OW9tc21CeU9qT2NqWGpt",
    "archived": false,
    "process_info": {
        "user_agent": "",
        "result": "Allowed",
        "progress_percentage": 100,
        "profile": "File scan",
        "file_type_skipped_scan": false,
        "blocked_reason": ""
    },
    "scan_results": {
        "scan_details": {
            "Zillya!": {
                "wait_time": 46,
                "threat_found": "",
                "scan_time": 1,
                "scan_result_i": 0,
                "def_time": "2018-10-12T07:38:00.000Z"
            },
            "Xvirus Personal Guard": {
                "wait_time": 21,
                "threat_found": "",
                "scan_time": 57,
                "scan_result_i": 0,
                "def_time": "2018-10-15T09:31:00.000Z"
            },
            "VirusBlokAda": {
                "wait_time": 46,
                "threat_found": "",
                "scan_time": 1,
                "scan_result_i": 0,
                "def_time": "2018-10-15T08:35:00.000Z"
            },
            "TrendMicro House Call": {
                "wait_time": 19,
                "threat_found": "",
                "scan_time": 778,
                "scan_result_i": 0,
                "def_time": "2018-10-09T21:13:00.000Z"
            },
            "TrendMicro": {
                "wait_time": 28,
                "threat_found": "",
                "scan_time": 675,
                "scan_result_i": 0,
                "def_time": "2018-10-15T09:02:00.000Z"
            },
            "Total Defense": {
                "wait_time": 40,
                "threat_found": "",
                "scan_time": 7,
                "scan_result_i": 0,
                "def_time": "2018-10-15T00:00:00.000Z"
            },
            "ThreatTrack": {
                "wait_time": 32,
                "threat_found": "",
                "scan_time": 15,
                "scan_result_i": 0,
                "def_time": "2018-10-14T08:10:00.000Z"
            },
            "TACHYON": {
                "wait_time": 42,
                "threat_found": "",
                "scan_time": 5,
                "scan_result_i": 0,
                "def_time": "2018-10-16T05:00:00.000Z"
            },
            "Sophos": {
                "wait_time": 45,
                "threat_found": "",
                "scan_time": 2,
                "scan_result_i": 0,
                "def_time": "2018-10-15T03:38:00.000Z"
            },
            "SUPERAntiSpyware": {
                "wait_time": 25,
                "threat_found": "",
                "scan_time": 1147,
                "scan_result_i": 0,
                "def_time": "2018-10-05T20:08:00.000Z"
            },
            "Quick Heal": {
                "wait_time": 46,
                "threat_found": "",
                "scan_time": 1,
                "scan_result_i": 0,
                "def_time": "2018-10-14T09:09:00.000Z"
            },
            "Preventon": {
                "wait_time": 22,
                "threat_found": "",
                "scan_time": 56,
                "scan_result_i": 0,
                "def_time": "2018-10-15T23:24:00.000Z"
            },
            "NANOAV": {
                "wait_time": 44,
                "threat_found": "",
                "scan_time": 3,
                "scan_result_i": 0,
                "def_time": "2018-10-16T03:26:00.000Z"
            },
            "McAfee": {
                "wait_time": 42,
                "threat_found": "",
                "scan_time": 5,
                "scan_result_i": 0,
                "def_time": "2018-10-15T00:00:00.000Z"
            },
            "K7": {
                "wait_time": 47,
                "threat_found": "",
                "scan_time": 0,
                "scan_result_i": 0,
                "def_time": "2018-10-16T00:17:00.000Z"
            },
            "Ikarus": {
                "wait_time": 47,
                "threat_found": "",
                "scan_time": 0,
                "scan_result_i": 0,
                "def_time": "2018-10-16T07:40:43.000Z"
            },
            "Huorong": {
                "wait_time": 32,
                "threat_found": "",
                "scan_time": 0,
                "scan_result_i": 0,
                "def_time": "2018-10-14T09:15:00.000Z"
            },
            "Hauri": {
                "wait_time": 46,
                "threat_found": "",
                "scan_time": 1,
                "scan_result_i": 0,
                "def_time": "2018-10-16T00:00:00.000Z"
            },
            "Fortinet": {
                "wait_time": 30,
                "threat_found": "",
                "scan_time": 17,
                "scan_result_i": 0,
                "def_time": "2018-10-15T00:00:00.000Z"
            },
            "Filseclab": {
                "wait_time": 9,
                "threat_found": "",
                "scan_time": 195,
                "scan_result_i": 0,
                "def_time": "2018-10-15T01:24:00.000Z"
            },
            "F-secure": {
                "wait_time": 25,
                "threat_found": "",
                "scan_time": 632,
                "scan_result_i": 0,
                "def_time": "2018-10-14T04:08:00.000Z"
            },
            "F-prot": {
                "wait_time": 32,
                "threat_found": "",
                "scan_time": 0,
                "scan_result_i": 0,
                "def_time": "2018-10-16T02:06:00.000Z"
            },
            "Emsisoft": {
                "wait_time": 25,
                "threat_found": "",
                "scan_time": 7,
                "scan_result_i": 0,
                "def_time": "2018-10-15T20:35:00.000Z"
            },
            "Cyren": {
                "wait_time": 26,
                "threat_found": "",
                "scan_time": 6,
                "scan_result_i": 0,
                "def_time": "2018-10-16T02:55:00.000Z"
            },
            "ClamAV": {
                "wait_time": 15,
                "threat_found": "",
                "scan_time": 32,
                "scan_result_i": 0,
                "def_time": "2018-10-16T04:55:00.000Z"
            },
            "ByteHero": {
                "wait_time": 14,
                "threat_found": "",
                "scan_time": 268,
                "scan_result_i": 0,
                "def_time": "2018-10-15T00:00:00.000Z"
            },
            "ESET": {
                "wait_time": 32,
                "threat_found": "",
                "scan_time": 0,
                "scan_result_i": 0,
                "def_time": "2018-10-16T00:00:00.000Z"
            },
            "BitDefender": {
                "wait_time": 30,
                "threat_found": "",
                "scan_time": 2,
                "scan_result_i": 0,
                "def_time": "2018-10-16T04:37:00.000Z"
            },
            "Avira": {
                "wait_time": 32,
                "threat_found": "",
                "scan_time": 0,
                "scan_result_i": 0,
                "def_time": "2018-10-14T00:00:00.000Z"
            },
            "Antiy": {
                "wait_time": 32,
                "threat_found": "",
                "scan_time": 0,
                "scan_result_i": 0,
                "def_time": "2018-10-16T04:35:00.000Z"
            },
            "Ahnlab": {
                "wait_time": 31,
                "threat_found": "",
                "scan_time": 1,
                "scan_result_i": 0,
                "def_time": "2018-10-16T00:00:00.000Z"
            },
            "Agnitum": {
                "wait_time": 31,
                "threat_found": "",
                "scan_time": 1,
                "scan_result_i": 0,
                "def_time": "2018-10-12T05:10:00.000Z"
            },
            "AegisLab": {
                "wait_time": 32,
                "threat_found": "",
                "scan_time": 0,
                "scan_result_i": 0,
                "def_time": "2018-10-15T01:00:00.000Z"
            },
            "Vir.IT eXplorer": {
                "wait_time": 45,
                "threat_found": "",
                "scan_time": 2,
                "scan_result_i": 0,
                "def_time": "2018-10-15T14:46:00.000Z"
            },
            "Vir.IT ML": {
                "wait_time": 46,
                "threat_found": "",
                "scan_time": 1,
                "scan_result_i": 0,
                "def_time": "2018-10-12T14:46:00.000Z"
            }
        },
        "rescan_available": true,
        "data_id": "ZDE4MTAxNnJ5dml1OW9tc21CeU9qT2NqWGpt",
        "scan_all_result_i": 0,
        "start_time": "2018-10-16T18:14:10.992Z",
        "total_time": 1208,
        "total_avs": 35,
        "total_detected_avs": 0,
        "progress_percentage": 100,
        "in_queue": 0,
        "scan_all_result_a": "No Threat Detected"
    },
    "file_info": {
        "file_size": 146,
        "upload_timestamp": "2018-10-16T18:14:10.981Z",
        "md5": "0C8CFB7FCD3CA747CC7942DEB37AC8B1",
        "sha1": "DCAD7AD6AC820AAA605032DB88BB73F6471D1EAB",
        "sha256": "ED37336B7A4B569B9B28D28E9CA62067EB309B1F6DC297D60A618ED079318772",
        "file_type_category": "T",
        "file_type_description": "ASCII text, with no line terminators",
        "file_type_extension": "txt",
        "display_name": "Unknown Filename"
    },
    "top_threat": -1,
    "share_file": 1,
    "rest_version": "4",
    "scan_result_history_length": 1,
    "votes": {
        "up": 0,
        "down": 0
    },
    "original_file": {
        "detected_by": 0,
        "progress_percentage": 100,
        "scan_result_i": 0,
        "data_id": "ZDE4MTAxNnJ5dml1OW9tc21CeU9qT2NqWGpt"
    }
}

```
Refer the API docs of the OPSWAT Metadefender Cloud [here](https://onlinehelp.opswat.com/mdcloud/). Here is Future class that handles the API calls and response updates. 

```
public class metaCallouts {
    @future(callout=true)
     public static void makePostFutureCallout(List<id> cd) {   
         List<ContentVersion> cdstatus = new List<ContentVersion>();
         List<Content__c> cnstatus = new List<Content__c>();
            for(id c1: cd){
                ContentVersion CVe = [SELECT Id, contentdocumentid, Status__c FROM ContentVersion WHERE ContentDocumentId =: c1 LIMIT 1];
                string endURL = 'https://your-domain.my.salesforce.com/' + c1;
                HttpRequest request = new HttpRequest();
                request.setEndpoint('https://api.metadefender.com/v2/file');
                request.setMethod('POST');  
                request.setHeader('Content-Type', 'application/x-www-form-urlencoded');
                request.setHeader('apikey’,’API-KEY’);
                request.setBody('{"URL": endURL }');
                Http http = new Http();
                HttpResponse response = http.send(request);
                string ress = response.getBody();
                Map<String, Object> results = (Map<String, Object>) JSON.deserializeUntyped(response.getBody());
                String name = (String)results.get('data_id');
                Integer start = System.Now().millisecond();
                ApexUtil.wait(10000);
                Http http1 = new Http();
                HttpRequest request1 = new HttpRequest();
                request1.setEndpoint('https://api.metadefender.com/v2/file/'+ name);
                request1.setMethod('GET');
                request1.setHeader('apikey','API-KEY');
                HttpResponse response1 = http.send(request1);
                string res = response1.getBody();
                JSON2Apex obj = (JSON2Apex) System.JSON.deserialize(res, JSON2Apex.class);
                string reso = obj.Scan_results.scan_all_result_a ;
                system.debug(reso);  
                CVe.Status__c = reso;
                cdstatus.add(CVe);
            }
         upsert cdstatus;      
        }      
}

```
Apart from making two subsequent API calls, we are programmatically adding a delay to make the second API call that gets us the scan status. To have the scan status associated with the ContentVersion for future reporting and use, a Status custom field is created in ContentVersion object. At the end off the future call, Status custom field is updated in the associated ContentVersion record. Here is a small POC demo fo a Lightning component that uploads a file and gives the scan status on submit. 

￼![_config.yml]({{ site.baseurl }}/images/Content/sfdcbreweryfilescan.gif)

While OPSWAT API is useful for majority of your infrastructure needs, it is recommended to go with the apps like F-secure from the App-Exchange if your use case is Salesforce specific. As part of our pilot, we tried F-secure and I am pretty impressed with their application. Here is the F-secure infrastructure snapshot:
￼![_config.yml]({{ site.baseurl }}/images/Content/fsecure.png)

And here is the impressive analytics we get to experience within Salesforce once its installed. 

File scanning is an important functionality that cannot be missed especially if you’re using communities or sites that are exposed to the external users. This sums up the three blog series of Files, content and security scanning in Salesforce. Hope you all found it helpful. Keep blazing!

## Resources:
1. [OPSWAT API](https://onlinehelp.opswat.com/mdcloud/)
2. [F-secure](https://www.f-secure.com/en_US/web/business_us/cloud-protection-for-salesforce)
