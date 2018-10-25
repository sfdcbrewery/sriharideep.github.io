---
layout: post
title:  Blazing Salesforce Files - Deep dive into Salesforce content data model (Blog-2)
---

![_config.yml]({{ site.baseurl }}/images/Content/banner.jpg)

In first half of blog post lets discuss the data model of Content in Salesforce ecosystem. In the second half, lets talk how we can access the Content programmatically using Apex and achieve similar functionalities we have in the case of an attachment. Here is the data-model of the content in Salesforce:
￼
![_config.yml]({{ site.baseurl }}/images/Content/file.png)

ContentDocument - Represents a document that has been uploaded to a library in Salesforce CRM Content or Salesforce Files. The maximum number of documents that can be published is 30,000,000. Archived files count toward this limit and toward storage usage limits.
1. Contact Manager, Group, Professional, Enterprise, Unlimited, and Performance Edition customers can publish a maximum of 200,000 new versions per 24-hour period.
1. Developer Edition and trial users can publish a maximum of 2,500 new versions per 24-hour period

* ContentDocumentHistory - Represents the history of a document. A user can query all versions of a document from their personal library and any version that is part of or shared with a library where they are a member, regardless of library permissions.
* ContentDocumentLink - Represents the link between a Salesforce CRM Content document or Salesforce file and where it's shared. A file can be shared with other users, groups, records, and Salesforce CRM Content libraries
* ContentDocumentListViewMapping - Represents an association between a ListView and a Quip ContentDocument. Applies to Quip file types only. Maintains the mapping between a list view and Quip document when the list view is exported to a newly created Quip document.
* ContentDocumentSubscription - Represents a subscription for a user following or commenting on a file in a library. 
* ContentFolder - Represents a folder in a content library for adding files. 
* ContentFolderItem - Represents a file (ContentDocument) or folder (ContentFolder) that resides in a ContentFolder in a ContentWorkspace. 
* ContentFolderLink - Defines the association between a library and its root folder.
* ContentVersion - Represents a specific version of a document in Salesforce CRM Content or Salesforce Files.
The maximum number of versions that can be published in a 24-hour period is 200,000.
Note: Depending on how files are shared, queries on ContentDocument and ContentVersion without specifying an ID won't return all files a user has access to. For example, if a user only has access to a file because they have access to a record that the file is shared with, the file won't be returned in a query such as "SELECT Id FROM ContentDocument."
* ContentWorkspace -  represents a content library excluding the personal libraries. 
* ContentWorkspaceDoc -Represents a link between a document and a public library in Salesforce CRM Content. 
* ContentWorkspaceMember - Represents a member of a content library.
* ContentWorkspacePermission - Represents a library permission.
* ContentWorkspaceSubscription - Represents a subscription for a user following a library.
* ContentAsset - Represents a Salesforce file that has been converted to an asset file in a custom app in Lightning Experience. Use asset files for org setup and configuration.
* ContentBody - Represents the body of a file in Salesforce CRM Content or Salesforce Files.
* ContentDistribution - Represents information about sharing a document externally. 
* ContentDistributionView - The read-only object which represents information about views of a shared document.   

### Customer and Partner Portal users must have the “View Content in Portal” permission to query content in libraries where they have access. 

As mentioned in the previous post Files are here to replace Attachments in Salesforce. If document is uploaded in Salesforce Lightning experience today, it becomes part of Files/Content in Salesforce. Files can be shared across objects which means having a attachment with multiple parents. We all being uploading attachment in different ways one and of the ways is via Apex. Each time a file is uploaded a new record of ContentDocument object gets created. Content Version who is child of ContentDocument is required to perform programmatic actions on Files. 

```
String yourFiles = 'Lets assume this is your binary string of the files';
 
ContentVersion conVer = new ContentVersion();
conVer.ContentLocation = 'S'; // S specify this document is in SF, use E for external files
conVer.PathOnClient = 'ionicLogo.png'; // The files name, extension is very important here which will help the file in preview.
conVer.Title = 'Proposal '; // Display name of the files
conVer.VersionData = EncodingUtil.base64Decode(yourFiles); // converting your binary string to Blog
insert conVer;

```
This will insert your files but it will still not be visible under Attachment/Files related list. For that you need to create
ContentDocumentLink which will share the files with users, records, groups etc.

```
// First get the content document Id from ContentVersion
Id conDoc = [SELECT ContentDocumentId FROM ContentVersion WHERE Id =:conVer.Id].ContentDocumentId;
 
//Create ContentDocumentLink
ContentDocumentLink cDe = new ContentDocumentLink();
cDe.ContentDocumentId = conDoc;
cDe.LinkedEntityId = yourObjectRecord.Id; // you can use objectId,GroupId etc
cDe.ShareType = 'I'; // Inferred permission, checkout description of ContentDocumentLink object for more details
cDe.Visibility = 'InternalUsers';
insert cDe;

```
You can create multiple records to attach the same files under multiple records. In the next blog post, lets discuss how we can scan the files that are being uploaded to Salesforce.

Resources:
1. [Vivek Deepak Blog](https://vivekdeepak.wordpress.com/2017/07/05/upload-files-as-attachment-using-apex-salesforce/)
2. [Developer documents](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects.htm)
