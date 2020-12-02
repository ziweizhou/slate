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


# Checkin Setting

## Checkin Setting for a specific property

```shell
curl "https://test.airhost.co/api/v1/checkin/houses/:id/settings" \
  -X GET -H "Content-Type: application/json" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
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
            "required": "foreigner_only"
        },
        {
            "name": "address",
            "required": "represent_only"
        },
        {
            "name": "phone",
            "required": "hide"
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

`GET https://test.airhost.co/api/v1/checkin/houses/:id/settings`

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
checkin_settings[fields][required] | true | `all` means all guests are required to submit. \n `represent_only` means only the person made the reservation \n `hide` meaning, no need to ask. `foreigner_only` meaning it is for non-Japanese.
checkin_settings[name] | false | Guest Name
checkin_settings[photo] | false | Guest Passport Photos
checkin_settings[dob] | false | Guest Date of Birth
checkin_settings[nationality] | false | Guest Nationality (2 letters ISO3166)
checkin_settings[postal_code] | false | Guest Postal Code
checkin_settings[address] | false | Guest Address
checkin_settings[occupation] | false | Guest Occupation
checkin_settings[visa_no] | false | Guest Passport/Visa Number
checkin_settings[phone] | false | Guest Phone number
checkin_settings[last_port_embark] | false | Previous Country before arriving here
checkin_settings[next_port_disembark] | false | Next Country will depart to
checkin_settings[signature] | false | Guest signature
<aside class="success">
These settings can be set via Airhost PMS UI.
</aside>

# Booking

## Retrieve Bookings

```shell
curl "https://test.airhost.co/api/v1/bookings" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
  -H "Content-Type: application/json" \
  -X GET \
  --data '{ \
    "house_id"=1, \
    "updated_at"=1533217109 \
    }'
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
        },
        "room":
        {
            "id": 123,
            "name": "One Bedroom"
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
        "fees":[
            {"paid":false,
            "fee_type":"per_day",
            "amount":7700.0,
            "date":"2020-04-08",
            "description":"Standard Rate",
            "ota_collect":false,
            "included":false
            }
        ],
        "payment_status": "paid",
        "payment_method": "hotel_collect", 
        "currency": "JPY", 
        "reservation_site" : "Booking.COM"
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

`GET https://test.airhost.co/api/v1/bookings`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
house_id | true | Search booking by House ID
updated_at | true | Bookings updated after this time, it is in timestamp format. \n current epoch/unix timestamp
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
cancellation_fee | false | cancellation fees if booking is cancelled
payment_status | false | :not_paid, :paid, :partial_paid, :over_paid
payment_method | false | :ota_collect, :hotel_collect, :hotel_collect_credit, :hotel_collect_cash
currency | true | currency for this booking
reservation_site | false | reservation is made from. 

<aside class="warning">
1. The <b>updated_at </b> is a integer field, it is a number represent time since the Unix Epoch <br />
2. To ensure a most updated listed of bookings will be delivery to your end, a <b>hourly</b> data pull is suggested.
</aside>

## Search a Booking

```shell
curl "https://test.airhost.co/api/v1/checkin/bookings" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
  -H "Content-Type: application/json" \
  -X GET \
  --date '{ \
    "uid":123, \
    "last_name": "Smith", \
    "first_name": "John", \
    "house_id": 1 \
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
        },
        "room":
        {
            "id": 123,
            "name": "One Bedroom"
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
        "fees":[
            {"paid":false,
            "fee_type":"per_day",
            "amount":7700.0,
            "date":"2020-04-08",
            "description":"Standard Rate",
            "ota_collect":false,
            "included":false
            }
        ],
        "payment_status": "paid",
        "payment_method": "hotel_collect", 
        "currency": "JPY", 
        "reservation_site" : "Booking.COM"
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

`GET https://test.airhost.co/api/v1/checkin/bookings`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
house_id | true | Search booking by House ID
uid | true | Search booking by its OTA confirmation ID
last_name | false | Search booking by guest last name
first_name | false | Search booking by guest first name
room_number | false | Search booking by Room Number (used for checkout mostly)

