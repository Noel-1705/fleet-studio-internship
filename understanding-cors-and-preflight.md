# CONCEPTS
---
## 1. What is CORS and why it exists
CORS or Cross Origin Responses is the requestes handled by the browser from one origin to another origin. It might be to access the data from a bank server by a shopping website, or to access the data from one origin to another origin. If CORS didn't exist any origin could access any data and that would cause problems.
## 2. What is an Origin
An origin is the combination of scheme + host + port. If any of these is different in a URL then it is a different origin for example: 
- www.amazon.in and www.amazon.in/products are same origin
- www.amazon.in and www.api.amazon.in are different origin as there host is different
- https://www.amazon.in and http://www.amazon.in are differnt origin as their schemes are different
- localhost:5173 and localhost:5000 are different origins as they have different ports.
## 3. What is a Cross-Origin Request
A cross-origin request is a request send from a url of one origin to a url of another origin to access it's data. A request counts as a corss-origin when the a url accessing a different url whose scheme or host or port is different.
## 4. Simple Requests vs Preflighted Requests
- A simple request is a request which doesn't send an options request to the server to know if the url is allowed to request the data which it is accessing. The methods, headers and content-type all are simple in this case.
- A Preflighted request is a request which sends an options request first to the server to know if the url is allowed to access the given data and after receving the servers response and if it satisfied with the response then only it'll send the actual request. Even if any one of them (methods, headers or content-type) are of non-simple type the browser considers it as a preflighted response.
## 5. What is a Preflight OPTIONS Request
A pre-flight options request is a request sent by the browser to the server when a url is trying to access the data of a url of different origin when it is of a non-simple request. It is a request send by the browser first to see if the process the url is trying to do to the different origin url's data is allowed first. If the server sends a positive response then only the actual request is send otherwise the browser itself deletes the request.
## 6.  What Triggers a Preflight
A preflight can be triggered by a variety of reasons:
- **Non-simle methods**: when the url sends a non-simple method to access the data (example: `PUT`, `DELETE`, `FETCH`)
- **Custom headers**: when the url sends custom headers to access the data (example: `Authorisation:Bearer`)
- **Non-simple content type**: when the url sends a non-simple content type (example: `json`)
## 7. Key CORS Response Headers
- **Access-Control-Allow-Origin**: This header decides which origins may read the response. (It can be specific origins or `*` which would mean all origins may access the data.)
- **Access-Control-Allow-Methods**: This header decides which http methods are accepted. 
- **Access-Control-Allow-Headers**: This header decides which all custom headers are permitted. 
- **Access-Control-Allow-Credentials**: This header decides whether the `cookies/authorization` may be included. It should be `true`. This header cannot be made true with `Access-Control-Allow-Origin:*` as when the `allow-cred:true` and `allow-origin:*` this means that every origin can access the credentials. That means any malicious site has access to the credentials. 
## 8. Why it works in Postman or curl but not in the browser
**CORS** is entirely client-side (browser) security enforcement to prevent the JS from accessing the execution context. The server doesn't have any idea on which caller is a browser or which caller is a curl command. Therefore only browsers enforce the CORS policy. In case of postman or curl, they do not have a origin, same-origin policy or JS to protect itself from. They just simply send the reqeust and shows the output. The browser checks the response CORS's header and decides whether the JS is allowed to see it or not.

# Practical Investigation

### Observations

| Case | OPTIONS before main request? | Main request: success or fail? | CORS error in Console (if failed) | Headers reviewed (Request + Response) | Fix (if it failed) |
|---|---|---|-----------|---|---|
| 1. Simple GET | NO | success | - | origin: null, creds: true| - |
| 2. Simple POST (form) | NO | success | - | origin: null, creds: true| - |
| 3. Non-simple POST (JSON) | YES | success | - | origin: null, creds: true | - |
| 4. JSON + credentials | YES | fail | `ACAO` - `wild card`vand `request's credentials`:`true` | origin:`*`, creds:`true`| change `ACAO` to specific origins |
---
# Real-World CORS Problem – Learning from a Developer
- ## Problem: 
    The developer was building a web app for a client using `content management system` as its backend. The tests ran locally showed no error and ran without error during the local testing phase even while connecting the back end and front end. But after deploying the web app it started to show errors as the origin * couldn't access the front end. This happened due to the CMS needing specific origin in the backend and not * as the production configuration expects it.
- ## Solution Taken: 
    The developer tried giving the origin specific front end URL as they saw the network CORS error message mentioning the origin *. As they ran again after using the specifc URL the code worked perfectly
- ## My Takeaway: 
    While using CMS as backend, using credentials or not, it's advised to always give the specific URL's list as CORS which would reduce the error that might pop-up related to CORS and browser specifically blocks all the `origin *` with `creds true`.
