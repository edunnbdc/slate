---
title: Bill.com API Developer Portal

language_tabs:


toc_footers:
  - <a href='http://developer.bill.com/dev-program/sign-up'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Platform Overview

The Bill.com Developer Platform lets you interact with the Bill.com service through easy-to-use APIs, enabling you to automate the actions that a Bill.com user would perform through our application. This documentation walks you through various aspects of the platform, to get you started on building a high-quality integration with Bill.com. 

Whether you are a Marketplace, 3rd Party App or a Power User, our API will enable tight integration with Bill.com.  

You'll also know that you are connected to a solution trusted by over 800,000 members paying and receiving over $25 billion per year.  Bill.com is the leading Business Payments Network.  Bill.com is tightly synched with leading accounting software including Quickbooks, Xero, NetSuite, Intacct and more. 

All of these benefits of **Bill.com Payables** and more are supported via our API:

|     |     |
|-----|-----|
![No More Filing Cabinets](http://www.bill.com/sites/all/themes/bdc_bootstrap/images/features_chunk/Flying-Cabinet_180x150.png) **No More Filing Cabinets** | ![No more paperwork to shuffle](http://www.bill.com/sites/all/themes/bdc_bootstrap/images/features_chunk/No-More-Paper_180x150.png)**No more paperwork to shuffle**
![No more waiting on approvers](http://www.bill.com/sites/all/themes/bdc_bootstrap/images/features_chunk/Clock_180x180.png) **No more waiting on approvers** | ![No more vendor-go-round](http://www.bill.com/sites/all/themes/bdc_bootstrap/images/features_chunk/Roundabout_180x180.png) **No more vendor-go-round**

All of these benefits of **Bill.com Receivables** and more are supported via our API:

|     |     |
|-----|-----|
![You choose the payment method](http://www.bill.com/sites/all/themes/bdc_bootstrap/images/features_chunk/ACH-Credit-Card_180x180.png) **You choose the payment method** | ![We send the invoices for you](http://www.bill.com/sites/all/themes/bdc_bootstrap/images/features_chunk/Open-Mail_180x180.png) **We send the invoices for you**
![Bill.com sends automatic reminders by email](http://www.bill.com/sites/all/themes/bdc_bootstrap/images/features_chunk/Caution-Screen_180x180.png) **Bill.com sends automatic reminders by email** | ![Online payments go straight into your bank account](http://www.bill.com/sites/all/themes/bdc_bootstrap/images/features_chunk/Money-in-Pocket_180x180.png) **Online payments go straight into your bank account**

## MarketPlace
You are an online marketplace connecting one or more parties in an economic exchange.  You can power funds-transfer using Bill.com Payment APIs while eliminating risk related to storing user's bank account details.

![Bill.com Awards](https://github.com/edunnbdc/Images/blob/master/BDC%20Awards.png?raw=true)


## 3rd Party App
You need to develop data integration between Bill.com and your own application, for clients or as a white labeled solution.  

![Bill.com App Integrations](https://github.com/edunnbdc/Images/blob/master/Bill.com%20App%20Integrations.png?raw=true)

## Power User
You have a large volume of transactions.  You can automate Bill.com features and integrate with your other 3rd party platforms.

# API Mechanics

This section describes the mechanics of Bill.com API - the end points, request and response formats. 

##End Points
Login API requests always need to use the URL: `https://api.bill.com/api/v2/`

The subsequent calls should be made to the `apiEndPoint` (referred as <API End Point URL>  throughout this document) returned on the Login call.  All entities (exceptions noted) can be created, updated and read using the same set of CRUD (Create, Read, Update & Delete) operations. The API end point follows the format :  `<API End Point URL>/Crud/<Operation>/<Entity>.json` where Operation is one of the following: Create, Update, Read, Delete, Undelete. 

The response from the Bill.com API is JSON, which is described in the rest of this document. 

##Request Format
We recommend that all API requests are to be made using an http POST method. There are three required parameters that need to be passed with each call (except ListOrgs and Login)

 - devKey (or appkey) 
 - sessionId 
 - data


All the API parameters are to be passed as `POST` parameters.

Content-Type is to be set to `application/x-www-form-urlencoded`. Note that to upload a file to Bill.com Inbox or to attach a document to an object in Bill.com via `UploadAttachment` API, you must set the content-type to `multipart/form-data`. 

##Response Format
The Bill.com API responses always have the three name/value pairs

 - response_status : 0 for success, or 1 for error.
 - response_message: can be "Success" or "Error"
 - response_data: will have all the information returned, such as a list of records or any errors, etc. 

We may introduce additional values for the status and message in the future.

# Payables

## Create Vendor
The vendor object holds information about the companies and people your company pays for goods and services. If your vendors are part of the Bill.com network, meaning they have a Bill.com Receivables account, their bills will be delivered as eBills and will be paid electronically. If your vendors are not part of the Bill.com network, Bill.com either prints and mails checks to your vendors, or sends them an electronic payment. You can invite the vendors to set up electronic payments, through your Bill.com account or through Bill.com API.

###Request

    <API URL End Point>/Crud/Create/Vendor.json
    data={
       "obj" : {
          "entity" : "Vendor",
          "isActive" : "1",
          "name" : "Sample Vendor 1"
       }
    }

###Response 

    {
       "response_status" : 0,
       "response_message" : "Success",
       "response_data" : {
           "entity" : "Vendor",
           "id" : "00901FLEDIKRCILHfyui",
           "isActive" : "1",
           "name" : "Sample Vendor 1",
           "nameOnCheck" : null,
           "accNumber" : null,
           "taxId" : null,
           "track1099" : false,
           "address1" : null,
           "address2" : null,
           "address3" : null,
           "address4" : null,
           "addressCity" : null,
           "addressState" : null,
           "addressZip" : null,
           "addressCountry" : null,
           "email" : null,
           "fax" : null,
           "phone" : null,
           "payBy" : "0",
           "description" : null,
           "createdTime" : "2012-11-14T20:48:04.000+0000",
           "updatedTime" : "2012-11-14T20:48:04.000+0000"
       }
    }

## Create Bill
Creating a Bill is a little different than creating other Bill.com objects because bills contain one or more bill line items. The bill line items are contained in a list (similar to line items for vendor credits and invoices). When creating a Bill, each bill line item can be assigned to either an expense account or to an item. Here is an example where the first bill line item is assigned to an item and the second bill line item is assigned to an expense account (chart of account).
When you retrieve a Bill, the response contains all the bill attributes as well as attributes for each bill line item.
###Request 

    <API URL End Point>/Crud/Create/Bill.json
    data={
       "obj" : {
          "entity" : "Bill",
          "isActive" : "1",
          "vendorId" : "00901XZBIUHKIHNTeanq",
          "invoiceNumber" : "1234A",
          "invoiceDate" : "2012-11-14",
          "dueDate" : "2012-11-14",
          "glPostingDate" : "2012-11-14",
          "description" : "Bill description",
          "billLineItems" : [ {
             "entity" : "BillLineItem",
             "amount" : 200.00,
             "chartOfAccountId" : "0ca01QXZWLVTZBAD5bah",
             "departmentId" : "0de01BGWFJYOEBFRRd39",
             "locationId" : "loc01AZSDXOQCFZDP7nd",
             "description" : "Lineitem for expenses",
             "lineType" : "1",
             "actgClassId" : "cls01SIYVBONRTDX1u4t"
          }, {
             "entity" : "BillLineItem",
             "amount" : 400.00,
             "departmentId" : "0de01BGWFJYOEBFRRd39",
             "locationId" : "loc01AZSDXOQCFZDP7nd",
             "description" : "Line item with items",
             "lineType" : "2",
             "quantity" : 2,
             "unitPrice" : 200.00,
             "actgClassId" : "cls01SIYVBONRTDX1u4t"
          } ]
       }
    }

###Response 

    {
      "response_status" : 0,
      "response_message" : "Success",
      "response_data" : {
        "entity" : "Bill",
        "id" : "00n01ELUVVEVLWIYoslo",
        "isActive" : "1",
        "vendorId" : "00901XZBIUHKIHNTeanq",
        "invoiceNumber" : "1234A",
        "approvalStatus" : "1",
        "invoiceDate" : "2012-11-14",
        "dueDate" : "2012-11-14",
        "glPostingDate" : "2012-11-14",
        "amount" : 600.00,
        "paidAmount" : null,
        "paymentStatus" : "1",
        "description" : "Bill description",
        "createdTime" : "2013-10-28T04:41:49.000+0000",
        "updatedTime" : "2013-10-28T04:41:54.000+0000",
        "payFromBankAccountId" : "00000000000000000000",
        "payFromChartOfAccountId" : "00000000000000000000",
        "billLineItems" : [ {
          "entity" : "BillLineItem",
          "id" : "bli01TYXGJSTPPEMqqzs",
          "billId" : "00n01ELUVVEVLWIYoslo",
          "amount" : 200.00,
          "chartOfAccountId" : "0ca01QXZWLVTZBAD5bah",
          "departmentId" : "0de01BGWFJYOEBFRRd39",
          "locationId" : "loc01AZSDXOQCFZDP7nd",
          "jobId" : "00000000000000000000",
          "customerId" : "00000000000000000000",
          "jobBillable" : false,
          "description" : "Lineitem for expenses",
          "createdTime" : "2013-10-28T04:41:50.000+0000",
          "updatedTime" : "2013-10-28T04:41:53.000+0000",
          "lineType" : "1",
          "itemId" : "00000000000000000000",
          "quantity" : null,
          "unitPrice" : null,
          "employeeId" : "00000000000000000000",
          "actgClassId" : "cls01SIYVBONRTDX1u4t"
        }, {
          "entity" : "BillLineItem",
          "id" : "bli01OBVTAIGZUAJqqzt",
          "billId" : "00n01ELUVVEVLWIYoslo",
          "amount" : 400.00,
          "chartOfAccountId" : "00000000000000000000",
          "departmentId" : "0de01BGWFJYOEBFRRd39",
          "locationId" : "loc01AZSDXOQCFZDP7nd",
          "jobId" : "00000000000000000000",
          "customerId" : "00000000000000000000",
          "jobBillable" : false,
          "description" : "Line item with items",
          "createdTime" : "2013-10-28T04:41:51.000+0000",
          "updatedTime" : "2013-10-28T04:41:54.000+0000",
          "lineType" : "1",
          "itemId" : "00000000000000000000",
          "quantity" : 2,
          "unitPrice" : 200.00,
          "employeeId" : "00000000000000000000",
          "actgClassId" : "cls01SIYVBONRTDX1u4t"
        } ]
      }
    }


## Pay Bill
SAMPLE REQUEST:

<Endpoint>/api/v2/PayBills.json?sessionId=<sessionid>&devKey=<devKey>

# Receivables

## Create Customer
SAMPLE REQUEST:

<API URL End Point>/Crud/Create/Customer.json?sessionId=<sessionid>&devKey=<devKey>

## Create Invoice Record

SAMPLE REQUEST:

<API URL End Point>/Crud/Create/Invoice.json?sessionId=<sessionid>&devKey=<devKey>

## Send the Invoice

SAMPLE REQUEST:

<API URL End Point>/SendInvoice.json?sessionId=<sessionid>&devKey=<devKey>

## Charge the Customer

SAMPLE REQUEST:

<API URL End Point>/Crud/Read/ReceivedPay.jsonsessionId=<sessionid>&devKey=<devKey>

# API Reference

## Approval Policy

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Approval Policy Approver


This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Bank Account


This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Bill

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve


## Bill Payment

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## All the other Entities from the API Reference....