<aside class="info">
1. with Airhost PMS A unique uid is sent to the guest if the auto message feature is enabled. <br />
2. Please note the URL has <b>/checkin</b> in the front.
</aside>

## Mark a Booking as Paid

```shell
curl -X POST "https://test.airhost.co/api/v1/checkin/bookings/:id/completed" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST"
  --date '{ \
    "payload": "some details about this transaction" \
    "payment_method": "credit_card" \
    }'
```

> The above command returns JSON structured like this:

```json
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
        },
        "room":
        {
            "id": 123,
            "name": "One Bedroom"
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
        "fees":[
            {"paid":false,
            "fee_type":"per_day",
            "amount":7700.0,
            "date":"2020-04-08",
            "description":"Standard Rate",
            "ota_collect":false,
            "included":false
            }
        ],
        "payment_status": "paid",
        "payment_method": "hotel_collect", 
        "currency": "JPY", 
        "reservation_site" : "Booking.COM"
    }
```
This endpoint marks the booking's payment status to paid

### HTTP Request

`POST http://cloud.airhost.co/api/v1/checkin/bookings/:id/paid`

### URL Parameters


Parameter | Default | Description
--------- | ------- | -----------
id | true | The Booking's ID
payload | false | transaction data to save to the server. Such as transactionID or Paid Amount
payment_method | false | use one of the following: cash, credit_card, bank_transfer, ota_collect


## Complete a Booking's checkin.

```shell
curl -X POST "https://test.airhost.co/api/v1/checkin/bookings/:id/completed" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST"
```

> The above command returns JSON structured like this:

```json
{
  "room_unit": "101",
  "checkin_information": "Checkin Instructure is at http://goog.gl/abc",
  "room_code": "123",
  "key_doc_url": "http://goo.gl/abc",
}
```

This endpoint mark the booking checkin is completed and receive the details room information.

<aside class="warning">Please note, if this endpoint is not called, guest has no way to receive their checkin instruction</aside>

### HTTP Request

`POST http://cloud.airhost.co/api/v1/checkin/bookings/:id/completed`

### URL Parameters


Parameter | Default | Description
--------- | ------- | -----------
booking_id | true | The Booking's ID

### Return Data Parameters

Parameter | Default | Description
--------- | ------- | -----------
room_unit | true | Guest's Room Number
checkin_information | false | Instruction of How to get Key
room_code | false | If it is smart lock
key_doc_url | false | Additional informations.



## Email a Booking's checkin information.

```shell
curl -X POST "https://test.airhost.co/api/v1/checkin/bookings/:id/email" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
  --date '{"name": "John Smith", \
  "email": "john@smith.com" \
    }'
```

> The above command returns JSON structured like this:

```json
{
  "room_unit": "101",
  "checkin_information": "Checkin Instructure is at http://goog.gl/abc",
  "room_code": "123",
  "key_doc_url": "http://goo.gl/abc",
}
```

This endpoint mark the booking checkin is completed and receive the details room information.

<aside class="warning">Please note, if this endpoint is not called, guest has no way to receive their checkin instruction</aside>

### HTTP Request

`POST http://cloud.airhost.co/api/v1/checkin/bookings/:id/completed`

### URL Parameters


Parameter | Default | Description
--------- | ------- | -----------
booking_id | true | The Booking's ID
name | true | The new recipient's name
email | true | The new recipient's email

## Complete a Booking's checkout.

```shell
curl -X POST "https://test.airhost.co/api/v1/checkin/bookings/:id/checked_out" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST"

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
        "checkin_status": "checked_out",
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

This endpoint mark the booking checkout is completed and receive the details booking information.


### HTTP Request

`POST http://cloud.airhost.co/api/v1/checkin/bookings/:id/checked_out`

### URL Parameters


Parameter | Default | Description
--------- | ------- | -----------
id | true | The Booking's ID

# Guest

## Get All Guests

