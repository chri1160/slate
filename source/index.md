---
title: Swoogo API Docs

language_tabs:
  - php

toc_footers:
  - <a href="https://www.swoogo.com/site/login">Log in to Swoogo</a>
  - <a href="https://github.com/chri1160/swoogo-api">Swoogo API PHP class on GitHub</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Swoogo API! You can use our API to access Swoogo API endpoints, which can get information on events, registrations, sessions and more.
Swoogo uses API keys to allow access to the API. You can generate or re-generate your API key for a particular account and user by logging in to the app and visiting Profile > API Credentials.
<aside class="notice">In order to ensure that the API remains accessible to all users, we have a rate limit of 200 requests in every 10 minute period.</aside>

# Authentication

> Using Curl and PHP:

```php
$consumerKey = urlencode('CONSUMER_KEY');
$consumerSecret = urlencode('CONSUMER_SECRET');
$ch = curl_init();
curl_setopt($ch, CURLOPT_USERPWD, $consumerKey.':'.$consumerSecret);
curl_setopt($ch, CURLOPT_URL, 'https://www.swoogo.com/api/v1/oauth2/token.json');
curl_setopt($ch, CURLOPT_POSTFIELDS, 'grant_type=client_credentials');
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$result = json_decode(curl_exec($ch));
$accessToken = $result->access_token;
```

> To use the Swoogo API class, simply include the class and use:

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

The value calculated in step 1 must be exchanged for a bearer token by issuing a request to POST /api/v1/oauth2/token.json:

- The request must be a HTTP POST request.<br />
- The request must include an Authorization header with the value of your base 64 encoded token (from step 1).<br />
- The request must include a Content-Type header with the value of application/x-www-form-urlencoded;charset=UTF-8.<br />
- The body of the request must be grant_type=client_credentials.<br />

Example request:

<table>
<tr>
<td>
POST /api/v1/oauth2/token.json HTTP/1.1<br />
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
GET /api/v1/events.json HTTP/1.1<br />
Host: www.swoogo.com<br />
Authorization: Bearer YOUR_BEARER_TOKEN<br />
</td>
</tr>
</table>

# Events

## Get All Events

```php
$response = $api->request('https://www.swoogo.com/api/v1/events.json', array(
     'fields' => 'id,name,start_date'
));
```

> The above command returns JSON structured like this:

```json
{
    "items": [
        {
            "id": 1,
            "name": "International Conference",
            "start_date": "2015-10-26"
        },
        {
            "id": 2,
            "name": "Member Meeting",
            "start_date": "2015-11-12"
        }
    ],
    "_links": {
        "self": {
            "href": "events.json?fields=id%2Cname%2Cstart_date&page=1&per-page=200"
        }
    },
    "_meta": {
        "totalCount": 2,
        "pageCount": 1,
        "currentPage": 1,
        "perPage": 200
    }
}
```
This endpoint retrieves all events.

### HTTP Request

`GET https://www.swoogo.com/api/v1/events.json`

### Parameters

These are the parameters you can pass to the API when calling this function.

Parameter | Type | Required? | Default | Description
--------- | ------- | ----------- | ----------- | -----------
fields | string | Optional | id,name | Comma separated list of fields you want to return
expand | string | Optional | null | Comma separated list of related info you want to return
search | string | Optional | null | Comma separated list of search filters you want to apply, eg. name=Member Meeting,start_date=2015-11-12
page | integer | Optional | null | The page of results you want to view
per-page | integer | Optional | null | The number of results per page (to a max of 200)


## Get A Specific Event

```php
$response = $api->request('https://www.swoogo.com/api/v1/events/1.json');
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
    "start_date": "2016-02-10",
    "start_time": null,
    "end_date": null,
    "end_time": null,
    "timezone": "Europe\/London",
    "capacity": null,
    "target_attendance": null,
    "created_at": "2015-09-20 09:07:19",
    "updated_at": "2015-09-23 15:38:35"
}
```

This endpoint retrieves a specific Event.

### HTTP Request

`GET https://www.swoogo.com/api/v1/events/ID.json`

### Parameters

These are the parameters you can pass to the API when calling this function.

Parameter | Type | Required? | Default | Description
--------- | ------- | ----------- | ----------- | -----------
fields | string | Optional | null | Comma separated list of fields you want to return
expand | string | Optional | null | Comma separated list of related info you want to return



## Available Event Fields

These are the standard fields that the API can return for events. Please note that custom fields added for specific accounts or events are not listed here.

