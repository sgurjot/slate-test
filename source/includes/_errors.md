# Errors

Combyne Connect uses standard HTTP response codes to indicate success or failure of an API request. 
In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that resulted from the provided information (e.g., a required parameter was missing), and codes in the 5xx range indicate an error with Best Buy’s servers.

Applies to: • Listing API • Authentication API

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Credentials or token invalid.
403 | Forbidden -- You do not have access to that resource.
404 | Not Found -- Resource not found
500 | Internal Server Error -- We had a problem with our server. Try again later.
