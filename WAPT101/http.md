---
permalink: /wapt101/http/
title: HTTP protocol
parent: Web Application Security 101
nav_order: 3
---

[<< Components for a web application](/wapt101/components/){: .btn .btn-outline }

HTTP protocol
=============

Before diving into web application, we need to first understand the protocol – or language – used between the client and the server to communicate, i.e. the __Hypertext Transfer Protocol__, also known as __HTTP__.

In this course, I will only cover the basis of the HTTP protocol, just enough to understand the rest of the course. However, I would highly recommend you to spend some time on the [HTTP/1.1 RFC](https://www.ietf.org/rfc/rfc2616.txt) document to have a better understanding of the protocol.


Communication client - server
-----------------------------

Four elements are required for a communication to happen:

* The sender: your browser, e.g. IE, Edge, Firefox, Google Chrome, etc.
* The receiver: the web server that will deliver the pages, images, etc.
* The message: The HTTP request and HTTP response
* The channel: your network (wifi/cable)

For instance, you are on your browser and you want to read what's new on your favorite website. You therefore type http://9gag.com in the URL bar and hit enter to have your daily dose of memes. But what did actually happen in the background?

Without going too much in detail with the [TCP/IP stack](https://en.wikipedia.org/wiki/Internet_protocol_suite), your browser first need to know to whom to ask for the page, images, etc. For this, it will use the [DNS protocol](https://en.wikipedia.org/wiki/Domain_Name_System) to convert the domain _9gag.com_ into an IP address (e.g. 52.29.229.140 for _9gag.com_).

Once the IP address known, the browser look at the _scheme_ used, i.e. the text entered before the `://`. Usually, browsers accept at least `http://` and `https://`. By default, when using the scheme HTTP the browser will uses the port 80 and when using the scheme HTTPS the browser will uses the port 443. A port is a number used to identify to which application is the communication intended, here in this case, the web server. Users can specify a port by typing `:port-number` after the domain, e.g. http://9gag.com:80.

> Note: It is important to note that HTTPS is not a protocol on its own, but rather a combination of two protocols. Whenever the browser find the _scheme_ `https://`, it will first establish a SSL/TLS tunnel in which the HTTP communication will transit securely if everything is configured properly.

With the IP address and the port number, the browser can now establish a [TCP connection](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Connection_establishment) between the web server and the browser itself. Any data sent by the browser to this connection will be received by the web server, which will parse and try to interpret it. If needed, the web server will then send data to the established connection that will be received by the browser, which will also parse and try to interpret.

In essence, the browser will use the HTTP protocol to request some resources from the web server and display them to the user within its graphical interface. The protocol works on the simple concept of _request_ - _response_.


HTTP request
------------

The HTTP request is split in two part, the _header_ and the _body_ (optional). Both parts are separated by an empty line.

### Header

The header first start with a _request line_ followed by 0, 1 or more _header fields_. The request line contains 3 elements: the __method__ (also known as _verb_), the __resource__ and the __version__.

The __methods__ indicate the desired action to be performed on the identified resource [[source](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)]. HTTP version 1.1 has 8 defined methods, the 2 main methods being _GET_ and _POST_. Both can be used to get data (e.g. HTML pages, images, etc) or to send date (e.g. send a comment, upload a file, etc). The main difference is that HTTP _POST_ requests supply additional data from the client (browser) to the server in the message body. In contrast, _GET_ requests include all required data in the URL [[source](http://www.diffen.com/difference/GET-vs-POST-HTTP-Requests)].

The __resource__ contains a _path_ and optionally a _query_. The path should always start with a `/` (slash). The query contains variables and always start with a `?` (question mark). The variables are (often) separated with a `&` (ampersand). Later, we will see that we can actually customise the structure to make it more elegant and readable.

The __version__ indicate what version of the HTTP protocol the browser is talking. The most widely used version enabled by default in major browser is `HTTP/1.1`.

There are many __header fields__ available. One is mandatory since HTTP version 1.1, the `Host` header. It is possible to have two websites hosted on the same server (same IP address). Therefore, in order to differentiate between the websites, the `Host` header has been introduced. Another one often used is the `User-Agent` header. It is meant to specify what browser is sending the request. This allow a website for instance to know whether the user is on a laptop or a mobile phone. Another interesting header is the `Cookie` field, used to keep track of the different sessions between users. We will discuss that concept later.

> Note: There is a additional information that could be used in the URL called the _fragment_, placed after a _#_ (hashtag). This value is never sent to the web server and only available to the browse.

### Body

The body contains additional data for the query that will be processed by the underlying framework/scripting language. The body should be used only with a _POST_ request.

Like GET variables, POST variables could be separated with `&` (ampersand), but whenever the data gets too big, for instance when sending a file to the web server, there is another format to send data, i.e. the [Multipart Content-Type](https://www.w3.org/Protocols/rfc1341/7_2_Multipart.html). Here, the variable are split by a boundary defined in the HTTP header `Content-type`. The third most common way to send data in POST request is in a JSON format: `{"variable_name_1": "data", "variable_name_2": "data", ... }`. We will get back to JSON later in this course.

Whenever the body is used, the size of the entity body should be indicate in the `Content-Length` header except in specific cases described [here](https://www.w3.org/Protocols/rfc2616/rfc2616-sec4.html#sec4.4).


HTTP response
-------------

The HTTP response is also split in two part with a _header_ and _body_ separated with an empty line.

### Header

The first line of a Response message is the _Status-Line_, consisting of the _protocol version_ followed by a numeric _status code_ and its associated _textual phrase_. The Status-Code element is a 3-digit integer result code used to understand how the web server handled the request. The Reason-Phrase is intended to give a short textual description of the Status-Code.

The first digit of the Status-Code defines the class of response. The last two digits do not have any categorization role. There are 5 values for the first digit:

* __1xx: Informational__ - Request received, continuing process
* __2xx: Success__ - The action was successfully received, understood, and accepted
* __3xx: Redirection__ - Further action must be taken in order to complete the request
* __4xx: Client Error__ - The request contains bad syntax or cannot be fulfilled
* __5xx: Server Error__ - The server failed to fulfill an apparently valid request

The following line(s) are response _header field_. The response header fields allow the server to pass additional information about the response which cannot be placed in the Status-Line. These header fields give information about the server and further information about the resource requested [[source](https://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html)].

Here is a list of [header fields](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields).

### Body

The body contains the data requested. It could be a HTML page, an image, a JavaScript script, some variables in a JSON format, a file to download, anything.


Examples
--------

Now that we reviewed the basic of the HTTP protocol, let's have some examples. Here is what happen whenever you type `http://www.example.com/`: First, your browser will first translate the domain `www.example.com` into the IP `93.184.216.34`. It then generates the following HTTP request:

```
GET / HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
<empty line>
<empty line>
```

Since no port has been specified and that the scheme given it `http://`, the HTTP request is sent to the IP `93.184.216.34` on port `80`.

Upon request, the server look at the `Host` in order to select the right virtual host (in case more than one is hosted on that IP), it then look at the resource requested, in this case `/`. By default, if no path are specified, the file `index.html` (or `.php`, `.aspx`, etc) is used (this default behaviour can be modified in the web server configuration.)

The request didn't contain any query, so the HTTP will directly read the content of the file or pass it to the framework if any. Once the final HTML page generated, the web server will wrap it in the HTTP format as follow:

```
HTTP/1.1 200 OK
Date: Thu, 20 Oct 2016 21:36:22 GMT
Server: ECS (ewr/1445)

<!doctype html>
<html>
<head>
    <title>Example Domain</title>
    [...]
</head>
<body>
    <div>
        <h1>Example Domain</h1>
        <p>This domain is established to be used for illustrative examples in documents. You may use this domain in examples without prior coordination or asking for permission.</p>
        [...]
    </div>
</body>
</html>
```

The response body is then parsed by the browser to be graphically displayed. If other resources are linked, such as CSS, JS or images, the browser will generate new HTTP requests to load those resources in the page.

__LAB 1: USING NETCAT TO SEND YOUR FIRST HTTP REQUEST__

Open your terminal, and type the following command:

```
$ dig www.example.com @8.8.8.8
```

This command will send a DNS query to resolve the domain www.example.com with the DNS server 8.8.8.8 (Google public DNS). You should receive an answer with the IP.

```
[...]

;; ANSWER SECTION:
www.example.com.	26543	IN	A	93.184.216.34

[...]
```

Now that we know the IP of the server, we can use netcat to send our first HTTP request. In your terminal, type the following command:

```
nc 93.184.216.34 80
```

This will create a TCP connection with the server 93.184.216.34 (www.example.com) on the port 80. If the connection is properly established, you should have your terminal waiting for you to type your message to the web server. Type the following:

```
GET / HTTP/1.1
Host: www.example.com


```
 Note the two extra empty line after the header field _Host_ to indicate the end of the HTTP request. If everything goes fine, you should have received the HTTP response with the header and the body that contains the HTML page for http://www.example.com/:80.

 If the TCP connection is still open (i.e. if you didn't get back to your local environment), you can close it by hitting `[ctrl] + c`.


Caching
-------

In order to increase the loading performance, HTTP/1.1 introduced different caching mechanisms to reduce the amount of requests and/or to reduce the size of the response. For this, HTTP uses header to give a validity period (with the header `max-age`) and an identifier (with the header `ETag`).

Whenever the browser requests a resource, the web server will generate the response's body, calculate a token (usually a hash or fingerprint of the body) and specify it in the `ETag` header. The next time the browser is requesting this same resource, it will add a header field with the ETag value. The web server will then compare the given ETag and the one for the requested resource. If the ETag is the same, it means that the resource hasn't change and the server doesn't need to send the content again. Instead, the browser will just retrieve the file in its local cache.

The `max-age` goes one step further by preventing the request itself to be sent. The header specify for how long the resource should be stored until the browser has to request it again if needed. For instance, if the user access a page that contains a linked CSS, the browser will send a HTTP request for that resource. The server will send back a HTTP response that contains the CSS sheet with the header field `Cache-control: max-age=86400`. If later the user access a page with the same CSS sheet linked within a day, the browser will not send a new HTTP request for that resource since it hasn't expired yet (60 seconds x 60 minutes x 24 hours = 86400 seconds).

There are more straight forward solution for the cache management with HTTP, like for instance with the `If-Modified-Since`, where the browser tells the server what is the latest version it has, and the server send or not the resource accordingly, but in general you got the idea: caching is meant for performance, either by preventing the request to be sent or by reducing the response size.


Cookies
-------

HTTP is a stateless protocol. This means after one full transaction (request - response), the web application won't be able to recognise/identity you with your subsequent transaction, event if it happens right after the first one. Your IP address is not a reliable enough information to identify a person (see [NATing](https://en.wikipedia.org/wiki/Network_address_translation) or [public IP lease](https://en.wikipedia.org/wiki/Network_address_translation)). Although the [EFF](https://www.eff.org/) did some [research](https://www.eff.org/deeplinks/2010/01/primer-information-theory-and-privacy) to identify with a relative accuracy an Internet user based on its User-Agent but this is another topic.

This means there is no concept of _session_ and _session variable_ with the HTTP protocol. Let's illustrate the limitation with the following example. You have developed a website that can be displayed in different languages. The user can select which language he wants to read. Once the language selected, the text change to the appropriate language. Now if the user click on a link, the new page will not be able to know which language the user has selected previously unless the value is sent in a HTTP GET or POST parameter. This would be quite complicate to implement as for each link, the user would have to add the language value either in the URL of the link, or in a hidden text input if the user sends a form.

In order to prevent the user to constantly send the language to use in POST/GET parameters, cookies were introduced. Instead of building a page in a way that it constantly send the value, it let the browser send automatically the value thanks to its dedicated cookie feature. It works like this: whenever the header of a HTTP response contains the field `Set-Cookie`, the browser will store in what's called a cookie jar (basically a local database) the value. For instance, if the HTTP response contains `Set-Cookie: lang=EN`, the browser will save in its cookie jar the variable `lang` with the value `EN`. Now that the cookie is stored, anytime the browser will access the __same domain__, it will add into the header of the HTTP request the field `Cookie: lang=EN`. It is important to note that each cookie are linked to the host that deliver them. If a new cookie is set when browsing `https://google.com`, the browser will add that cookie in the header only whenever the destination host of the request is `google.com`. It is possible to add additional parameters whenever the cookie  is set in order to add some granular control over it:

* `Expires=<date>`: Set the maximum lifetime of the cookie.
* `Max-Age=<non-zero-digit>`: Set number of seconds until the cookie expires.
* `Domain=<domain-value>`: Specifies those hosts to which the cookie will be sent. If not specified, defaults to the host portion of the current document location (but not including subdomains). If a domain is specified, subdomains are always included.
* `Path=<path-value>`: Indicates a URL path that must exist in the requested resource before sending the Cookie header.
* `Secure Optional`: A secure cookie will only be sent to the server when a request is made using SSL/TLS and the HTTPS protocol.
* `HttpOnly`: HTTP-only cookies aren't accessible via JavaScript through the Document.cookie property, the XMLHttpRequest and Request APIs to prevent attacks against cross-site scripting (we will discuss this later).
* `SameSite=Strict` and `SameSite=Lax`: Allows servers to assert that a cookie ought not to be sent along with cross-site requests, which provides some protection against cross-site request forgery attacks (we will discuss this later).

([Source](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie))

Due to its nature, cookies are often use to store a session ID that is used to identify an authenticated user.


Encoding
--------

We have seen that some ASCII characters are used as delimiter or for special use in the HTTP protocol. For instance, the space character is used in the HTTP header to separate the method, the URI and the HTTP version. Or the new line feed is used to separate multiple HTTP header fields. All those special characters should be encoded in the HTTP header (and sometimes HTTP request body) in order to prevent them to be interpreted literally. For this, HTTP uses the percent-encoding, also known as URL encoding.

A percent-encoded octet is encoded as a character triplet, consisting of the percent character "%" followed by the two hexadecimal digits representing that octet's numeric value.  For example, "%20" is the percent-encoding for the binary octet "00100000" (ABNF: %x20), which in US-ASCII corresponds to the space character (SP). [[source](https://tools.ietf.org/html/rfc3986#page-12)]

Let say you want to send the post variable `message` with the value `Hello World!`, the HTTP body of the request will be encoded as follow:

```
message=Hello%20World%21
```

Note that the space character can also be replaced by the plus character (i.e. `+`) when used in URI or the HTTP request body.

Here is a website to easily URL-encode/URL-decode any text: http://www.url-encode-decode.com


Compression
-----------

In order to save bandwidth and thus increase the communication speed, web servers can compress the data transferred (requested resource). For this, the client first need to advertise what compression schemes it supports with the `Accept-Encoding` header field:

```
GET /dir/index.html HTTP/1.1
Host: www.example.com
Accept-Encoding: gzip, deflate
```

If the web server know one or more of the advertised schemes, it will decide whether the data will be compressed and which algorithm(s) to use. The data is then compressed and sent back to the client:

```
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 438
Connection: close
Content-Type: text/html; charset=UTF-8
Content-Encoding: gzip

[438 bytes of binary compressed data]
```

Multiple algorithms (tokens) are available by default [see [link](https://en.wikipedia.org/wiki/HTTP_compression#Content-Encoding_tokens)].


Authentication
--------------

Before digging into the different most used authentication mechanisms I would first like to define and compare _authentication_ and _identification_.

__Identification__ is the action of indicating who someone is. This is the purpose of a username; it is meant to link you, the user, to a profile in the database. When you give your username, you claiming an identity and thus identify yourself. Giving only a username wouldn't be secure, as many people would impersonate other user by simply providing a username (usually public information) to a server. Therefore, whenever we want to user the user is really the one he claims to be, we need to _authenticate_ the user.

__Authentication__ is the action of verifying or proving the identity of a user. Typically, this is whenever the user provide a pair of username and password. The username (login) is used to identify you,and password is used to authenticate you. The password is a secret that is (should be) shared only between you and the web site. The server consider therefor that if you know the password, since you are the only other person who knows it, it means that you own indeed the identity you are claiming. This model is quite simplistic and thus has flaws that we will discuss later.

Following are the most used authentication mechanism used to verify the identity of user.

### Basic Access Authentication

Basic Access Authentication is an authentication mechanism that transmits credentials as user-id/password pairs, encoded using [Base64](https://tools.ietf.org/html/rfc7617) in the header of the HTTP request.

The Basic authentication scheme is based on the model that the client needs to authenticate itself with a user-id and a password for each protection space also knwon as _realm_ [[source](https://tools.ietf.org/html/rfc7617)]. A _realm_ is a free-form string that is meant to differentiate areas in case multiple areas of the website is restricted with different credentials sets. The server will grant access to the resource requested only if it can validate the user-id and password for the _realm_ applying to the requested resource.

The Basic authentication scheme utilise the Authentication Framework as follows. First the user sends a normal request to access a resource on the web server (see chapter about HTTP). If the resource is part of a protected realm, the HTTP will not send the resource itself but a _challenge_. The challenge has the scheme name `Basic` and contains the `realm` and optionally a `charset`.

For instance, here is the request:

```
GET /dir/index.html HTTP/1.1
Host: www.test.com
```

and here is the response:

```
HTTP/1.1 401 Unauthorized
Date: Sun, 01 Jan 2017 01:00:00 GMT
WWW-Authenticate: Basic realm="Restricted Area 1"
```

where `Restricted Area 1` is the string assigned by the server to identify the protection space.

Upon reception of the response with the `WWW-Authenticate` header field, the browser will prompt its native login box with a text that usually contain the realm and a form where the user can enter its username and password. The browser will then take the username and password and construct the _user-pass_ by concatenating the user-id, a single colon (":") character, and the password. The result will then be encoded with the [BASE64 algorithm](https://en.wikipedia.org/wiki/Base64). Once done, the browser will send again the initial HTTP request but this time with the encoded _user-pass_ in the `Authorization` field. If the browser wishes to send the user-id "Aladdin" and password "open sesame", it would send the following HTTP request:

```
GET /dir/index.html HTTP/1.1
Host: www.test.com
Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
```

The server will then decode the _user-pass_ and compare it with the list of credentials it has (usually saved in a file called `.htpasswd`). If the credentials match, it will grant access to the requested resource.

The authentication scope of that authenticate session is obtained by removing all characters after the last slash ("/") character of the path component. Browser usually preemptively send the corresponding Authorization header field with requests for resources in that space without receipt of another challenge from the server.

For example, given an authenticated request to:

```
http://www.test.com/docs/index.html
```

requests to the URIs below could use the known credentials:

```
http://www.test.com/docs/
http://www.test.com/docs/test.doc
http://www.test.com/docs/?page=1
```

while the URIs

```
http://www.test.com/other/
https://www.test.com/docs/
```
would be considered to be outside the authentication scope.

The Basic authentication scheme is not a secure method of user authentication, nor does it in any way protect the entity, which is transmitted in cleartext across the physical network used as the carrier. The most serious flaw of Basic authentication is that it results in the cleartext transmission of the user's password over the physical network. Many other authentication schemes address this problem, such as _Digest Access Authentication_

### Digest Access Authentication

Like Basic Access Authentication, Digest Access Authentication is a challenge-response authentication mechanism. However, instead of sending the credentials encoded in BASE64, the username and password are concatenated with other values then the result is hashed. By default, the hash algorithm used is MD5.

Whenever the user send a HTTP request to a resource that is protected with Digest Access Authentication, the server send a response that contains a WWW-Authenticate with the following values:

* `Digest realm`: Name of the area protected.
* `qop`: Stands for Quality of Protection. It is a quoted string of one or more tokens indicating the "quality of protection" values supported by the server.
* `nonce`: A server-specified data string which should be uniquely generated each time a 401 response is made. It is meant to prevent the replay attack.
* `opaque`: A string of data, specified by the server, which should be returned by the client unchanged in the Authorization header of subsequent requests with URIs in the same protection space.

For instance, here is a HTTP request/response example on a resource that is protected with Digest Access Authentication:

```
GET /dir/index.html HTTP/1.1
Host: www.test.com
```

```
HTTP/1.0 401 Unauthorized
Server: HTTPd/0.9
Date: Sun, 10 Apr 2014 20:26:47 GMT
WWW-Authenticate: Digest realm="testrealm@host.com",
                  qop="auth,auth-int",
                  nonce="dcd98b7102dd2f0e8b11d0f600bfb0c093",
                  opaque="5ccc069c403ebaf9f0171e9517f40e41"
Content-Type: text/html
Content-Length: 153

<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Error</title>
  </head>
  <body>
    <h1>401 Unauthorized.</h1>
  </body>
</html>
```

The WWW-Authenticate header indicates the browser to prompt its native login box. The box does not differ from the Basic Authentication one. The user enter its credentials with the login "`Mufasa`" and password "`Circle Of Life`". Since the server sent two value for the `qop`, the browser can decide which protection to use, i.e. `auth` (authentication) or `auth-int` (authentication with integrity check). Let say the browser decide to go for the `auth` protection. The browser will then do the following computation:

* A1 = username:realm:password = `Mufasa:testrealm@host.com:Circle Of Life`
* A2 = method:URI = `GET:/dir/index.html`
* Initialize the request counter (`nc` = `00000001`)
* Generate a client nonce (`cnonce` = `0a4f113b`)
* Calculate the `response`: MD5( MD5(A1):nonce:nc:cnonce:qop:MD5(A2) )

```
MD5(A1) = MD5("Mufasa:testrealm@host.com:Circle Of Life") = 939e7578ed9e3c518a452acee763bce9
nonce = dcd98b7102dd2f0e8b11d0f600bfb0c093
nc = 00000001
cnonce = 0a4f113b
qop = auth
MD5(A2) = MD5("GET:/dir/index.html") = 39aff3a2bab6126f332b942af96d3366

MD5(MD5(A1):nonce:nc:cnonce:qop:MD5(A2)) = MD5("939e7578ed9e3c518a452acee763bce9:dcd98b7102dd2f0e8b11d0f600bfb0c093:00000001:0a4f113b:auth:39aff3a2bab6126f332b942af96d3366") = 6629fae49393a05397450978507c4ef1
```

Now that the response has been calculated, the browser send the following request:

```
GET /dir/index.html HTTP/1.1
Host: www.test.com
Authorization: Digest username="Mufasa",
               realm="testrealm@host.com",
               nonce="dcd98b7102dd2f0e8b11d0f600bfb0c093",
               uri="/dir/index.html",
               qop=auth,
               nc=00000001,
               cnonce="0a4f113b",
               response="6629fae49393a05397450978507c4ef1",
               opaque="5ccc069c403ebaf9f0171e9517f40e41"
```

On the server site, the credentials are stored in a file (.htdigest) with the following format:

```
username:realm:MD5(username:realm:password)
```

For instance, for the user `Mufasa` in the realm `testrealm@host.com` using the password `Circle Of Life`, the file .htdigest will contain the following line:

```
Mufasa:testrealm@host.com:939e7578ed9e3c518a452acee763bce9
```

The server has thus all the information needed to re-calculate the response and compare it with the given one in the HTTP request. If it matches, the server will send a HTTP response with the requested resource, e.g.:

```
HTTP/1.0 200 OK
Server: HTTPd/0.9
Date: Sun, 10 Apr 2005 20:27:03 GMT
Content-Type: text/html
Content-Length: 7984

[...]
```

For the following requested, the client (i.e. browser) will simply increment the `nc` and recalculate the response accordingly.

### Integrated Windows Authentication

Integrated Windows Authentication (IWA) uses the security features of Windows clients and servers. Unlike Basic or Digest authentication, initially, it does not prompt users for a user name and password. The current Windows user information on the client computer is supplied by the web browser through a cryptographic exchange involving hashing with the Web server. If the authentication exchange initially fails to identify the user, the web browser will prompt the user for a Windows user account user name and password [[source](https://en.wikipedia.org/wiki/Integrated_Windows_Authentication)].

IWA is not an authentication protocol. The server and the browser will negatiate which authentication protocol to use ([Kerberos](https://en.wikipedia.org/wiki/Kerberos_%28protocol%29#Protocol) or [NTLMSSP](https://en.wikipedia.org/wiki/NTLMSSP)) thanks to the [SPNEGO](https://en.wikipedia.org/wiki/SPNEGO) mechanism.

### Normal form authentication

Basic Authentication, Digest Authentication and Integrated Windows Authentication are built-in authentication mechanism already integrated in the browser and the server. However, most of the time, the authentication is handled by the application itself. For this, the application developer will create a form in the HTML page so that the user can enters its username and password. It is than up to the developer to decide how to identify and authenticate the user. Usually, developers tent to follow this workflow:

The client tries to access a restricted resource:

```
GET /dir/index.php HTTP/1.1
Host: www.test.com
```

Since the user is not yet authenticated, the server will redirect the user to the login page:

```
HTTP/1.1 302 Found
Location: https://www.test.com/dir/login.php
```

```
GET /dir/login.php HTTP/1.1
Host: www.test.com
```

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 2789

[...]
<form action="/dir/login.php">
  <input name="username" type="text">
  <input name="password" type="password">
  <button type="button">Login</button>
</form>
[...]
```

The browser will render a form where the user can enter a username and password. When the user click on the "Login" button, the browser will send the following POST request:

```
POST /dir/login.php
Host: www.test.com
Content-Type: application/x-www-form-urlencoded;charset=utf-8
Content-Length: 43

username=Mufasa&password=Circle%20Of%20Life
```

Upon reception, the web application will extract from the database the [salt](https://en.wikipedia.org/wiki/Salt_%28cryptography%29) and the hash for the user that has the (unique) username "Mufasa". It will then compute the hash based on the given password and extracted salt from the database:

```
hash = sha512(password + salt)
```

Then compare if the calculated hash match the extracted hash from the database. If it does, the user is authenticated and the web application creates a session for that user. In order to keep track of the user and link it to the session, the web application will set a unique session ID in a new cookie:

```
HTTP/1.1 302 Found
Set-Cookie: session=t8eBLufiuchL6cr4eqiA4La7lApieMou;Path=/dir/;Expires=Fri, 31-Aug-2019 12:00:00;Secure;HttpOnly
Location: https://www.test.com/dir/index.php
```

The user is then redirected and this time, the browser send the session ID:

```
GET /dir/index.php HTTP/1.1
Host: www.test.com
Cookie: session=t8eBLufiuchL6cr4eqiA4La7lApieMou
```

The web application verify that the session ID belong to a session. If yes, it then check if the user has the authorization to read the resource.

__LAB: Find password based on TCP Dump (basic auth)__

__LAB: Find password based on TCP Dump (digest)__


URL Rewriting
-------------

By default, the URI given in the HTTP header is a relative path to the requested resource. From a aesthetic point of view, the URI might not be very pretty or convenient. For instance the file extension or the GET parameter structure makes the URL more complicate than it should. Therefore, the web server allows the developers to change the structure and logic of the URI.

For instance, let say you have a blog where you can select the article based on the publication date and its title. You will typically have a URL like this:

```
https://example.com/blog/article.php?year=2017&month=08&day=31&title=Hello
```

With URL rewriting, it is possible to simplify the URL like this for instance:

```
https://example.com/blog/2017-08-31/Hello
```

Or any other format as long as the GET parameters are still present. In the web server config file, the developer need to implement the right regular expression to parse the URI and match it to the right resource with the right GET parameters.

Therefore, if you find the following URL:

```
https://example.com/incude/config/index.html
```

Maybe it is not a static HTML page, but rather the following resource:

```
https://example.com/include.php?file=config.php
```


HTTPS, SSL & TLS
----------------

HTTP Secure (HTTPS) is protocol that uses _HTTP_ over a "_secure_" connection using SSL (Secure Sockets Layer), or its predecessor, TLS (Transport Layer Security). The main goal of SSL/TLS is to _secure_ the communication by encrypting its so that only the client and the server can reads its content. If an attacker manage to eavesdrop the communication, (s)he should not be able to read the HTTP communication. Another interesting goal of SSL/TLS is to _authenticate_ the client and/or the server based on a _chain a trust_. Finally, the last goal of SSL/TLS is the message _integrity_ check that makes sure that the communication hasn't been altered or loss.

SSL/TLS uses [symmetric cryptography](https://en.wikipedia.org/wiki/Symmetric-key_algorithm) to encrypt the communication and uses [asymmetric cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) to exchange the key used for the encryption. The key exchange phase can be summarized as follow:

* The client advertises ([ClientHello](https://tools.ietf.org/html/rfc5246#section-7.4.1.2)) which version of SSL/TLS and what encryption algorithm (cipher suite) it can use.
* The server decides ([ServerHello](https://tools.ietf.org/html/rfc5246#section-7.4.1.3)) which SSL/TLS version and which cipher suite to use.
* The server sends ([Certificate](https://tools.ietf.org/html/rfc5246#section-7.4.2)) its [certificate chain](https://en.wikipedia.org/wiki/Chain_of_trust). The server certificate contains the public key that will be used later to encrypt the pre-master key (symmetric key for encryption). The certificate is [signed](https://en.wikipedia.org/wiki/Digital_signature) to verify its authenticity. If the chain cannot be trusted, the browser usually ask the user to validate whether it should proceed anyway or terminate the communication.
* The server indicate ([ClientKeyExchange](https://tools.ietf.org/html/rfc5246#section-7.4.5)) that it is done.
* The client uses the server's public key to encrypt the pre-master key and sends the encrypted value to the server ([ClientKeyExchange](https://tools.ietf.org/html/rfc5246#section-7.4.7)).
* The client and the server compute a _common secret_ based on the pre-master key. The _common secret_ will be used to derived the symmetric key for the communication encryption.
* The client indicates ([ChangeCipherSpec](https://tools.ietf.org/html/rfc5246#section-7.1))that from now on, the communication will be encrypted.
* The client sends an encrypted [Finished](https://tools.ietf.org/html/rfc5246#section-7.4.9) message that contains a hash and MAC over the previous handshake messages.
* The server decrypt the Finished message and verify the hash and the MAC, in which case it would mean the SSL/TLS handshake was successful.
* The server sends a [ChangeCipherSpec](https://tools.ietf.org/html/rfc5246#section-7.1).
* The server sends an encrypted Finished message.
* The client verify the Finished message.
* Handshake is finally complete.

> Note: I omitted several steps for brevity making this description inaccurate. If you want additional information you can look at the Wikipedia [page](https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_handshake) and the [RFC](https://tools.ietf.org/html/rfc5246).

Clients should store a list of [Certificate Authorities](https://en.wikipedia.org/wiki/Certificate_authority) (CA) that will be trusted by default. Those certificates contains the public keys of CA that is used to verify signed certificates in the [certificate chain](https://en.wikipedia.org/wiki/Chain_of_trust).

It is possible for an attacker to execute a [Man-in-the-Middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attack even with SSL/TLS implemented and thus being able to read the HTTP communication. For this, the attacker should wait for the [ClientHello](https://tools.ietf.org/html/rfc5246#section-7.4.1.2) from the client, then establish a SSL/TLS connection with the server. Once done, the attacker should then continue the SSL/TLS handshake with the client and generate a [self-signed certificate](https://en.wikipedia.org/wiki/Self-signed_certificate) for the [Certificate](https://tools.ietf.org/html/rfc5246#section-7.4.2) message. Since the certificate used is self-signed, it's authenticity cannot be verified. Therefore, current browser will display an error message and (sometimes) ask the user to trust the certificate anyway or simply terminate the communication. For clients without user interaction, the communication should be terminated by default.


Web Proxy
---------

Sometimes, you may want to have all you HTTP requests to go through one single server. This would grant you many advantages, such as:

* Monitoring and filtering the content for security reason, e.g. blocking all hacking related boards
* Logging all browsed website
* Caching heavy pages
* Accessing service anonymously since all



Sometimes, you also want to look at the entire HTTP request/response, including the headers. You might also want to intercept the request before being rendered in the browser or even repeat a specific package in order to alter a small of big part of the request.

Web proxy can offer such versatility. Initially meant for monitoring, web proxy ...


Burp Suite

Fiddler

ZAP


Browser
-------

The main tool available to communicate with HTTP and accessing websites is the web browser. The browser _generates_ HTTP requests, _parses_ the responses to _render_ the HTML/CSS page, _executes_ JavaScript scripts, update the cookie jar and manage the caching system.

HTTP Requests are generated when the user clicks or enters a link/URL. By default it will send a GET request, but if the button/link is part of a form, the browser will generate a POST request. When parsing the HTTP request, the header will be used to keep the cookie jar up to date and to fetch cached data if needed then the browser looks at the additional resources referred in the body (image, css, js, etc) and will create successively GET requests to access that resource and render/parse/execute it.

Nowadays browser embed by default useful tool to analyse the debug web applications. The _Web Inspector_ in Safari and the _Developer Tools_ in Chrome and Firefox. Those tools allow you to read/edit the source code (adding or deleting an element for instance) and to debug JavaScript with the possibility to create breakpoints and execute code directly in the console.

__LAB: Delete a div to read a document__

__LAB: Find password by executing a specific function__

[<< Components for a web application](/wapt101/components/){: .btn .btn-outline }
