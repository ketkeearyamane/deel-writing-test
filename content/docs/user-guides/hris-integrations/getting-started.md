# Build an HRIS integration

The following getting started guide describes steps to build a HRIS integration. This integration helps you synchronise the employee data between two different systems.

## Prerequisites
Before you proceed, you must:
* Sign up for an account on the [Nmbrs developer portal.](https://developer.nmbrs.com/)
* Create a web or a mobile app through <b>My Integrations<b> tab and get the Client Id and the Client Secret to call APIs. Ensure you store the Client Secret credentials at a safe location in your system since it's displayed only once when you create it.
* Choose one of the subscription plans on [Products](https://developer.nmbrs.com/products) page on the Nmbrs developer portal. Ensure you store the subscription id since you need to send it in the request body for API calls.
* Set up authentication correctly. For more information on how to set up authentication, see [How to authenticate.](https://nmbrs.stoplight.io/docs/nmbrs-restapi/e9e0f5292b4a1-authentication)

## Required endpoints
The synchronisation of the employee data between two systems requires the following endpoints:

* Create employee
* [Update employee personal info](https://nmbrs.stoplight.io/docs/nmbrs-restapi/e12e45d11695c-update-employee-personal-info)

## Create employee

Use the create endpoint to add a new employee's information to our system.

### Sample request

```json
curl --request POST \
  --url https://api.nmbrsapp.com/api/companies/{companyId}/employees \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer 123' \
  --header 'Content-Type: application/json' \
  --header 'X-Subscription-Key: ' \
  --data '{
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
}'

```
* Replace `Authorization: Bearer 123` with your authorisation bearer token.
* Replace `X-Subscription-Key: ` with your subscription id. For more information, see [Prerequisites.](#Prerequisites)
* Provide all the information about the specific employee in the `PersonalInfo` object.
  * Replace the `personalInfoId` with a Version 4 UUID generated through your code.
  * Provide the basic, birth, and contact information in the `basicInfo`, `birthInfo`, and `contactInfo` objects respectively.
* Provide all the information about the payroll employees in the `AdditionalEmployeeInfo` object.

For more information on the fields in the request, see the complete [request body.](https://nmbrs.stoplight.io/docs/nmbrs-restapi/13c6a8d9c7190-create-employee#request-body)

### Sample response

A successful response returns a unique employee UUID created by the API for the new employee.

```json
{
  "employeeId": "d30ec597-bd29-453e-9613-7786297eedcc"
}
```

For all other response types returned by the endpoint, see [Response codes for create employee.](https://nmbrs.stoplight.io/docs/nmbrs-restapi/13c6a8d9c7190-create-employee#Responses)

## Update employee personal info

Use this endpoint to update either the basic, birth, or the contact information for an employee.

### Sample request

```json
curl --request PUT \
--url https://api.nmbrsapp.com/api/employees/{employeeId}/personalInfo \
--header 'Accept: application/json' \
--header 'Authorization: Bearer 123' \
--header 'Content-Type: application/json' \
--header 'X-Subscription-Key: ' \
--data '{
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
}'
```
* Replace `Authorization: Bearer 123` with your authorisation bearer token.
* Replace `X-Subscription-Key: ` with your subscription id. For more information, see [Prerequisites.](#Prerequisites)
* Provide the updated basic, birth, and contact information in the `basicInfo`, `birthInfo`, and `contactInfo` objects respectively.

### Sample response

The response returns the personal Info UUID that you generated when you invoked the [Create employee](#create-employee) endpoint.

```json
{
  "personalInfoId": "756bd0de-fc2f-4b7c-b7ba-d3a3f5360a5b"
}
```
For all other response types returned by the endpoint, see[Response codes for update employee personal info.](nmbrs.stoplight.io/docs/nmbrs-restapi/e12e45d11695c-update-employee-personal-info#Responses)

