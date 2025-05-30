# Build an HRIS integration

The following getting started guide outlines the steps to build an HRIS integration that synchronizes employee data between your system and Visma.
When a new employee joins your organization, you collect their information and use the Nmbrs API to store it in Visma.
Later, if the employee updates any personal details, those changes can be synced seamlessly with Visma via the same API.

## Prerequisites
Before you proceed, you must:
* Sign up for an account on the [Nmbrs developer portal.](https://developer.nmbrs.com/)
* Create a web or a mobile app through [My Integrations](https://partner-portal.nmbrs.com/integrations) tab and get the Client Id and the Client Secret to call APIs. Ensure you store the Client Secret credentials at a safe location in your system since it's displayed only once when you 
  create it.
* Choose one of the subscription plans on [Products](https://developer.nmbrs.com/products) page on the Nmbrs developer portal. Ensure you store the subscription id since you need to send it in the request body for API calls.
* Set up authentication correctly. For more information on how to set up authentication, see [How to authenticate.](https://nmbrs.stoplight.io/docs/nmbrs-restapi/e9e0f5292b4a1-authentication) If you want to do a quick test without credentials, you can use the [Mock Server](https://nmbrs.stoplight.io/docs/nmbrs-restapi/9c14f1c024642-getting-started#mock-server). 

## Required endpoints
The synchronisation of the employee data between your system and Visma requires the following endpoints:

* Create employee
* Update employee personal info

Base URL for the endpoints - `https://api.nmbrsapp.com`

## Create employee

Use the Create endpoint to add a new employee's information to Visma. You can include basic details, birth information, and contact data. The API supports storing records for both applicants and payroll employees.

### Sample request

```POST /api/companies/{companyId}/employees```

```json
{
  "request": {
    "method": "POST",
    "path": "/companies/{companyId}/employees",
    "headers": {
      "Content-Type": "application/json",
      "X-Subscription-Key": "7b92603e-77ed-4896-8e78-5dea2050476a",
      "Authorization": "Authorization: Bearer 123"
    },
    "data": {
      "PersonalInfo": {
        "personalInfoId": "0039b188-b5f5-49a6-a3d5-0448ce4042ee",
        "basicInfo": {
          "employeeNumber": 98072,
          "firstName": "John",
          "firstNameInFull": "string",
          "prefix": "van der",
          "initials": "string",
          "lastName": "Doe",
          "employeeType": "applicant"
        },
        "birthInfo": {
          "birthDate": "1980-02-27",
          "birthCountryCodeISO": "NL",
          "nationalityCodeISO": "PT",
          "deceasedOn": "1980-02-27",
          "gender": "unspecified"
        },
        "contactInfo": {
          "privateEmail": "doe@private.com",
          "businessEmail": "doe@business.com",
          "businessPhone": "+351222222",
          "businessMobilePhone": "+351222222",
          "privatePhone": "+351222222",
          "privateMobilePhone": "+351222222",
          "otherPhone": "+351222222"
        },
        "partnerInfo": {
          "partnerPrefix": "string",
          "partnerName": "string",
          "ascriptionCode": 0
        },
        "period": {
          "year": 2021,
          "period": 4
        },
        "createdAt": "2021-07-01T10:15:08Z"
      },
      "AdditionalEmployeeInfo": {
        "inServiceDate": "2019-08-24",
        "defaultEmployeeTemplate": "string"
      }
    }
  }
}

```
* Replace the value for the header `Authorization` with your authorisation bearer token.
* Replace the value for the header `X-Subscription-Key: ` with your subscription id. For more information, see [Prerequisites.](#Prerequisites)
  * Provide all the information about the specific employee in the `PersonalInfo` object. This object can be stored for both employee types - `applicant` and `employee`.
    * Replace the `personalInfoId` with a Version 4 UUID generated through your code.
    * Provide the basic, birth, and contact information in the `basicInfo`, `birthInfo`, and `contactInfo` objects respectively.
* Provide all the information about the payroll employees in the `AdditionalEmployeeInfo` object if the `employeeType` is `employee`.

For more information on the fields in the request, see the complete [request body.](https://nmbrs.stoplight.io/docs/nmbrs-restapi/13c6a8d9c7190-create-employee#request-body)

### Sample response

A successful JSON response returns a unique employee UUID created by the API for the new employee.

```json
{
  "employeeId": "d30ec597-bd29-453e-9613-7786297eedcc"
}
```

For all other response types returned by the endpoint, see [Response codes for create employee.](https://nmbrs.stoplight.io/docs/nmbrs-restapi/13c6a8d9c7190-create-employee#Responses)

## Update employee personal info

Use this endpoint to update the basic, birth, or the contact information for an employee.

### Sample request
``` PUT /api/employees/{employeeId}/personalInfo```

```json
{
  "request": {
    "method": "PUT",
    "path": "/employees/{employeeId}/personalInfo",
    "headers": {
      "Content-Type": "application/json",
      "X-Subscription-Key": "7b92603e-77ed-4896-8e78-5dea2050476a",
      "Authorization": "Authorization: Bearer 123"
    },
    "data": {
      "basicInfo": {
        "employeeNumber": 98072,
        "firstName": "John",
        "firstNameInFull": "string",
        "prefix": "van der",
        "initials": "string",
        "lastName": "Doe",
        "employeeType": "applicant"
      },
      "birthInfo": {
        "birthDate": "2019-08-24T14:15:22Z",
        "birthCountryCodeISO": "NL",
        "nationalityCodeISO": "NL",
        "deceasedOn": "2019-08-24T14:15:22Z",
        "gender": "unspecified"
      },
      "contactInfo": {
        "privateEmail": "doe@private.com",
        "businessEmail": "doe@business.com",
        "businessPhone": "+351222222",
        "businessMobilePhone": "+351222222",
        "privatePhone": "+351222222",
        "privateMobilePhone": "+351222222",
        "otherPhone": "+351222222"
      },
      "partnerInfo": {
        "partnerPrefix": "string",
        "partnerName": "string",
        "ascriptionCode": 0
      },
      "period": {
        "year": 2021,
        "period": 4
      }
    }
  }
}
```
* Replace the value for the header `Authorization` with your authorisation bearer token.
* Replace the value for the header `X-Subscription-Key: ` with your subscription id. For more information, see [Prerequisites.](#Prerequisites)
* Replace the path parameter `employeeId` with the UUID that you received in response after calling the [Create employee](#create-employee) endpoint. This UUID uniquely defines the employee in Visma.
* Replace the `employeeNumber` to the unique employee number generated by your system.
* Provide the updated basic, birth, and contact information in the `basicInfo`, `birthInfo`, and `contactInfo` objects respectively.

For more information on the fields in the request, see the complete [request body.](https://nmbrs.stoplight.io/docs/nmbrs-restapi/e12e45d11695c-update-employee-personal-info#request-body)

### Sample response

A successful JSON response returns the personal Info UUID that you generated when you invoked the [Create employee](#create-employee) endpoint.

```json
{
  "personalInfoId": "756bd0de-fc2f-4b7c-b7ba-d3a3f5360a5b"
}
```
For all other response types returned by the endpoint, see [Response codes for update employee personal info.](https://nmbrs.stoplight.io/docs/nmbrs-restapi/e12e45d11695c-update-employee-personal-info#Responses)

