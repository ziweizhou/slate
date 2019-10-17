---
title: Airhost Checkin API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - php
  - javascript
  - python

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

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ=="
  -H "APPID: APIKEY_FROM_AIRHOST"
```


```ruby
require 'base64'
base64_encoded_string = Base64.encode64("username:password")

Base64.encode64("demo:nikuniku")
=> "ZGVtbzpuaWt1bmlrdQ=="
```

```php
base64_encode("demo:nikuniku")
=> "ZGVtbzpuaWt1bmlrdQ=="
```

```javascript
Buffer.from("demo:nikuniku").toString('base64')
#=> "ZGVtbzpuaWt1bmlrdQ=="
```

```python
base64.b64decode("demo:nikuniku")
#=> "ZGVtbzpuaWt1bmlrdQ=="
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
Each request has a maximum of 10 results. If more than 10 results, use page 2 to get other results.

### HTTP Request

`GET https://cloud.airhost.co/api/v1/houses`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | true | The house's ID
page| false | to retrieve houses from paginated result. (if meta.total_pages > 1)

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
  "updated_at": 11111111,
  "page": 2
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
        "checkin_type": "self_checkin",
        "checkin_status": "checked_in",
        "source": "airbnb",
        "created_at": "2018-01-07T16:00:00.000+09:00",
        "updated_at": "2018-01-11T11:00:00.000+09:00",
        "payment_status": "paid",
        "currency": "JPY",
        "reservation_site": "Booking.COM",
        "payment_method": "hotel_collect",
        "user": {
            "name": "Nikolai Ramstetter",
            "email": "nikolai@gmail.com",
            "language": "en",
            "phone": "1234567",
            "address": null,
            "country": "Spain",
            "postal_code": null
        },
        "room":
        {
            "id": 123,
            "name": "One Bedroom",
            "room_unit": "201"
        },
        "house":{
            "id": 1,
            "name": "Airhost Hotel"
        },
        "user":
        {
            "name": "Nikolai Ramstetter",
            "email": "nikolai-smxiddb2jijh8gjt@guest.airbnb.com"
        },
        "booking_fees": [{
            "id":3,
            "fee_type":"transaction_fee",
            "description":"Stripe service fee",
            "dtdate":null,"amount":1008.0,
            "included":true,"currency":null,
            "paid":true,"per_night":false,
            "per_person":false,
            "percentage":0,
            "created_at":"2018-12-11T12:17:43.378+09:00",
            "updated_at":"2018-12-15T01:02:18.800+09:00"
        }],
        "fees":[{
            "paid":false,
            "fee_type":"per_day",
            "amount":7700.0,
            "date":"2020-04-08",
            "description":"Standard Rate",
            "ota_collect":false,
            "included":false
            },
            {"included": false,
            "paid": true,
            "fee_type": "cleaning_fee",
            "amount": "123.0"
        }],
        "booking_fees": [],
        "checkin_code": 9760,
        "pre_checkin_url": "http://airhost/en/checkin/bookings/2b4e456f-abcd-efgh-ijkl-cfb76bb1bf1e",
        "checkin_information": null,
        "room_code": null,
        "key_doc_url": null,
        "checkin_completion_percentage": 80
    }],
    "meta":
    {
        "total_pages": 1,
        "total_count": 1
    }
}
```

This endpoint retrieves all bookings from one house.
Each request has a maximum of 10 results. If more than 10 results, use page 2 to get other results.

### HTTP Request

`GET https://cloud.airhost.co/api/v1/bookings`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
house_id | true | Search booking by House ID
updated_at | true | bookings updated after this time (epoch time in seconds)
page| false | to retrieve bookings from paginated result. (if meta.total_pages > 1)

### Return Data Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | true | Booking ID in Airhost
uid | true | Booking ID from OTA
guestnum | true | number of guests
summary | true | Booking's description
dtstart | true | checkin date
dtend | true | checkout date
checkin_type | false | self_checkin or operator_checkin
checkin_status | false | :before_checkin, :checked_in, :checked_out
source | true | OTA name
room | true | room information
house | true | property information
user | true | guest information
booking_fees | false | document any additional charges
fees | false | charges from OTA
payment_status | false | :not_paid, :paid, :partial_paid, :over_paid
payment_method | false | :ota_collect, :hotel_collect, :hotel_collect_credit, :hotel_collect_cash
currency | true | currency for this booking
reservation_site | false | reservation is made from. 


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