Name | Type | Description
--------- | ------- | -----------
id|integer|Unique ID for the event
folder_id|integer|The folder ID the event is in
status|string|The event's status
name|string|The name of the event
url|string|The URL slug identifying this event
hashtag|string|The hashtag being used for the event
description|string|The event description
organizer_id|integer|The ID of the person organizing the event
start_date|string|The date the event starts
start_time|string|The time the event starts
end_date|string|The date the event ends
end_time|string|The time the event ends
timezone|string|The timezone the event is taking place in
capacity|integer|The maximum number of people who can register for the event
target_attendance|integer|The number of registrations the organizer was targeting
created_at|string|The date and time the event was created
updated_at|string|The date and time the event was updated

### Extra fields

This is the extra information that can be returned if required by specifying what you need in the expand variable.

Name | Type | Description
--------- | ------- | -----------
location|object|The location where the event is taking place


# Questions

## Get All Questions

```php
$response = $api->request('https://www.swoogo.com/api/v1/questions.json', array(
     'event_id' => 1,
     'fields' => 'id,name,type'
));
```

> The above command returns JSON structured like this:

```json
{
    "items": {
        "1": {
            "id": 1,
            "type": "email",
            "name": "Email Address"
        },
        "2": {
            "id": 2,
            "type": "textInput",
            "name": "First Name"
        },
        "3": {
            "id": 3,
            "type": "textInput",
            "name": "Last Name"
        },
        "4": {
            "id": 4,
            "type": "textInput",
            "name": "Company"
        }
    },
    "_links": {
        "self": {
            "href": "https:\/\/local.swoogo.com\/api\/v1\/questions.json?fields=id%2Cname%2Ctype&page=1"
        }
    },
    "_meta": {
        "totalCount": 4,
        "pageCount": 1,
        "currentPage": 1,
        "perPage": 20
    }
}
```
This endpoint retrieves all questions.

### HTTP Request

`GET https://www.swoogo.com/api/v1/questions.json`

### Parameters

These are the parameters you can pass to the API when calling this function.

Parameter | Type | Required? | Default | Description
--------- | ------- | ----------- | ----------- | -----------
fields | string | Optional | id,name | Comma separated list of fields you want to return
expand | string | Optional | null | Comma separated list of related info you want to return
search | string | Optional | null | Comma separated list of search filters you want to apply, eg. name=Member Meeting
page | integer | Optional | null | The page of results you want to view
per-page | integer | Optional | null | The number of results per page (to a max of 200)


## Get A Specific Question

```php
$response = $api->request('https://www.swoogo.com/api/v1/questions/1.json');
```

> The above command returns JSON structured like this:

```json
{
    "id": 1,
    "event_id": 0,
    "type": "email",
    "attribute": "email",
    "name": "Email Address",
    "public_short_name": null,
    "admin_short_name": null,
    "hint": null,
    "min_length": null,
    "max_length": 100,
    "has_fees": false,
    "visible_to_contacts": true,
    "default_page": "begin",
    "default_visible": true,
    "default_required": true,
    "default_sort": 0,
    "upload_groups": null,
    "notes": null,
    "created_at": "2016-01-04 19:20:28",
    "updated_at": "2016-01-05 14:24:02"
}
```

This endpoint retrieves a specific Question.

### HTTP Request

`GET https://www.swoogo.com/api/v1/questions/ID.json`

### Parameters

These are the parameters you can pass to the API when calling this function.

Parameter | Type | Required? | Default | Description
--------- | ------- | ----------- | ----------- | -----------
fields | string | Optional | null | Comma separated list of fields you want to return
expand | string | Optional | null | Comma separated list of related info you want to return



## Available Question Fields

These are the standard fields that the API can return for questions. Please note that custom fields added for specific accounts or events are not listed here.

Name | Type | Description
--------- | ------- | -----------
id|integer|
event_id|integer|Unique ID for the event
type|string|The question type (textInput, textArea, radioList, dropDownList, checkboxList, email, date, time, phone or upload)
attribute|string|The system character ID
name|string|The value of "Name On Forms" for the question
public_short_name|string|The value of "Public Short Name" for the question
admin_short_name|string|The value of "Admin Short Name" for the question
hint|string|The value of "Hint" for the question
min_length|integer|The value of "Min Length" for the question
max_length|integer|The value of "Max Length" for the question
has_fees|boolean|Whether or not the question has any fees associated to it
upload_groups|string|For upload questions, the groups of file types that can be uploaded
notes|string|Any notes entered for the question
created_at|string|When the question was created
updated_at|string|When the question was last updated
default_response|string|

