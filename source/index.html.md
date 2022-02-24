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

We use basic authentication for all requests.

The username should be “api” and the password is your API key.

# Leads

## Create a new Lead

```shell
curl -u api:your-api-key \
  -d '{"name":"John Doe","phone":"555 123 4567","email":"test@acme.com","postalCode":"12345"}' \
  -H "Content-Type: application/json" \ 
  http://bamlt.test/api/lead
```

```php
$ch = curl_init('https://bamlt.com/api/lead');
curl_setopt($ch, CURLOPT_HTTPHEADER, [
  'Accept: application/json',
  'Content-Type: application/json',
  'Authorization: Basic abcdefg12345678123456789',
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
distributionMethod | no | string | The default value is “store”. You can set this to “client” to automatically distribute the lead to one of your stores by matching the submitted postal code to a postal code assigned to one of your stores.
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
leadSource | no | string | 
mediaType | no | string | 
httpReferrer | no | string | The referring/source URL
businessName | no | string | 
businessAddress | no | string | 
businessAddress2 | no | string | 
businessCity | no | string | 
businessState | no | string | 
businessPostalCode | no | string | 
businessCountry | no | string | 2-character ISO country code

*Either the “name” OR the “firstName” AND “firstName” parameters are required.
