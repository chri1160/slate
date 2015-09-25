---
title: Swoogo API Docs

language_tabs:
  - php

toc_footers:
  - <a href="https://www.swoogo.com/site/login">Log in to Swoogo</a>
  - <a href="https://github.com/chri1160/swoogo-api">Swoogo API on GitHub</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Swoogo API! You can use our API to access Swoogo API endpoints, which can get information on events, registrations, sessions and more.
Swoogo uses API keys to allow access to the API. You can generate or re-generate your API key by logging in to the app and visiting Account Setup > API.

# Authentication

> If using the Swoogo API class:

```php
$api = new SwoogoApi('CONSUMER_KEY', 'CONSUMER_SECRET');
```

> Make sure to replace `CONSUMER_KEY` AND `CONSUMER_SECRET` with your actual API credentials.

<b>Step 1: Encode consumer key and secret</b>

The steps to encode your consumer key and secret into a set of credentials to obtain a bearer token are:

1) URL encode the consumer key and the consumer secret according to RFC 1738.<br />
2) Concatenate the encoded consumer key, a colon character “:”, and the encoded consumer secret into a single string.<br />
3) Base64 encode the string from the previous step.

<table>
<tr>
<td>
Consumer Key:<br />xvz1evFS4wEEPTGEFPHBog<br /><br />
Consumer Secret:<br />L8qq9PZyRg6ieKGEKhZolGC0vJWLw8iEJ88DRdyOg<br /><br />
Concatenated:<br />xvz1evFS4wEEPTGEFPHBog:L8qq9PZyRg6ieKGEKhZolGC0vJWLw8iEJ88DRdyOg<br /><br />
Base 64 Encoded:<br />eHZ6MWV2RlM0d0VFUFRHRUZQSEJvZzpMOHFxOVBaeVJnNmllS0dFS2hab2xHQzB2SldMdzhpRUo4OERSZHlPZw==
</td>
</tr>
</table>

<br /><b>Step 2: Obtain a bearer token</b>

The value calculated in step 1 must be exchanged for a bearer token by issuing a request to POST oauth2 / token:

- The request must be a HTTP POST request.<br />
- The request must include an Authorization header with the value of your base 64 encoded token (from step 1).<br />
- The request must include a Content-Type header with the value of application/x-www-form-urlencoded;charset=UTF-8.<br />
- The body of the request must be grant_type=client_credentials.<br />

Example request:

<table>
<tr>
<td>
POST /api/v1/oauth2/token HTTP/1.1<br />
Host: www.swoogo.com<br />
Authorization: Basic YOUR_BASE_64_ENCODED_TOKEN<br />
Content-Type: application/x-www-form-urlencoded;charset=UTF-8<br />
Content-Length: 29<br /><br />
grant_type=client_credentials
</td>
</tr>
</table>

<br /><b>Step 3: Authenticate API requests with the bearer token</b>

The bearer token may be used to issue requests to Swoogo API endpoints. To use the bearer token, construct a normal HTTPS request and include an Authorization header with the value of your bearer token from step 2.

Example request:

<table>
<tr>
<td>
GET /api/v1/events HTTP/1.1<br />
Host: www.swoogo.com<br />
Authorization: Bearer YOUR_BEARER_TOKEN<br />
</td>
</tr>
</table>

# Events
## Get All Events

```php
$response = $api->request('events', array('fields' => 'id,name,start_date'));
```

> The above command returns JSON structured like this:

```json
[
    {
        "id": 1,
        "name": "International Conference",
        "start_date": "2016-02-10"
    },
    {
        "id": 2,
        "name": "Members Meeting",
        "start_date": "2015-11-25"
    }
]
```

This endpoint retrieves all events.

### HTTP Request

`GET https://www.swoogo.com/api/v1/events`

### Parameters

Field | Type | Description
--------- | ------- | -----------
fields | string | Comma separated list of fields you want to return
search | string | Comma separated list of search filters you want to apply


## Get A Specific Event
```php
$response = $api->request('events/1');
```

> The above command returns JSON structured like this:

```json
{
    "id": 1,
    "folder_id": 0,
    "status": "draft",
    "name": "International Conference",
    "url": "international-conference",
    "hashtag": "",
    "description": "",
    "organizer_id": 1,
    "location": {
        "id": 2,
        "name": "Hilton Reading",
        "line_1": " Drake Way",
        "line_2": "",
        "line_3": "",
        "city": "Reading",
        "state": "England",
        "zip": "RG2 0GQ",
        "country": {
            "code": "GB",
            "name": "United Kingdom",
            "continent": "Europe"
        },
        "phone": "+44 118 916 9000",
        "website": "http:\/\/www.placeshilton.com\/reading",
        "latitude": "51.430484",
        "longitude": "-0.9775939999999537",
        "created_at": "2015-09-20 09:07:19",
        "updated_at": "2015-09-23 15:38:35"
    },
    "start_date": "2016-02-10",
    "start_time": null,
    "end_date": null,
    "end_time": null,
    "timezone": "Europe\/London",
    "capacity": null,
    "target_attendance": null,
    "type_id": "Conference",
    "created_at": "2015-09-20 09:07:19",
    "updated_at": "2015-09-23 15:38:35"
}
```

This endpoint retrieves a specific Event
### HTTP Request

`GET https://www.swoogo.com/api/v1/events/<ID>`