### Extra fields

This is the extra information that can be returned if required by specifying what you need in the expand variable.

Name | Type | Description
--------- | ------- | -----------
choices|object|For multiple choice questions, the available options that can be chosen


# Registrants

## Get All Registrants

```php
$response = $api->request('https://www.swoogo.com/api/v1/registrants.json', array(
     'event_id' => 1,
     'fields' => 'id,first_name,last_name'
));
```

> The above command returns JSON structured like this:

```json
{
    "items": [
        {
            "id": 1,
            "first_name": "Aleshia",
            "last_name": "Tomkiewicz"
        },
        {
            "id": 2,
            "first_name": "Evan",
            "last_name": "Zigomalas"
        }
    ],
    "_links": {
        "self": {
            "href": "registrants.json?event_id=1&fields=id%2Cfirst_name%2Clast_name&page=1"
        }
    },
    "_meta": {
        "totalCount": 2,
        "pageCount": 1,
        "currentPage": 1,
        "perPage": 20
    }
}
```
This endpoint retrieves all registrants.

### HTTP Request

`GET https://www.swoogo.com/api/v1/registrants.json`

### Parameters

These are the parameters you can pass to the API when calling this function.

Parameter | Type | Required? | Default | Description
--------- | ------- | ----------- | ----------- | -----------
event_id | integer | Required | null | The ID of the event you want to retrieve registrants for
fields | string | Optional | id,first_name,last_name | Comma separated list of fields you want to return
expand | string | Optional | null | Comma separated list of related info you want to return (see available fields for details)
search | string | Optional | null | Comma separated list of search filters you want to apply
page | integer | Optional | null | The page of results you want to view
per-page | integer | Optional | null | The number of results per page (to a max of 200)


## Get A Specific Registrant

```php
$response = $api->request('https://www.swoogo.com/api/v1/registrants/1.json');
```

> The above command returns JSON structured like this:

```json
{
    "id": 1,
    "event_id": 1,
    "email": "atomkiewicz@hotmail.com",
    "prefix": "",
    "first_name": "Aleshia",
    "middle_name": "",
    "last_name": "Tomkiewicz",
    "suffix": "",
    "company": "Alan D Rosenburg Cpa Pc",
    "job_title": "",
    "work_phone": "+44 1835 703597",
    "mobile_phone": "",
    "profile_picture": "",
    "twitter_handle": "",
    "gender": {
        "id": 1,
        "value": "Female"
    },
    "birth_date": "",
    "cc_email": "",
    "bio": "",
    "created_at": "2015-10-12 13:48:03",
    "updated_at": "2015-10-12 13:48:15"
}
```

This endpoint retrieves a specific Registrant.

### HTTP Request

`GET https://www.swoogo.com/api/v1/registrants/ID.json`

### Parameters

These are the parameters you can pass to the API when calling this function.

Parameter | Type | Required? | Default | Description
--------- | ------- | ----------- | ----------- | -----------
fields | string | Optional | null | Comma separated list of fields you want to return
expand | string | Optional | null | Comma separated list of related info you want to return (see available fields for details)



## Available Registrant Fields

These are the standard fields that the API can return for registrants. Please note that custom fields added for specific accounts or events are not listed here.

Name | Type | Description
--------- | ------- | -----------
id|integer|The ID of the registrant
event_id|integer|The ID of the event they registered for
group_id|integer|The ID of the registrant they are in a group with
notes|string|Any notes against the registrant
reference|string|Any reference code passed in when the person registered
ip_address|string|The IP address the registrant registered from
individual_net|string|How much this individual registration cost, net of tax
individual_tax|string|How much tax this individual registration paid
individual_gross|string|How much this individual registration paid, including tax
group_net|string|How much the group booking cost, net of tax
group_tax|string|How much the group booking paid in tax
group_gross|string|How much the group booking cost, including tax
created_at|string|When the registration was created
updated_at|string|When the registration was last updated
cancelled_at|string|When the registration was cancelled (if applicable)
checked_in_at|string|When the registration was checked in (if applicable)
reg_type_id|integer|Their registrant type
package_id|integer|Their package selection
discount_id|integer|Their discount code
email|string|The email address
prefix|string|The prefix, for example Mr, Mrs, Dr
first_name|string|The first name
middle_name|string|The middle name
last_name|string|The last name
suffix|string|The suffix, for example Jr
company|string|The company they work for
job_title|string|The job title
work_phone|string|The work phone number
mobile_phone|string|The mobile phone number
gender|string|The gender
birth_date|string|The date of birth
twitter_handle|string|The twitter handle
bio|string|Their bio, usually used for speakers
cc_email|string|The email address that should be copied on emails
registration_status|string|The status of their registration
group_paid|string|How much the group has paid towards their registration
session_ids|string|The sessions the registrant selected

