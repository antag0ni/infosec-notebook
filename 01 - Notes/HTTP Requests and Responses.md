## HTTP Request

![[Pasted image 20250804195128.png]]

|**Field**|**Example**|**Description**|
|---|---|---|
|`Method`|`GET`|The HTTP method or verb, which specifies the type of action to perform.|
|`Path`|`/users/login.html`|The path to the resource being accessed. This field can also be suffixed with a query string (e.g. `?username=user`).|
|`Version`|`HTTP/1.1`|The third and final field is used to denote the HTTP version.|

The next set of lines contain HTTP header value pairs, like `Host`, `User-Agent`, `Cookie`, and many other possible headers. These headers are used to specify various attributes of a request. The headers are terminated with a new line, which is necessary for the server to validate the request. Finally, a request may end with the request body and data.

**Note:** HTTP version 1.X sends requests as clear-text, and uses a new-line character to separate different fields and different requests. HTTP version 2.X, on the other hand, sends requests as binary data in a dictionary form.

## HTTP Response

![[Pasted image 20250804200147.png]]

The first line of an HTTP response contains two fields separated by spaces. The first being the `HTTP version` (e.g. `HTTP/1.1`), and the second denotes the `HTTP response code` (e.g. `200 OK`).

Response codes are used to determine the request's status. After the first line, the response lists its headers, similar to an HTTP request.

Finally, the response may end with a response body, which is separated by a new line after the headers. The response body is usually defined as `HTML` code. However, it can also respond with other code types such as `JSON`, website resources such as images, style sheets or scripts, or even a document such as a PDF document hosted on the webserver.

## cURL

In our earlier examples with cURL, we only specified the URL and got the response body in return. However, cURL also allows us to preview the full HTTP request and the full HTTP response, which can become very handy when performing web penetration tests or writing exploits. To view the full HTTP request and response, we can simply add the `-v` verbose flag to our earlier commands, and it should print both the request and response.

The `-vvv` flag shows an even more verbose output.

## Browser DevTools

Whenever we visit any website or access any web application, our browser sends multiple web requests and handles multiple HTTP responses to render the final view we see in the browser window. To open the browser devtools in either Chrome or Firefox, we can click [`CTRL+SHIFT+I`] or simply click [`F12`]. The devtools contain multiple tabs, each of which has its own use. We will mostly be focusing on the `Network` tab in this module, as it is responsible for web requests.

If we click on the Network tab and refresh the page, we should be able to see the list of requests sent by the page:

![[Pasted image 20250804200617.png]]

As we can see, the devtools show us at a glance the response status (i.e. response code), the request method used (`GET`), the requested resource (i.e. URL/domain), along with the requested path. Furthermore, we can use `Filter URLs` to search for a specific request, in case the website loads too many to go through.

**Exercise:** Try clicking on any of the requests to view their details. You can then click on the `Response` tab to view the response body, and then click on the `Raw` button to view the raw (unrendered) source code of the response body.