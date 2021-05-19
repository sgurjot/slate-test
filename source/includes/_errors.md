# Errors

Combyne Connect uses standard HTTP response codes to indicate success or failure of an API request. 
In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that resulted from the provided information (e.g., a required parameter was missing), and codes in the 5xx range indicate an error with Best Buy’s servers.

Applies to: • Listing API • Authentication API

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The kitten requested is hidden for administrators only.
404 | Not Found -- The specified kitten could not be found.
405 | Method Not Allowed -- You tried to access a kitten with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The kitten requested has been removed from our servers.
418 | I'm a teapot.
429 | Too Many Requests -- You're requesting too many kittens! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