### Extra fields

This is the extra information that can be returned if required by specifying what you need in the expand variable.

Name | Type | Description
--------- | ------- | -----------
workAddress|object|The work address of the registrant
homeAddress|object|The home address of the registrant
billingAddress|object|The billing address of the registrant
sessions|object|The sessions chosen by the registrant


# Sessions

## Get All Sessions

```php
$response = $api->request('https://www.swoogo.com/api/v1/sessions.json', array(
     'event_id' => 1,
     'fields' => 'id,name,date'
));
```

> The above command returns JSON structured like this:

```json
{
    "items": [
        {
            "id": 1,
            "name": "Symposium",
            "date": "2015-08-08"
        },
        {
            "id": 2,
            "name": "Class",
            "date": "2015-08-08"
        }
    ],
    "_links": {
        "self": {
            "href": "sessions.json?event_id=3&fields=id%2Cname%2Cdate&page=1"
        }
    },
    "_meta": {
        "totalCount": 2,
        "pageCount": 1,
        "currentPage": 1,
        "perPage": 200
    }
}
```
This endpoint retrieves all sessions.

### HTTP Request

`GET https://www.swoogo.com/api/v1/sessions.json`

### Parameters

These are the parameters you can pass to the API when calling this function.

Parameter | Type | Required? | Default | Description
--------- | ------- | ----------- | ----------- | -----------
event_id | integer | Required | null | The ID of the event you want to retrieve sessions for
fields | string | Optional | id,name | Comma separated list of fields you want to return
search | string | Optional | null | Comma separated list of search filters you want to apply
page | integer | Optional | null | The page of results you want to view
per-page | integer | Optional | null | The number of results per page (to a max of 200)


## Get A Specific Session

```php
$response = $api->request('https://www.swoogo.com/api/v1/sessions/1.json');
```

> The above command returns JSON structured like this:

```json
{
    "id": 1,
    "name": "Symposium",
    "description": null,
    "date": "2015-08-08",
    "start_time": "11:30:00",
    "end_time": "12:45:00",
    "capacity": "20",
    "notes": null,
    "type_id": null,
    "created_at": "2015-09-28 09:20:03",
    "updated_at": "2015-09-28 09:20:03"
}
```

This endpoint retrieves a specific Session.

### HTTP Request

`GET https://www.swoogo.com/api/v1/sessions/ID.json`

### Parameters

These are the parameters you can pass to the API when calling this function.

Parameter | Type | Required? | Default | Description
--------- | ------- | ----------- | ----------- | -----------
fields | string | Optional | null | Comma separated list of fields you want to return



## Create A New Session

```php
$response = $api->request('https://www.swoogo.com/api/v1/sessions/create.json', array(
     'event_id' => 1,
     'name' => 'Sample Session',
     'date' => '2016-01-22'
), 'post');

```

> The above command returns JSON structured like this:

```json
{
    "id": 10,
    "name": "Sample Session",
    "description": null,
    "date": "2016-01-22",
    "start_time": null,
    "end_time": null,
    "capacity": null,
    "notes": null,
    "created_at": "2015-09-28 10:35:21",
    "updated_at": "2015-09-28 10:35:21"
}
```

This endpoint creates a new Session.

### HTTP Request

`POST https://www.swoogo.com/api/v1/sessions/create.json`

### Parameters

Field | Type | Required? | Default | Description
--------- | ------- | ----------- | ----------- | -----------
event_id | integer | Required | null | The ID of the event you want to create a session for
name | string | Required | null | The name of the session
date | date | Optional | null | The date of the session, formatted YYYY-MM-DD
start_time | time | Optional | null | The start time of the session, formatted HH:MM:SS
end_time | time | Optional | null | The end time of the session, formatted HH:MM:SS
capacity | integer | Optional | null | The maximum number of people who can register for the session



## Available Session Fields

These are the standard fields that the API can return for sessions. Please note that custom fields added for specific accounts or events are not listed here.

Name | Type | Description
--------- | ------- | -----------
id|integer|The ID of the session
event_id|integer|The ID of the event the session belongs to
name|string|The name of the session
description|string|The session description
date|string|The session date
start_time|string|The session start time
end_time|string|The session end time
capacity|string|The number of people who can register for the session
notes|string|Any notes entered about the session




