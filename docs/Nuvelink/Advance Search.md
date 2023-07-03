Feature Name





Advance Search

Feature Details Document	




|Revision|Date|Author|Remarks|
| :- | :- | :- | :- |
|1|31/01/2022|Manjunath R|First Draft|



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


|Feature Name|Advance Search|
| :- | :- |
|Microservice name|All Services|
|Database (yes/no)|No|
|Public APIs (yes/no)|No|
|Complexity|Medium|

# <a name="_toc94016945"></a>Requirements Specifications

Get All API in V2 was designed to fetch all the details for the requested entity or resource. The main drawback of these APIs is that they will fetch all the details based on the pagination but will not support the search or filter functionality. The main requirement is to implement the advanced search which should support for all the existing Get All APIs with the minimum code change

This feature support the below listed functionalities

1. Equal
1. Negation
1. IN: This is used to search based on the list of data provided with the selected delimiter
1. Greater Than
1. Greater Than or Equal
1. Lower Than
1. Lower Than or Equal
1. Starts With
1. Ends With
1. Contains 

# <a name="_toc94016946"></a>Solution Overview	

This goal can be achieved using the Spring Criteria API. 

Following are the steps to 

1. Receive the advance search request in the Controller as the request body
1. Forward the request to the service layer
1. Use the Specification Builder to validate and build the specification dynamically using the request
1. Extend the JpaSpecificationExecutor in the repository
1. Pass the dynamically build specification to the criteria API find All method
# <a name="_toc94016947"></a>Modules

# <a name="_toc94016948"></a>APIs
# <a name="_toc94016949"></a>Scope
# <a name="_toc94016950"></a>Use cases
# <a name="_toc94016951"></a>Assumptions












Nuvepro Technologies Private Limited

No.26 (Old No. 686), 16th Main, 4th T Block, Jayanagar

Bangalore-560 041

INDIA

Email – <info@nuvepro.com> 

Web- [www.nuvepro.com](http://www.nuvepro.com)




This document is the exclusive property of Nuvepro Technologies Private Limited (Nuvepro). The recipient agrees that they will not copy, transmit, use, or disclose the confidential and proprietary information in this document by any means without the expressed and written consent of Nuvepro. 





© Nuvepro Technologies Private Limited 2021-20226
