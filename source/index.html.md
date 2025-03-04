---
title: BAM LeadTracker JSON API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - php

toc_footers:
  - <a href='https://bamadv.com/contact-us/'>Contact us for an API Key</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the BAM LeadTracker JSON API! You can use our API to submit new leads to the BAM LeadTracker.

You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

We use bearer authentication for all requests. Your **API Key** should be sent in the `Authorization` header using the `Bearer` keyword:

`Authorization: Bearer abcdefg12345678123456789`

# Leads

## Create a new Lead

```shell
curl -d '{"name":"John Doe","phone":"555 123 4567","email":"test@acme.com","postalCode":"12345"}' \
     -H "Authorization: Bearer abcdefg12345678123456789" \
     -H "Accept: application/json" \
     -H "Content-Type: application/json" \
     https://bamlt.com/api/lead
```

```php
$ch = curl_init('https://bamlt.com/api/lead');
curl_setopt($ch, CURLOPT_HTTPHEADER, [
  'Authorization: Bearer abcdefg12345678123456789',
  'Accept: application/json',
  'Content-Type: application/json',
]);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, '{"name":"John Doe","phone":"555 123 4567","email":"test@acme.com","postalCode":"12345"}');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_exec($ch);
curl_close($ch);
```

> The above command returns a JSON 201 HTTP response, which includes the uuid of the new lead:

```json
{
  "uuid": "abcdefg-1234-12345-1231-1231-xyz123"
}
```

This endpoint creates a new lead.

### HTTP Request

`POST https://bamlt.com/api/lead`

### Query Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
distributionMethod | no | string | The default value is “store”. You can set this to “client” to automatically distribute the lead to one of your stores by matching the submitted postal code to a postal code assigned to one of your stores. If no postal code match is found, then the lead will be given to your default store.
namePrefix | no | string | 
<b>name</b> | <b>yes*</b> | string | 
firstName | no* | string | 
lastName | no* | string | 
address | no | string | 
address2 | no | string | 
city | no | string | 
state | no | string | 
<b>postalCode</b> | <b>yes</b> | string | 
country | no | string | 2-character ISO country code
email | no | string | 
<b>phone</b> | <b>yes</b> | string | 
comments | no | string | 
interest | no | string | 
leadSource | no | string | 
leadGenerator | no | string | 
leadType | no | string | 
mediaType | no | string | 
httpReferrer | no | string | The referring/source URL
assignTo | no | string | The email address of a user to be auto-assigned to the lead
customerType | no | string | Ex: "Contractor" or "Designer"
disposition | no | string | Set to "closed" to immediately close the lead
businessName | no | string | 
businessAddress | no | string | 
businessAddress2 | no | string | 
businessCity | no | string | 
businessState | no | string | 
businessPostalCode | no | string | 
businessCountry | no | string | 2-character ISO country code
smsConsent | no | boolean | 
vendorLeadId | no | string | 
vendorProduct | no | string | 
vendorScheduleDate | no | string | 
vendorScheduleTime | no | string | 
vendorServiceRequestReason | no | string | 
vendorTrackingNumber | no | string | 
utm | no | object | 
utm.gclid | no | string | 
utm.utm_source | no | string | 
utm.utm_medium | no | string | 
utm.utm_term | no | string | 
utm.utm_campaign | no | string | 
utm.utm_content | no | string | 
utm.utm_keyword | no | string | 
appointment | no | object | This is an opt-in parameter. Please contact BAM to enable.
<b>appointment.dateTime</b> | <b>yes, with appointment</b> | string | The appointment's date and time in ISO 8601 format. Ex: 2025-02-05T05:54:56Z

*Either the “name” OR the “firstName” AND “lastName” parameters are required.
