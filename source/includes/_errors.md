# Errors

The Swoogo API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is wrong
401 | Unauthorized -- Your API credentials are wrong
403 | Forbidden -- You don't have permissions to view the resource you requested
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- You tried to access a resource with an invalid method
418 | I'm a teapot
429 | Too Many Requests -- You're requesting too many resources! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
