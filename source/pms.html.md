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

`Authorization: Basic Base64(username:password)`<br/>
`APPID: APIKEY_FROM_AIRHOST`

Base64(username:password) is host's username and password with the character ":" in the middle, then encode with base64 format.

<aside class="notice">
You must replace <code>APIKEY_FROM_AIRHOST</code> with your personal API key.<br/>
Production API URL is cloud.airhost.co<br/>
Test API URL is test.airhost.co
</aside>

# House

## Retrieve All Houses

```shell
curl "https://cloud.airhost.co/api/v1/houses"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
```

> JSON response:

```json
{
	"data": [
		{
			"id": 409,
			"name": "AirHost Hotel No. 1",
			"house_photos": null,
			"address": null,
			"rooms": [
				{
					"id": 421,
					"name": "Double Bedroom"
				}
			]
		},
		{
			"id": 401,
			"name": "AirHost Hotel No. 2",
			"house_photos": [
				"/uploads/house_picture/22/picture/1567674820-f07c678d-725f-59d3-be3a-30926e14ed98_636364134932167010.jpeg",
				"/uploads/house_picture/21/picture/1567674820-f07c678d-725f-59d3-be3a-30926e14ed98.jpeg"
			],
			"address": "Address ABC, Street ABC, Japan",
			"rooms": [
				{
					"id": 4241,
					"name": "Single Bedroom"
				},
				{
					"id": 401,
					"name": "Double Bedroom"
				}
			]
		}
	],
	"meta": {
		"total_pages": 1,
		"total_count": 2
	}
}
```

This endpoint retrieves all the PMS connected houses with room information.

### API Endpoint

`GET https://cloud.airhost.co/api/v1/houses`

### Request Parameters

Parameter   | Description    | Mandatory 
--          | --             | --
id          | House ID		   | true
page        | Page number    | false

<aside class="warning">
Results are paginated. Add page as query parameter for other pages.<br/>
Refer to meta.total_count and meta.total_pages and for total result count and total page count.
</aside>

### Response Properties

Parameter    | Description         | Mandatory 
--           | --                  | --
id           | Property ID         | true
name         | Property Name       | true
house_photos | Array of Photo URLs | true
address      | Address             | true
rooms        | Array of Rooms      | true
rooms[id]    | Room Type ID        | true
rooms[name]  | Room Type Name      | true

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

> JSON response:

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

This endpoint retrieves all bookings from one house.<br/>
Each request has a maximum of 10 results. If more than 10 results, use page 2 to get other results.

### API Endpoint

`GET https://cloud.airhost.co/api/v1/bookings`

### Request Parameters

Parameter  | Description                                              | Mandatory
--         | --                                                       | --
house_id   | Search booking by House ID                               | true
updated_at | Bookings updated after this time (Epoch time in seconds) | true
page       | Page number                                              | false

<aside class="warning">
1. <b>updated_at </b> is a integer field, it is a number represent time since the Unix Epoch <br />
2. To ensure a most updated listed of bookings will be delivery to your end, a <b>hourly</b> data pull is suggested.
</aside>

### Response Properties

Parameter                     | Description                                                                        | Mandatory
--                            | --                                                                                 | --
id                            | Booking ID in Airhost                                                              | true
uid                           | Booking ID from OTA                                                                | true
guestnum                      | Number of guests                                                                   | true
summary                       | Booking's description                                                              | true
dtstart                       | Checkin date                                                                       | true
dtend                         | Checkout date                                                                      | true
status                        | :pending, :confirmed, :cancelled, :blocked, :overlapped, :closed, :user_cancelled  | true
checkin_type                  | self_checkin or operator_checkin                                                   | false
checkin_status                | :before_checkin, :checked_in, :checked_out                                         | false
source                        | OTA name                                                                           | true
created_at                    | Time booking was created E.g. "2018-01-07T16:00:00.000+09:00"                      | true
updated_at                    | Time booking was last updated E.g. "2018-01-07T16:00:00.000+09:00"                 | true
payment_status                | :not_paid, :paid, :partial_paid, :over_paid, :void                                 | true
currency                      | Currency for this booking                                                          | true
reservation_site              | Reservation source				                                                         | false
payment_method                | :ota_collect, :hotel_collect, :hotel_collect_credit, :hotel_collect_cash           | false
user                          | Guest information                                                                  | true
room                          | Room information                                                                   | true
house                         | Property information                                                               | true
fees                          | Charges from OTA                                                                   | false
booking_fees                  | Document any additional charges                                                    | false
checkin_code                  | Same as Booking ID in Airhost                                                      | false
pre_checkin_url               | Pre checkin URL                                                                    | false
checkin_information           | Checkin Information                                                                | false
room_code                     | Pin number of lock if IOT access is enabled                                        | false
key_doc_url                   | Information for key for house                                                      | false
checkin_completion_percentage | Pre checkin guest information submission completion rate                           | false

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

> JSON response:

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

### API Endpoint

`POST http://cloud.airhost.co/api/v1/bookings`

### URL Parameters

Parameter         | Description                                     | Mandatory
--                | --                                              | --
uid               | Your unique ID for this booking                 | true
guestnum          | Number of guests                                | true
summary           | Reservation summary                             | true
description       | Reservation details or guest remarks            | false
dtstart           | Checkin date in ISO 8601 format with timezone.  | true
dtend             | Checkout date in ISO 8601 format with timezone. | true
room_id           | Room ID from airhost                            | true
house_id          | House ID from airhost                           | true
reservation_site  | Website name or company name or the OTA name    | false
user[name]        | Guest name                                      | true
user[email]       | Guest email address                             | true
user[phone]       | Guest phone number                              | true
user[language]    | Guest preferred language, two letters format.   | true
user[country]     | Guest country code                              | false
user[address]     | Guest address                                   | false


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

> JSON response:

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

### API Endpoint

`PUT http://cloud.airhost.co/api/v1/bookings/:id`

### URL Parameters

Parameter | Description                | Mandatory
--        | --                         | --
id        | The Booking's ID           | true
status    | "confirmed" or "cancelled" | true
