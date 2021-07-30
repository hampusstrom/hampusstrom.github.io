+++ 
draft = true
date = 2021-07-30T10:40:21+02:00
title = "ConfigMgr - Third-Party Updates and How I Automated Them"
description = "Third-Party Updates in Configuration Manager and How I Fully Automated Them Without Expensive Products"
slug = ""
authors = ["Hampus Ström"]
tags = ["Adobe","ConfigMgr","Configuration Manager","SCCM","MEMCM","Third-Party Updates"]
categories = ["Configuration Manager"]
externalLink = ""
series = []
+++

# Introduction
Have you already automated all your Microsoft product updating needs with [Automatic Deployment Rules](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/automatically-deploy-software-updates), but you're still manually updating Third-Party updates like Adobe Reader through SCUP/the MEMCM console? 
Ever thought about how nice it would be if you could use for Third-Party products like Adobe Reader? Yeah, me too.  

You might even have tried it and cried a little bit when you noticed that Automatic Deployment Rules don't pick up Third-Party Updates until they have been published. Me too.    

You may have cried even more when you noticed that there is no PowerShell cmdlet that lets you automate the publishing either. Again, me too. 

Until now it has only been possible to fully automate Third-Party update deployments by using third party software solutions like [PatchMyPC](https://patchmypc.com/). However, with ConfigMgr 2103 this has finally changed, thanks to the addition of the following PowerShell cmdlet:   
[Publish-CMThirdPartySoftwareUpdateContent](https://docs.microsoft.com/en-us/powershell/module/configurationmanager/publish-cmthirdpartysoftwareupdatecontent?view=sccm-ps). I could not find anything about this cmdlet in the [ConfigMgr 2103 Patch Notes](https://docs.microsoft.com/en-us/mem/configmgr/core/plan-design/changes/whats-new-in-version-2103#powershell), but as you can see it is in the [documentation](https://docs.microsoft.com/en-us/powershell/module/configurationmanager/publish-cmthirdpartysoftwareupdatecontent?view=sccm-ps).

Now, with the help of a little bit of scripting we can publish and deploy Third-Party Updates automatically without the need for expensive products, hell yeah!  

Here's how I did it and how you can do it too.

# Prerequisites
- Microsoft Endpoint Configuration Manager version 2103 or higher
- [Third-Party Update Support Enabled and Configured](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-updates)
- Any [Third-Party Update Catalogs](https://docs.microsoft.com/en-us/mem/configmgr/sum/deploy-use/third-party-software-update-catalogs) Set Up and Working 
- Some PowerShell scripting Knowledge

# Instructions

### Automatic Deployment Rule
Product: Adobe Reader
Classification: Security Update

### Status Filter Rules

Publish Third-Party Update Content On Catalog Sync Completion  
Component: SMS_ISVUPDATES_SYNCAGENT  
Message ID: 11503  
Site Code: if needed  
Action: Run a program - script.cmd %msgis01  

Run Third-Party Update ADRs Whenever New Third-Party Content Is Published Successfully  
Component: SMS_ISVUPDATES_SYNCAGENT  
Message ID: 11515  
Action: run a proram - script2.cmd  