```shell
curl -X GET "https://test.airhost.co/api/v1/checkin/bookings/:booking_id/guests" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
  -H "Content-Type: application/json"
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
        "nationality": "US",
        "postal_code": "123"
    },
    "last_name": "Phillips",
    "first_name": "Tom",
    "gender": "male",
    "dob": "2018-05-01",
    "visa_no": "123456789",
    "occupation": "CEO",
    "nationality": "US",
    "postal_code": "123",
    "address": "USA",
    "phone": "1234456",
    "photo": {
      "id": 36,
      "checkin_guest_id": 13,
      "booking_id": 530592,
      "item": {
        "url": "/uploads/development/checkin_attachment/36/item/id_photo.jpg",
        "thumb": {
          "url": "/uploads/development/checkin_attachment/36/item/thumb_id_photo.jpg"
        }
      },
      "data": {},
      "attachment_type": "photo",
      "created_at": "2018-12-10T14:25:34.617+09:00",
      "updated_at": "2018-12-10T14:25:34.617+09:00",
      "name": "photo"
    },
    "ident_photo": null,
    "last_port_embark": null,
    "next_port_disembark": null,
    "signature": {
      "id": 37,
      "checkin_guest_id": 13,
      "booking_id": 530592,
      "item": {
        "url": "/uploads/development/checkin_attachment/37/item/signature.png",
        "thumb": {
          "url": "/uploads/development/checkin_attachment/37/item/thumb_signature.png"
        }
      },
      "data": {},
      "attachment_type": "photo",
      "created_at": "2018-12-10T14:25:34.666+09:00",
      "updated_at": "2018-12-10T14:25:34.666+09:00",
      "name": "signature"
    },
    "email": "tom@airhost.co"

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
        "nationality": "US",
        "postal_code": "12345"
    }
}]
```

This endpoint retrieves all guests associate with a booking match search query.

### HTTP Request

`GET https://test.airhost.co/api/v1/checkin/bookings/:booking_id/guests`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
booking_id | true | The Booking's ID



## Create a guest.

```shell
curl -X POST "https://test.airhost.co/api/v1/checkin/bookings/:booking_id/guests" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
  -data '{ \
        "first_name": "Samanta", \
        "last_name": "Phillips", \
        "notes": "test", \
        "dob": "2018-05-17", \
        "phone": "123", \
        "address": "123 USA", \
        "visa_no": "123456789", \
        "occupation": "developer", \
        "nationality": "US", \
        "postal_code": "12345" \
    }'
```

> The above command returns JSON structured like this:

```json
{
    "id": 3,
    "first_name": "Samanta",
    "last_name": "Phillips",
    "notes": "sad",
    "settings":
    {
        "dob": "2018-05-17",
        "phone": "123",
        "address": "123 USA",
        "visa_no": "123456789",
        "occupation": "developer",
        "nationality": "US",
        "postal_code": "12345"
    }
}
```

This endpoint add a guest into this booking

### HTTP Request

`POST http://cloud.airhost.co/api/v1/checkin/bookings/:booking_id/guests`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
booking_id | true | The Booking's ID
first_name | false | Guest First Name
last_name | false | Guest Last Name
dob | false | Guest Date of Birth
gender | false | Guest Gender (male or female)
email | false | Guest Email Address
nationality | false | Guest Nationality (2 letters ISO3166)
postal_code | false | Guest Postal Code
address | false | Guest Address
occupation | false | Guest Occupation
visa_no | false | Guest Passport/Visa Number
phone | false | Guest Phone number
last_port_embark | false | Previous Country before arriving here
next_port_disembark | false | Next Country will depart to



## Update a guest.

```shell
curl -X PUT "https://test.airhost.co/api/v1/checkin/bookings/:booking_id/guests/:id" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
  -data '{ \
        "first_name": "Samanta", \
        "last_name": "Phillips", \
        "notes": "test", \
        "dob": "2018-05-17", \
        "phone": "123", \
        "address": "USA", \
        "visa_no": "123456789", \
        "occupation": "developer", \
        "nationality": "Untied States", \
        "postal_code": "12345" \
    }'
```

> The above command returns JSON structured like this:

