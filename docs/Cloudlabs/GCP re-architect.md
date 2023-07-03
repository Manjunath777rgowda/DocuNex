GCP Re-architecture





GCP re-architect

Feature Details Document	




|Revision|Date|Author|Remarks|
| :- | :- | :- | :- |
|1|09/11/2022|Siva Krushna|First Draft|




In this document, we explain the current workflow as well as the proposed workflow for GCP.

**Current Architecture:**

**Account Labs:**

We pre-create folders and projects and keep them in a pool. On each lab request, a new user is created and folders and projects are assigned to that user. After each stop, the project is deleted, and after each start, one project is picked from the pool and assigned to the user again.













**Lab, User, Folder and Project Mapping:**










**Actions:** 

**Start:** one project is picked from the pool and assigned to the user again.

**Stop:** The assigned project will be deleted.

**Clean Up:** Not supported.

**Delete:** We delete user and projects

**Policies:** Not supported.

**Billing:** Not supported.

**VM labs:** Not supported.


**New Proposal:**

**Account Labs:**

We pre-create only projects and keep them in a pool. There will be no folder concept, and the folder dependency will be removed. In each lab request, a new user is created and a project is assigned to that user. The user will be detached from project after each stop and attached after each start.











Lab, User and Project Mapping:




**Actions:**

**Start:** Attach the user to the project

**Stop:** Detach the user from the project

**Cleanup:** During the cleanup, all resources under the project will be deleted. 

**Delete:** We delete the user and delete all the resources under project. The project will be moved back to pools

**Policies:** 

Need more research.

**VM labs:**

A project will be imported into team, and all the VMs will be created under it.


















**Actions:**

`	`**Create:** The VM will be created.

**Stop:** The VM will be stopped.

**Start:** The VM will be started

**Delete:** The VM will be deleted

**Billing:**

**Account Labs:**

Our usage data is stored in BigQuery tables. Through APIs, data will be pulled from BigQuery and displayed in our portal.

**VM Labs:**

The usage will be in Mins and comes from Nuvelink audit logs.


`	`**Tasks**

1. Remove folders dependency
1. DB mappings
1. Start action
1. Stop action
1. Cleanup
1. Delete
1. Billing
1. Policies









Nuvepro Technologies Private Limited

No.26 (Old No. 686), 16th Main, 4th T Block, Jayanagar

Bangalore-560 041

INDIA

Email – <info@nuvepro.com> 

Web- [www.nuvepro.com](http://www.nuvepro.com)




This document is the exclusive property of Nuvepro Technologies Private Limited (Nuvepro). The recipient agrees that they will not copy, transmit, use, or disclose the confidential and proprietary information in this document by any means without the expressed and written consent of Nuvepro. 





© Nuvepro Technologies Private Limited 2021-20226
