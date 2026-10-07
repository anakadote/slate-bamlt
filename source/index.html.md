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

Welcome to the BAM LeadTracker JSON API. Requests are throttled at 60 per minute.

# Authentication

We use bearer authentication for all requests. Your **API Key** should be sent in the `Authorization` header using the `Bearer` keyword:

`Authorization: Bearer abcdefg12345678123456789`

# Leads

## Create a New Lead

```shell
curl -d '{"name":"John Doe","phone":"555 123 4567","email":"test@acme.com","postalCode":"12345"}' \
     -H "Authorization: Bearer abcdefg12345678123456789" \
     -H "Accept: application/json" \
     -H "Content-Type: application/json" \
     https://bamlt.com/api/leads
```

```php
$ch = curl_init('https://bamlt.com/api/leads');
curl_setopt($ch, CURLOPT_HTTPHEADER, [
  'Authorization: Bearer abcdefg12345678123456789',
  'Accept: application/json',
  'Content-Type: application/json',
]);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, '{"name":"John Doe","phone":"555 123 4567","email":"test@acme.com","postalCode":"12345"}');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_exec($ch);
```

> The above command returns a JSON 201 HTTP response, which includes the UUID of the new lead:

```json
{
  "uuid": "abcdefgh-4b83-4a7b-9292-1f7ca3e0f4a4"
}
```

This endpoint creates a new lead.

### HTTP Request

`POST https://bamlt.com/api/leads`

### Request Body

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
distributionMethod | no | string | The default value is `store`. You can set this to `client` to automatically distribute the lead to one of your stores by matching the submitted postal code to a postal code assigned to one of your stores. If no postal code match is found, then the lead will be given to your default store.
namePrefix | no | string | 
<b>name</b> | <b>yes*</b> | string | 
firstName | no* | string | 
lastName | no* | string | 
address | no | string | 
address2 | no | string | 
city | no | string | 
state | no | string | 
<b>postalCode</b> | <b>yes</b> | string | 
county | no | string | 
country | no | string | 2-character ISO country code.
email | no | string | 
<b>phone</b> | <b>yes</b> | string | Phone numbers are normalized to digits.
comments | no | string | 
interest | no | string | 
productInterest | no | string | e.g. “Water Softeners”.
leadSource | no | string | 
leadGenerator | no | string | 
leadType | no | string | 
mediaType | no | string | 
httpReferrer | no | string | The referring/source URL.
ipAddress | no | string | The submitter’s IP address.
userAgent | no | string | The submitter’s user agent.
assignTo | no | string | The email address of a user to be auto-assigned to the lead.
customerType | no | string | e.g. “Contractor” or “Designer”.
disposition | no | string | Set to `closed` to immediately close the lead.
businessName | no | string | 
businessAddress | no | string | 
businessAddress2 | no | string | 
businessCity | no | string | 
businessState | no | string | 
businessPostalCode | no | string | 
businessCountry | no | string | 2-character ISO country code.
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
<b>appointment.datetime</b> | <b>yes, with appointment</b> | string | The appointment’s date and time in ISO 8601 format (e.g. 2025-02-05T05:54:56Z).

*Either the `name` OR the `firstName` AND `lastName` parameters are required.

## Retrieve Leads

```shell
curl -H "Authorization: Bearer abcdefg12345678123456789" \
     -H "Accept: application/json" \
     https://bamlt.com/api/leads
```

```php
$ch = curl_init('https://bamlt.com/api/leads');
curl_setopt($ch, CURLOPT_HTTPHEADER, [
  'Authorization: Bearer abcdefg12345678123456789',
  'Accept: application/json',
]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
$response = curl_exec($ch);
```

> The above command returns a JSON 200 HTTP response:

```json
{
  "data": [
    {
      "uuid": "abcdefgh-4b83-4a7b-9292-1f7ca3e0f4a4",
      "firstName": "Sample",
      "lastName": "Lead",
      "accountNumber": null,
      "email": "sample@email.com",
      "phone": "+14125551212",
      "address": null,
      "address2": null,
      "city": null,
      "state": null,
      "postalCode": "15212",
      "assignee": {
        "firstName": "Sample",
        "lastName": "Assignee",
        "email": "sample@email.com"
      },
      "status": "Pending",
      "createdAt": "2026-09-25T14:32:43.000000Z",
      "updatedAt": "2026-09-25T14:32:43.000000Z",
      "appointments": [
        {
          "uuid": "stuvwxyz-4b83-4a7b-9292-1f7ca3e0f4a4",
          "datetime": "2026-09-28T18:00:00.000000Z",
          "duration": 90,
          "status": "Pending",
          "createdAt": "2026-09-26T15:05:30.000000Z",
          "updatedAt": "2026-09-26T15:05:30.000000Z"
        }
      ]
    }
  ],
  "meta": {
    "currentPage": 1,
    "from": 1,
    "lastPage": 1,
    "perPage": 250,
    "to": 1,
    "total": 1
  },
  "links": {
    "first": "https://bamlt.com/api/leads?page=1",
    "last": "https://bamlt.com/api/leads?page=1",
    "next": null,
    "prev": null
  }
}
```