```json
{
    "id": 3,
    "first_name": "Samanta",
    "last_name": "Phillips",
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

`PUT http://cloud.airhost.co/api/v1/checkin/bookings/:booking_id/guests/:id`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
booking_id | true | The Booking's ID
id | true |The ID of the guest



# Attachment

## Get All Attachments

```shell
curl -X GET "https://test.airhost.co/api/v1/checkin/bookings/:booking_id/guests/:guest_id/attachments" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
  -H "Content-Type: application/json"
```


> The above command returns JSON structured like this:

```json
[{
    "id": 2,
    "remote_item_url": "http://xxxxxx.jpg",
    "name": "photo"
},
{
    "id": 3,
    "remote_item_url": "http://xxxxxx.jpg",
    "name": "signature"
}]
```

This endpoint retrieves all attachments associate with a booking match search query.

### HTTP Request

`GET https://test.airhost.co/api/v1/checkin/bookings/:booking_id/guests/:guest_id/attachments`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
booking_id | true | The Booking's ID
guest_id | true |The ID of the guest
name| true| the name of the attachment.


## Create a attachment.

```shell
curl -X POST "https://test.airhost.co/api/v1/checkin/bookings/:booking_id/guests/:guest_id/attachments" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
  --data '{ \
        "item": "image/jpeg;base64,(base64 encoded data)", \
        "name": "ident_photo",
        "file_name": "passport_photo.jpg" \
    }'
```

> The above command returns JSON structured like this:

```json
{
    "id": 3,
    "name": "ident_photo",
    "remote_item_url": "http://xxxxxx.jpg"
}
```

This endpoint add a attachment into this booking

### HTTP Request

`POST http://cloud.airhost.co/api/v1/checkin/bookings/:booking_id/guests/:guest_id/attachments`

### URL Parameters

Parameter | Default| Description
--------- | ------- | -----------
booking_id | true |The ID of the booking
guest_id | true |The ID of the guest
name| true |the name of this attachment.
    |      | Use to distinguish if it is a passport or other type of attachment.
    |      | please use name="photo" for passport photo
    |      | Please use name="ident_photo" for holding passport photo
    |      | please use name="signature" for signature photo
file_name| false| the file name of the image.
## Update a attachment.

```shell
curl -X PUT "https://test.airhost.co/api/v1/checkin/bookings/:booking_id/guests/:guest_id/attachments/:id" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
  -data '{ \
        "id": 3, \
        "item": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMkAAADJCAYAAACJxhYFAAAAAXNSR0IArs4c6QAAQABJREFUeAHtvQeYZNd153equ0Ln3NOT8ww.....", \
        "file_name": "passport_photo.jpg", \
        "name": "photo" \
    }'
```

> The above command returns JSON structured like this:

```json
{
    "id": 3,
    "name": "photo",
    "remote_item_url": "http://xxxxxx.jpg"
}
```

This endpoint add a attachment into this booking

### HTTP Request

`PUT http://cloud.airhost.co/api/v1/checkin/bookings/:booking_id/guests/:guest_id/attachments/:id`

### URL Parameters

Parameter | Default| Description
--------- | ------- | -----------
booking_id | true |The ID of the booking
guest_id | true |The ID of the guest
id | true | The id of the attachment

#Video Chat
## Create a Video Chat request.

```shell
curl -X POST "https://test.airhost.co/api/v1/checkin/bookings/:booking_id/video_chats" \
  -H "Authorization: Basic ZGVtbzpuaWt1bmlrdQ==" \
  -H "APPID: APIKEY_FROM_AIRHOST" \
```

> The above command returns JSON structured like this:

```json
{
    "token": "xxxxxx",
    "chat_room": "xxxx-xxx-xxx-xxx"
}
```

This endpoint create a video chat

### HTTP Request

`POST http://cloud.airhost.co/api/v1/checkin/bookings/:booking_id/video_chats`

### URL Parameters

Parameter | Default | Description
--------- | ------- | -----------
token | true | The Auth token to connect to the video chat service
chat_room| true| the chat room that is created
