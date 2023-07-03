Usage Rearchitected





Usage Rearchitected

Feature Details Document	




|Revision|Date|Author|Remarks|
| :- | :- | :- | :- |
|1|26-05-2023|Manjunath R|First Draft|



# Contents
[Feature Description	3](#_toc94016944)

[Requirements Specifications	3](#_toc94016945)

[Solution Overview	3](#_toc94016946)

[Modules	3](#_toc94016947)

[APIs	3](#_toc94016948)

[Scope	3](#_toc94016949)

[Use cases	3](#_toc94016950)

[Assumptions	3](#_toc94016951)




# <a name="_toc94016944"></a>Feature Description


|Feature Name|Usage Rearchitected|
| :- | :- |
|Microservice name||
|Database (yes/no)|yes|
|Public APIs (yes/no)|no|
|Complexity|High|

# <a name="_toc94016945"></a>Requirements Specifications
<a name="_toc94016946"></a>Nuvelink is responsible to calculate the usage everyday based on the cloud platform. Fetching the usage is

Different for each platform. Following are the ways the usage is calculated by Nuvelink

1. AWS: The Amazon Web Service will generate the line-item report file and push it to the configured S3 bucket. This file will be downloaded by the Nuvelink, and usage will be manually calculated using Linked Account ID then the usage will be mapped to lab and stored in the ‘usergroupsubscriptioncostdetails’ table.
1. CSP: The Nuvelink is using the rate card API and usage API. Once the Nuvelink gets the rate card and the usage details, it will be computing the usage manually and mapped to lab and stored in the ‘usergroupsubscriptioncostdetails’ table.
1. GCP: Platform is not supported for usage calculation.

As we can observe the common thing in the AWS and CSP is that the usage details will be fetched, and the calculation is done manually. This manual calculation leading to some the mismatch in the usage across the product line. 
# Issues observed.
1. Usage calculation can go wrong when the price or the rate details has the invalid values such as NaN. This issue observed in the AWS.
1. ` `Since the Cloud assets are reused as the pool items the Nuvelink is failing to map the usage to the right lab. It is observed that the usage is added to the lab before creation of the lab or after expiry of the lab
1. Usage in the Account consolidated usage and the Nuvelink dashboard is not matching. Since the query used in the these are different
1. Usage is not correct in the cloud lab dashboard. Since the data is generated using the sync date instead of the usage date. 
1. The Jobs running for calculating the usage takes more that 8Hrs everyday and requires more computation resource. 
# <a name="_hlk132741843"></a>Solution Overview	
<a name="_toc94016947"></a>	As per our discussion in the above section and considering the RCA we can conclude that the cause of this is due to calculating the usage manually. There are advance Usage APIs provided the cloud vendors. We can make use of those API to get the daily usage for all the labs. Advantages of using APIs to get the usage details as follows.

1. This method will help in removing the load on the billing server for running huge process to fetch and calculate the usage.
1. Job time taken to calculate the usage will be reduced since the consolidated usage will be fetched from the cloud.
1. Usage will match across the report and the product dashboard. 
1. Leakage can be identified very quickly.

Note: The only disadvantage of this method is that the Cost Usage APIs provided by the cloud is chargeable and varies based on the platform

# Common Solution
The ‘usergroupsubscriptioncostdetails’ is added with the following columns.

1. isProcessed: The Boolean flag is introduced to the table to understand whether the usage is updated or not.
1. createdOn: This column is used to understand when the row is created by the Nuvelink.
1. updatedOn: This column is used to understand when the row is updated the cost. 
1. tenantId: This column is used to identify the row belongs to which tenant. In the current workflow duplicate row is created to add the same usage for the lab but the group id will be pointing to default tenant group

A Job will be introduced to generate the active labs list for every day. This data will be inserted to ‘usergroupsubscriptioncostdetails’ table with the isProcessed false. This job can be run in any frequency.


# AWS Solution
The AmazonBillGenJob is responsible for fetching the usages for all the labs. This job will fetch the all the active labs and start pulling the usage for the given date. Once the usage is successfully updated then the isProcessed flag will be set to true and will not be picked up from the next time for processing. Since the usage is updated every 4hrs in the AWS, Nuvelink will fetch the usage for the active labs which are created 1day before. This is to make sure the correct usage is fetched rather than the partial usage.

# Azure Solution
The CSPBillGenJob is responsible for fetching the usages for all the labs. This job will fetch the all the active labs and start pulling the usage for the given date. Once the usage is successfully updated then the isProcessed flag will be set to true and will not be picked up from the next time for processing. Since the usage take maximum of 2days in the azure to updated, Nuvelink will fetch the usage for the active labs which are active 3days before. This is to make sure the correct usage is fetched rather than the partial usage.

# Cloud cost report
This is the new report which will be generated by the Nuvelink. This report will take the date range as the input. The report will have the cloud asset ID (SubscriptionID, ARN, ProjectId) and the budget for the given dates in the cloud and the cumulative addition of all the labs that has used in the given date. Below is the sample snapshot of the report.

# Pool item Quarantine
Pool quarantine is the feature which will helps to identify the resource which are running in the cloud without the knowledge of Nuvelink. This will lead to the cost usage leak which finally has to be bear by the Nuvepro. To avoid this leak, we are introducing the **Pool item Quarantine.** 

The pool item will be monitored daily to check whether it is generating the cost. If yes, then those items will be marked with the status ‘QUARANTINE’. The support team or respected team will check the subscription and perform the cleanup. 

The quarantine pool items status will be changed to AVAILABLE when the subscription stops generating the usage in the cloud. This process will help us to identify on which pool items are responsible for leakages and pool items can be changed to available state ASAP instead of waiting till the COOL\_DOWN period to be completed. 
# Deprecated APIs
1. Detailed line-item usage report from the cloud in the Cloud dashboard is deprecated since the line item is not stored in the Nuvelink.
1. Detailed line-item usage report from the lab control panel is deprecated since the line item is not stored in the Nuvelink.
# Usage APIs used in the Cloud labs and Report.















Nuvepro Technologies Private Limited

No.26 (Old No. 686), 16th Main, 4th T Block, Jayanagar

Bangalore-560 041

INDIA

Email – <info@nuvepro.com> 

Web- [www.nuvepro.com](http://www.nuvepro.com)




This document is the exclusive property of Nuvepro Technologies Private Limited (Nuvepro). The recipient agrees that they will not copy, transmit, use, or disclose the confidential and proprietary information in this document by any means without the expressed and written consent of Nuvepro. 














Nuvepro Technologies Private Limited

No.26 (Old No. 686), 16th Main, 4th T Block, Jayanagar

Bangalore-560 041

INDIA

Email – <info@nuvepro.com> 

Web- [www.nuvepro.com](http://www.nuvepro.com)




This document is the exclusive property of Nuvepro Technologies Private Limited (Nuvepro). The recipient agrees that they will not copy, transmit, use, or disclose the confidential and proprietary information in this document by any means without the expressed and written consent of Nuvepro. 





© Nuvepro Technologies Private Limited 2021-202212
