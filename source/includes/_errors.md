# Errors

The Airhost API uses the following error codes:


Error Code | Description
---------- | -------
400 | Bad Request -- Your request is invalid (missing_required_paramter)
401 | Unauthorized -- Your API key is wrong (invalid_api_key) or \n  invalid_username_password: invalid username and password, please make sure they are base64 encoded.\n invalid_api_call: you are not authorized to use this API endpoint.
403 | Forbidden -- The kitten requested is hidden for administrators only.
404 | Not Found -- The specified record could not be found.
405 | Method Not Allowed -- You tried to access a record with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
441 | Not ready for checkin -- You cannot check in now, but need to wait till check-in time (not_ready_for_checkin)
442 | Already checked in -- This reservation has already checked in (reservation_already_checked_in)
443 | Already checked out -- This reservation has already checked out (reservation_already_checked_out)
429 | Too Many Requests -- You're requesting too many kittens! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

