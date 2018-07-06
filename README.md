                                            MuleSoft API Implementation - RESTful API using RAML

TASK 1 - The RAML spec itself

SOLUTION DESIGN: I have designed resourceAPI.raml for customer object which can be extended for further objects like order, product etc. 
                 JSON schemas have been used for the objects as well as the samples. Basic Authentication, resourceTypes and traits have                  also been implemented. For better explanation of the API, I have implemented Customer as standalone (resourceTypes and                  traits have been explained with Order\Product).
                 The following functionalities have been implemented.
				 
				List customers - HTTP GET method, URI - /api/v1/customer
				Create a new customer - HTTP POST method, URI - /api/v1/customer
				Get Customer by firstName - HTTP GET method, URI - /api/v1/customer/{firstName}
				Update a customer - HTTP PUT method, URI -  /api/v1/customer/{firstName}
				Deletes a customer - HTTP DELETE method, URI - /api/v1/customer/{firstName}

TASK 2 - Commentary on the interaction of use case 1 with the API
			 
SOLUTION DESIGN: Retrieve List Customers functionality (HTTP GET method, URI - /api/v1/customer)

STEPS:
Step1: Consumer scheduler Job will send a request for retrieving all the customers by invoking HTTP get method
Step2: Request must contain the headers value Username & Password necessary for basic authentication
Step3: Authentication Error - If either the provided username and password combination is invalid or the user is not allowed to access
       the content provided by the requested URL, then Unauthorized HTTP error code '401' is sent back as error response.
Step4: Success - On success, this will provide the list of all the customers in the end provider system with HTTP status code '200' to the consuming system.
Step5: The consumer will synchronize the list of customer data to match the provider end system.
 
TASK 3 - Commentary on interaction of use case 2 with the API

SOLUTION DESIGN: Get Customer by firstName and then Update the Customer details(HTTP GET method and then invoke PUT method, URI - /api/v1/customer/{firstName})

STEPS:
Step1: Customer service representatives will search for a specific customer by providing the first Name of the Customer in his mobile application.
Step2: This will send a request for retrieving a customer data by firstName invoking HTTP get method
Step3: Request must contain the headers value Username & Password necessary for basic authentication
Step4: Authentication Error - If either the provided username and password combination is invalid or the user is not allowed to access
       the content provided by the requested URL, then Unauthorized HTTP error code '401' is sent back as error response.
Step5: Error - If there are no Customer data for the query, then a '404' HTTP error (Customer not found) is sent as error response.
Step6: Success - On success, this will provide the specific customer (searched) with HTTP status code '200' to the consuming system.
Step7: Then the Customer service representatives will make the necessary updates of the specific customer details displayed in mobile application.
Step8: This will send a request for updating the specific customer data invoking HTTP put method
Step9: Request must contain the headers value Username & Password necessary for basic authentication
Step10: Authentication Error - If either the provided username and password combination is invalid or the user is not allowed to access
        the content provided by the requested URL, then Unauthorized HTTP error code '401' is sent back as error response.
Step11: Error - If there are any error while updating the Customer data,  then a '500' HTTP error (Internal Server Error) is sent as error response. 
Step12: Success - On success, this will update the specific customer details in the end system and send the success message with HTTP status code 200 to the consuming system.
Step13: The Customer service representatives will then verify that the necessary updates done for the specific customer is displayed correctly in the mobile application.
                
TASK 4 - Commentary on how the API could be extended for use case 3

SOLUTION DESIGN: By using resourceTypes and traits, the API can be reused and extended to support future resources like order, products etc. collection resourceType is to represent a collection of items and item resourceType is to represent any single item. traits that I have used are hasRequestItem, hasResponseItem, hasResponseCollection and hasNotFound.

STEPS:
Step1: JSON Schemas and samples in separate file - I had separated the JSON schemas and sample in separate files and used the !include feature in RAML.
		types:    
		customer: !include schema/customer.json    
		order:    !include schema/order.json    
		product:  !include schema/product.json
		example: !include sample/customerExample.json
		example: !include sample/error.json
Step2: I had used resourceTypes and traits for future resource reusability.

		resourceTypes:
		  collection:
			usage: resourceType to represent a collection of items
			description: A collection of <<resourcePathName|!uppercamelcase>>
			get:
			  description: List all <<resourcePathName|!uppercamelcase>>        
			  is: [ hasResponseCollection: { typeName: <<typeName>> } ]

		traits:  
		hasResponseCollection:    
		responses:
			  200:        
				body:
				  application/json:            
				  type: <<typeName>>[]

		/product:  
		type: { collection: { typeName: product } }  
		get: 


TASK 5 - Commentary on any 'interesting' design decisions you made (and alternative options considered) 
SOLUTION DESIGN: I have designed resourceAPI.raml for customer object which can be extended for further objects like order, product etc. 

DECISION:
Decision1: Customer JSON schema created with only firstName, lastName and address (complex) elements.
Decision2: Separated the JSON schemas and sample in separate files and used the !include feature in RAML.
Decision3: Used resourceTypes (collection & item) and traits (hasRequestItem, hasResponseItem, hasResponseCollection and hasNotFound) 
           for future resource reusability.
Decision4: Used Basic Authentication (Username\Password) security features.
Decision5: For better explanation of the API, I have implemented Customer as stand alone. 
          resourceTypes and traits have been explained with Order & Product.
		  
		  
TASK 6 - Link to git repository where the code is stored along with the README.md

Link - https://github.com/bjdavids/RestAPI