This endpoint retrieves a paginated listing of leads along with their associated appointments.

### HTTP Request

`GET https://bamlt.com/api/leads`

### Query Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
page | no | integer | The page of results to return. Defaults to 1. Use `meta.lastPage` or `links.next` to page through results.
perPage | no | integer | The number of results per page. Defaults to 250. Maximum 250.
dateStart | no | string | Return leads created on or after this date (`YYYY-MM-DD`, e.g. `2026-01-01`). Dates are in UTC.
dateEnd | no | string | Return leads created on or before this date (`YYYY-MM-DD`, e.g. `2026-12-31`). Must not be earlier than dateStart. Dates are in UTC.
updatedStart | no | string | Return leads last updated at or after this time. An ISO 8601 timestamp with a timezone (e.g. `2026-01-01T00:00:00Z` or `2026-01-01T07:00:00-05:00`).
updatedEnd | no | string | Return leads last updated at or before this time. Same format as `updatedStart`, and must not be earlier than it.
phone | no | string | Filter leads by a phone number in E.164 format (e.g. `+14125551212`).

A lead's `updatedAt` changes only when the lead itself is updated. Changes to its appointments are not included; use the appointments endpoint's `updatedStart` to find those.

# Appointments

## Retrieve Appointments

```shell
curl -H "Authorization: Bearer abcdefg12345678123456789" \
     -H "Accept: application/json" \
     https://bamlt.com/api/appointments
```

```php
$ch = curl_init('https://bamlt.com/api/appointments');
curl_setopt($ch, CURLOPT_HTTPHEADER, [
  'Authorization: Bearer abcdefg12345678123456789',
  'Accept: application/json',
]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
$response = curl_exec($ch);
```

> The above command returns a JSON 200 HTTP response:

```json
{
  "data": [
    {
      "uuid": "stuvwxyz-4b83-4a7b-9292-1f7ca3e0f4a4",
      "datetime": "2026-09-28T18:00:00.000000Z",
      "duration": 90,
      "status": "Pending",
      "createdAt": "2026-09-26T15:05:30.000000Z",
      "updatedAt": "2026-09-26T15:05:30.000000Z",
      "lead": {
        "uuid": "abcdefgh-4b83-4a7b-9292-1f7ca3e0f4a4",
        "firstName": "Sample",
        "lastName": "Lead",
        "accountNumber": null,
        "email": "sample@email.com",
        "phone": "+14125551212",
        "address": null,
        "address2": null,
        "city": null,
        "state": null,
        "postalCode": "15212",
        "assignee": {
          "firstName": "Sample",
          "lastName": "Assignee",
          "email": "sample@email.com"
        },
        "status": "Pending",
        "createdAt": "2026-09-25T14:32:43.000000Z",
        "updatedAt": "2026-09-25T14:32:43.000000Z",
      }
    }
  ],
  "meta": {
    "currentPage": 1,
    "from": 1,
    "lastPage": 1,
    "perPage": 250,
    "to": 1,
    "total": 1
  },
  "links": {
    "first": "https://bamlt.com/api/appointments?page=1",
    "last": "https://bamlt.com/api/appointments?page=1",
    "next": null,
    "prev": null
  }
}
```

This endpoint retrieves a paginated listing of appointments along with their associated lead.

### HTTP Request

`GET https://bamlt.com/api/appointments`

### Query Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
page | no | integer | The page of results to return. Defaults to 1. Use `meta.lastPage` or `links.next` to page through results.
perPage | no | integer | The number of results per page. Defaults to 250. Maximum 250.
dateStart | no | string | Return appointments scheduled on or after this date (`YYYY-MM-DD`, e.g. `2026-01-01`). Dates are in UTC.
dateEnd | no | string | Return appointments scheduled on or before this date (`YYYY-MM-DD`, e.g. `2026-12-31`). Must not be earlier than dateStart. Dates are in UTC.
updatedStart | no | string | Return appointments last updated at or after this time. An ISO 8601 timestamp with a timezone (e.g. `2026-01-01T00:00:00Z` or `2026-01-01T07:00:00-05:00`).
updatedEnd | no | string | Return appointments last updated at or before this time. Same format as `updatedStart`, and must not be earlier than it.
phone | no | string | Filter appointments by a phone number in E.164 format (e.g. `+14125551212`).
