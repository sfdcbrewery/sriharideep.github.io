---
layout: post
title:  Adios Notes & Attachments! Aloha Salesforce Files! - An introduction to Salesforce Files management and collaboration. (Blog-1)
---

![_config.yml]({{ site.baseurl }}/images/Filescan/sfdcbrewerysalesforcefiles.jpg)

This is the first blog post in Salesforce Files series. In total three blog posts, I would like to introduce Salesforce Files, discuss the data model and demonstrate a proof of concept on how we can scan the Salesforce files using third-party security software tools.  
## Salesforce Files - an introduction (Blog -1) 
Its time to peace out the war between Documents, Content, Attachments, and Files within Salesforce. Salesforce Files offers a streamlined approach that unifies your files, documents, and libraries into a single place for easier management & collaboration . Below are some of the highlights of the Files home:
1. Access all the files you own or have access to.

![_config.yml]({{ site.baseurl }}/images/Filescan/screen1.png)

2. Libraries offer a way to organize and share content . One can even create folders within libraries, to better organize your content.
3. You can attach files that can go with the records. All file types from Microsoft® PowerPoint® presentations and Excel® spreadsheets to Adobe® PDFs, images, and audio and video files are support. Hurray! 
4. Access the files directly from the list with preview which also includes controls for downloading, sharing, or deleting the current file, uploading a new version of the file, editing file details, generating a public link to the file, and switching to full-screen mode for presentations.
5. With Files you can also access the files in your external repositories like Quip, Box, Google Drive, Dropbox etc. 

![_config.yml]({{ site.baseurl }}/images/Filescan/screen2.png)

6. Files also offer versioning that helping businesses to streamline better. 
7. Follow and receive updates about a file in the Chatter feed. 
8. If Files Connect is enabled in your org, browse, search, and share files stored in an external data source right from Salesforce

Files is the way to go to attach some files to an opportunity, account, or other records. Adios Notes & Attachments! If you are in classic and want the functionality to attach Files to Notes & Attachments, change the org preference to have files that are added to the list. 

Now lets see why Salesforce files is a fun trail to blaze:

![_config.yml]({{ site.baseurl }}/images/Filescan/screen3.png)

Its now a no-brainer, Files are the best way to go. Attachments will eventually be deprecated, so to help slow their proliferation, users can’t create new attachments in Lightning Experience but they can still be accessed in the read-only mode.  If you are using Salesforce CRM Content in your org, then keep smiling as you do not have do anything in LEX. Content libraries are automatically converted to Salesforce Files and accessible from Files home via the Libraries filter.  Its also time to ditch documents and consider using asset folder in Salesforce Files as they are no longer supported in Lightning experience. 

## Checklist for Salesforce Files goodness:
1. Add files to all your page layouts. 
2. Change the org preference to consider new uploads as files. In General Settings, enable ‘Files uploaded to the Attachments related list on records are uploaded as Salesforce Files, not as attachments’.
3. Download all the file sin the Documents tab and move them to Asset library.
4. Convert all your existing Attachments to files using this cool [App-exchange tool by Salesforce Labs](https://appexchange.salesforce.com/listingDetail?listingId=a0N3A00000EHAmyUAH).

## Files are automatically added to the file list when:
1. You upload a file.
2. You attach a file to a record.
3. You or someone else attaches a file to a Chatter feed or comment.
4. Someone else shares a file privately with you using the Sharing dialog box.
5. You upload a file to a Salesforce CRM Content library.
6. Someone else uploads a file to a library you're a member of.
7. You or someone else creates a content pack or uploads a web link in Salesforce CRM Content. You only see files from content packs and web links you have access to.

Check out this cool trail that can get you more interested in Salesforce Files [Trailhead](https://trailhead.salesforce.com/en/content/learn/modules/lightning-experience-productivity/work-with-notes-and-files).

Lets discuss the Files data model in the next blogpost.  Until then keep blazing :)

# Resources:
1. [Help](https://help.salesforce.com/articleView?id=collab_salesforce_files_parent.htm&type=0)
2. [Lori Sanders](https://admin.salesforce.com/pro-tip-simplify-file-management-salesforce-files-lightning-experience)
