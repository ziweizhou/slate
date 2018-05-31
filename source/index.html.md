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
</aside>


# Checkin Setting

## Checkin Setting for a specific property

```shell
curl "https://cloud.airhost.co/api/v1/houses/:id/checkin_settings"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
```


> The above command returns JSON structured like this:

```json
{
    "name": "AirHost Hotel No. 1",
    "welcome_msg": "Welcome to AirHost Hotel ^^",
    "terms": "https://goo.gl/eydXgi",
    "hotel_img": "https://s3-ap-northeast-1.amazonaws.com/triosky-airhost/uploads/house/292/app_background_img/TS_Hotel_King_lowrez.jpg",
    "logo": "https://s3-ap-northeast-1.amazonaws.com/triosky-airhost/uploads/house/292/app_logo/icons8-service-bell-96.png",
    "key_info_img_url": "https://s3-ap-northeast-1.amazonaws.com/triosky-airhost/uploads/house/292/key_info_img/TS_Hotel_King_lowrez.jpg",
    "key_info_doc_url": "https://goo.gl/QJrtJu",
    "checkin_type": "self_checkin",
    "operator_id": null,
    "checkin_settings":
    {
        "fields": [
        {
            "name": "photo",
            "required": "all"
        },
        {
            "name": "name",
            "required": "all"
        },
        {
            "name": "occupation",
            "required": "all"
        },
        {
            "name": "postal_code",
            "required": "all"
        },
        {
            "name": "address",
            "required": "represent_only"
        },
        {
            "name": "phone",
            "required": "all"
        },
        {
            "name": "nationality",
            "required": "all"
        },
        {
            "name": "visa_no",
            "required": "all"
        },
        {
            "name": "dob",
            "required": "all"
        }],
        "enabled": "1"
    }
}
```

This endpoint retrieves checkin setting of a listing.

### HTTP Request

`GET https://cloud.airhost.co/api/v1/houses/:id/checkin_settings`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | true | The house's ID

### Return Data Parameters

Parameter | Default | Description
--------- | ------- | -----------
name | true | Property Name
welcome_msg | false | Default greeting message
terms | false | Terms of Service
hotel_img | false | Picture of the property
logo | false | Logo of this property
key_info_img_url | false | image information related to how to get key
key_info_doc_url | false | document URL realted to how to get key
checkin_settings[fields][name] | true | guest detail information fields
checkin_settings[fields][required] | true | `all` means all guests are required to submit. \n `represent_only` means only the person made the reservation

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
updated_at | true | bookings updated after this time

<aside class="warning">
1. The <b>updated_at </b> is a integer field, it is a number represent time since the Unix Epoch <br />
2. To ensure a most updated listed of bookings will be delivery to your end, a <b>hourly</b> data pull is suggested.
</aside>

## Search a Booking

```shell
curl "https://cloud.airhost.co/api/v1/bookings"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
  --date '{"uid":123,
  "last_name": "Smith",
  "first_name": "John",
  "house_id": 1
    }'
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

This endpoint retrieves all bookings match search query.

### HTTP Request

`GET https://cloud.airhost.co/api/v1/bookings`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
house_id | true | Search booking by House ID
uid | true | Search booking by its OTA confirmation ID
last_name | false | Search booking by guest last name
first_name | false | Search booking by guest first name

<aside class="success">
with Airhost PMS A unique uid is sent to the guest if the auto message feature is enabled.
</aside>

## Complete a Booking's checkin.

```shell
curl -X POST "https://cloud.airhost.co/api/v1/bookings/:id/completed"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
```

> The above command returns JSON structured like this:

```json
{
  "room_unit": "101",
  "checkin_information": "Checkin Instructure is at http://goog.gl/abc",
  "room_code": "123",
  "wifi_info": "wifi name: hello, wifi password: 1234",
}
```

This endpoint mark the booking checkin is completed and receive the details room information.

<aside class="warning">Please note, if this endpoint is not called, guest has no way to receive their checkin instruction</aside>

### HTTP Request

`POST http://cloud.airhost.co/api/v1/bookings/:id/completed`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the booking


# Guest

## Get All Guests

```shell
curl "https://cloud.airhost.co/api/v1/bookings/:booking_id/checkin_guests"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
```


> The above command returns JSON structured like this:

```json
[{
    "id": 2,
    "name": "Tom Phillips",
    "notes": "awesome",
    "settings":
    {
        "dob": "2018-05-01",
        "phone": "1234456",
        "address": "USA",
        "visa_no": "123456789",
        "occupation": "CEO",
        "nationality": "Untied States",
        "postal_code": "123"
    }
},
{
    "id": 3,
    "name": "Samanta Phillips",
    "notes": "sad",
    "settings":
    {
        "dob": "2018-05-17",
        "phone": "123",
        "address": "USA",
        "visa_no": "123456789",
        "occupation": "developer",
        "nationality": "Untied States",
        "postal_code": "12345"
    }
}]
```

This endpoint retrieves all guests associate with a booking match search query.

### HTTP Request

`GET https://cloud.airhost.co/api/v1/bookings/:booking_id/checkin_guests`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
booking_id | true | The Booking's ID



## Create a guest.

```shell
curl -X POST "https://cloud.airhost.co/api/v1/bookings/:booking_id/checkin_guests"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
  -data '{
        "name": "Samanta Phillips",
        "notes": "test",
        "dob": "2018-05-17",
        "phone": "123",
        "address": "USA",
        "visa_no": "123456789",
        "occupation": "developer",
        "nationality": "Untied States",
        "postal_code": "12345"
    }'
```

> The above command returns JSON structured like this:

```json
{
    "id": 3,
    "name": "Samanta Phillips",
    "notes": "sad",
    "settings":
    {
        "dob": "2018-05-17",
        "phone": "123",
        "address": "USA",
        "visa_no": "123456789",
        "occupation": "developer",
        "nationality": "Untied States",
        "postal_code": "12345"
    }
}
```

This endpoint add a guest into this booking

### HTTP Request

`POST http://cloud.airhost.co/api/v1/bookings/:booking_id/checkin_guests`

### URL Parameters

Parameter | Description
--------- | -----------
booking_id | The ID of the booking


## Update a guest.

```shell
curl -X PUT "https://cloud.airhost.co/api/v1/bookings/:booking_id/checkin_guests/:id"
  -H "Authorization: Basic Base64(username:password)"
  -H "APPID: APIKEY_FROM_AIRHOST"
  -data '{
        "name": "Samanta Phillips",
        "notes": "test",
        "dob": "2018-05-17",
        "phone": "123",
        "address": "USA",
        "visa_no": "123456789",
        "occupation": "developer",
        "nationality": "Untied States",
        "postal_code": "12345"
    }'
```

> The above command returns JSON structured like this:

```json
{
    "id": 3,
    "name": "Samanta Phillips",
    "notes": "sad",
    "settings":
    {
        "dob": "2018-05-17",
        "phone": "123",
        "address": "USA",
        "visa_no": "123456789",
        "occupation": "developer",
        "nationality": "Untied States",
        "postal_code": "12345"
    }
}
```

This endpoint add a guest into this booking

### HTTP Request

`PUT http://cloud.airhost.co/api/v1/bookings/:booking_id/checkin_guests/:id`

### URL Parameters

Parameter | Description
--------- | -----------
booking_id | The ID of the booking
id | The id of the guest



