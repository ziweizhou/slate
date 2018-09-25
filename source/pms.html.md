---
title: Airhost Checkin API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby

toc_footers:
  - <a href='mailto:hello@airhost.co'>Email Us to obtain the API key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Airhost Checkin API! YOu can use our API to access basic booking information and guest informations.

We have language bindings in Ruby! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Authentication

> To generate proper authentication header, use this code:

```ruby
require 'base64'
base64_encoded_string = Base64.encode64("username:password")
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
```

> Make sure to replace `APIKEY_FROM_AIRHOST` with your API key.

If you don't have one, please contact [Airhost Support](mailto:hello@airhost.co)

Airhost expects for the API key plus the Authentication Key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Basic Base64(username:password)`
`APPID: APIKEY_FROM_AIRHOST`

Base64(username:password) is host's username and password with the character ":" in the middle, then encode with base64 format.

<aside class="notice">
You must replace <code>APIKEY_FROM_AIRHOST</code> with your personal API key.
Production API URL is cloud.airhost.co
Test API URL is test.airhost.co
</aside>


# House

## Retrieve All Houses

```shell
curl "https://cloud.airhost.co/api/v1/houses"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
```


> The above command returns JSON structured like this:

```json
{
    "data":[{
        "id": 1,
        "name": "AirHost Hotel No. 1",
        "rooms": [
            {
                "id":1,
                "name":"Double Bedroom"
            }
        ]
    }],
    "meta":{
        "total_pages":10,
        "total_count":100
    }
}
```

This endpoint retrieves all the PMS connected houses with room information.

### HTTP Request

`GET https://cloud.airhost.co/api/v1/houses`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | true | The house's ID

### Return Data Parameters

Parameter | Default | Description
--------- | ------- | -----------
name | true | Property Name
id | true | Property ID
rooms | true | Array of Rooms
rooms[id] | true | Room Type ID
rooms[name] | true | Room Type Name

<aside class="success">
These settings can be set via Airhost PMS UI.
</aside>

# Booking

## Retrieve Bookings

```shell
curl "https://cloud.airhost.co/api/v1/bookings"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
  --date '{
  "house_id": 1,
  "updated_at": 11111111
    }
```
```ruby
    updated_at = Time.zone.now.to_i
```

> The above command returns JSON structured like this:

```json
{
    "data": [
    {
        "id": 1358,
        "uid": "HMMJTRSTTE",
        "guestnum": 3,
        "summary": "Nikolai Ramstetter (HMMJTRSTTE)",
        "dtstart": "2018-04-07T16:00:00.000+09:00",
        "dtend": "2018-04-11T11:00:00.000+09:00",
        "status": "confirmed",
        "checkin_type": null,
        "checkin_status": "checked_in",
        "source": "airbnb",
        "room":
        {
            "id": 123,
            "name": "One Bedroom"
        },
        "house":{
            "id": 1,
            "name": "Airhost Hotel"
        },
        "user":
        {
            "name": "Nikolai Ramstetter",
            "email": "nikolai-smxiddb2jijh8gjt@guest.airbnb.com"
        }
    }],
    "meta":
    {
        "total_pages": 1,
        "total_count": 1
    }
}
```

This endpoint retrieves all bookings from one house.

### HTTP Request

`GET https://cloud.airhost.co/api/v1/bookings`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
house_id | true | Search booking by House ID
updated_at | true | bookings updated after this time (epoch time in seconds)

<aside class="warning">
1. The <b>updated_at </b> is a integer field, it is a number represent time since the Unix Epoch <br />
2. To ensure a most updated listed of bookings will be delivery to your end, a <b>hourly</b> data pull is suggested.
</aside>


## Create a Booking.

```shell
curl -X POST "https://cloud.airhost.co/api/v1/bookings"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
  -data '{
        "uid": "ABCDEFG",
        "guestnum": 3,
        "summary": "John Smith (ABCDEFG)",
        "description": "Need no smoking room",
        "dtstart": "2018-04-07T16:00:00.000+09:00",
        "dtend": "2018-04-11T11:00:00.000+09:00",
        "room_id": 123,
        "house_id": 1,
        "reservation_site": "your site name"
        "user":
        {
            "name": "John Smith",
            "email": "john_smith@gmail.com"
        }
    }'
```

> The above command returns JSON structured like this:

```json
{
    "id": 1358,
    "uid": "ABCDEFG",
    "guestnum": 3,
    "summary": "John Smith (ABCDEFG)",
    "description": "Need no smoking room",
    "dtstart": "2018-04-07T16:00:00.000+09:00",
    "dtend": "2018-04-11T11:00:00.000+09:00",
    "room":
    {
        "id": 123,
        "name": "One Bedroom"
    },
    "house":{
        "id": 1,
        "name": "Airhost Hotel"
    },
    "reservation_site": "your site name",
    "user":
    {
        "name": "John Smith",
        "email": "john_smith@gmail.com",
        "phone": "123456789",
        "language": "en",
        "country": "JP",
        "address": "123 test street"
    }
}
```

This endpoint add a guest into this booking

### HTTP Request

`POST http://cloud.airhost.co/api/v1/bookings`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
uid | true | The unique identify of this booking (could be the ID in your system)
guestnum | true | Number of the guest.
summary | true | Reservation summary
description | false | Reservation details or guest remarks
dtstart | true |  checkin date in ISO 8601 format with timezone.
dtend | true |  checkout date in ISO 8601 format with timezone.
room_id | true | room ID from airhost
house_id | true | house ID from airhost
reservation_site| false| your website name or company name or the OTA name
user[name] | true | guest name
user[email] | true | guest email address
user[phone] | true | guest phone number
user[language] | true | guest preferred language, two letters format.
user[country] | false | guest country code
user[address] | false | guest address


## Update a Booking.

```shell
curl -X PUT "https://cloud.airhost.co/api/v1/bookings/:id"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
  -data '{
    "uid": "ABCDEFG",
    "guestnum": 3,
    "summary": "John Smith (ABCDEFG)",
    "description": "Need no smoking room",
    "dtstart": "2018-04-07T16:00:00.000+09:00",
    "dtend": "2018-04-11T11:00:00.000+09:00",
    "status": "confirmed",
    "checkin_status": "checked_in",
    "reservation_site": "airbnb",
    "room_id": 123,
    "house_id": 1,
    "user":
    {
        "name": "John Smith",
        "email": "john_smith@gmail.com"
    }
}'
```

> The above command returns JSON structured like this:

```json
{
    "uid": "ABCDEFG",
    "guestnum": 3,
    "summary": "John Smith (ABCDEFG)",
    "dtstart": "2018-04-07T16:00:00.000+09:00",
    "dtend": "2018-04-11T11:00:00.000+09:00",
    "status": "confirmed",
    "checkin_status": "checked_in",
    "reservation_site": "airbnb",
    "room":
    {
        "id": 123,
        "name": "One Bedroom"
    },
    "house":{
        "id": 1,
        "name": "Airhost Hotel"
    },
    "user":
    {
        "name": "John Smith",
        "email": "john_smith@gmail.com"
    }
}
```

This endpoint add a guest into this booking

### HTTP Request

`PUT http://cloud.airhost.co/api/v1/bookings/:id`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | true | The Booking's ID
status| true | possible values are ["confirmed", "cancelled"]
