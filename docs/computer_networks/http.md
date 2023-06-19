```
nc example.com 80
HEAD / HTTP/1.1
```

- HEAD allows to get the headers of the file.
- OPTIONS is supposed to give the list of methods that are accepted on the current url
- `Content-Length` is a header that must be contained in every response and tells the browser the size of the body in the response.
- `Content-Type` is also a non-optional header and tells you what type the document has. This way the browser knows which parsing engine to spin up.
- `Last-Modified` is a header that contains the date when the document was last changed.
- most servers also send out an ETag. ETag stands for entity tag, and is a unique identifier that changes solely depending on the content of the file. Most servers actually use a hash function like SHA256 to calculate the ETag.
- `Cache-Control` is exactly what it sounds like. It allows the server to control how and for how long the client will cache the response it received.
- `If-Modified-Since` permits the server to skip sending the actual content of the document if it hasn’t been changed since the date provided in that header.
- REpresentational State Transfer
- PUT request updates an already present record in collection
- POST request creates new record and adds it to the collection
- TCP has a big impact on how we should structure our request.
- IP allows us to talk to other machines on the internet while TCP allows us to have multiple streams of data between these two machines.
- TCP streams are distinguished by port numbers.
- **Head-of-line blocking (HOL blocking)** in computer networking is a performance-limiting phenomenon that occurs when a line of packets is held up by the first packet
- HOL blocking is, when one request is blocking other requests. A browser opens up upto six connections to a server.
- Request + Response : round-trip
- A proxy is the legitimate MITM and has many benefits such as:
  - saving bandwidth by adding additional compression
  - downsampling images.
  - aggressive caching.
- HTTPS = HTTP + TLS (SSL)
- TLS stands for Transport Layer Security
- TLS utilizes sth called chain of trust to ensure we are talking to the server we intend to talk to.
- A server identifies itself with a certificate that contains both the metadata about itself and fingerprint of its encryption key.
- HTTP/2 tackles HOL blocking.
- Header is compressed in HTTP/2
- multiplex: a system or signal involving simultaneous transmission of several messages along a single channel of communication
- combining multiple signals into a single signal.
- mixed-content problem: different assets such as images and iframes are sent using different protocol such as http rather than https.
- An origin comprises of scheme + host + port
- cross-origin fetch requests:
- Cross Origin Resource Sharing(CORS): CORS headers permit cross origin requests.
- If the Request header referer is on the Response header Access-Control-Allow-Origin it will be able to inspect the answer and use the data.
- Preflight requests:
- A CSRF token is a additional field appended to a form that has been put by the server and is stored in the server side as well.

## Netflix's CDN

- CDN: Two concepts
  - Distributed caching of web assets
  - Intelligent rendezvous of client (e.g. web browsers) to appropriate cache.
- We created our own adaptive bitrate algorithms to adapt to changes in throughput
- Constantly polling the bit rate that they are able to maintain
- On the appliances : firmware contain OS, applications & scripts. The Firmware's OS: FreeBSD, Applications: nginx, BIRD BGP routing

## Commands

- `netstat -an` : see the current state of sockets on your host
- `lsof -i -n` : gives the open Internet sockets when used with the -i option.
- 1MB = 1 _ 2^20 _ 8 bits
- 1Kb = 1000 bits

* In the terminal, you can use the **host** program to look up hostnames in DNS:
* IP addresses distinguish computers; port numbers distinguish _programs_ on those computers.

```
ncat 127.0.0.1 8000
GET / HTTP/1.1
Host: localhost
```

- Different status codes of HTTP:

* **1xx - Informational.** The request is in progress or there's another step to take.
* **2xx - Success!** The request succeeded. The server is sending the data the client asked for.
* **3xx - Redirection.** The server is telling the client a different URI it should redirect to. The headers will usually contain a **Location** header with the updated URI. Different codes tell the client whether a redirect is permanent or temporary.
* **4xx - Client error.** The server didn't understand the client's request, or can't or won't fill it. Different codes tell the client whether it was a bad URI, a permissions problem, or another sort of error.
* **5xx - Server error.** Something went wrong on the server side.

- **headers** Each header is a line that starts with a keyword followed by a colon and a value.
- **Cookies** are a Web feature that lets servers store data on the browser. To set a cookie, the server sends the `Set-Cookie` header. The browser will then send the cookie data back in a `Cookie` header on subsequent requests.
- `ncat -l 9999` to listen on port 9999
- Web servers using `http.server` are made of two parts: the `HTTPServer` class, and a request handler class
- query parameters are written as `key=value` and separated by `&` signs
- `urllib.parse` module defines functions that fall into two broad categories: URL parsing and URL quoting.
- The URL parsing functions focus on splitting a URL string into its components, or on combining URL components into a URL string.
- `urllib.parse.urlparse(urlstring, scheme='', allow_fragments=True)`
  Parse a URL into six components, returning a 6-tuple. This corresponds to the general structure of a URL: `scheme://netloc/path;parameters?query#fragment`
- HTTP URLs aren't allowed to contain spaces or certain other characters. So if you want to send these characters in an HTTP request, they have to be translated into a "URL-safe" or "URL-quoted" format.
- quoting means translating a string into a form that doesn't have any special characters in it, but in a way than can be reversed later
- Inside `do_POST`, our code can read the request body by calling the `self.rfile.read` method.
- `self.rfile.read` needs to be told how many bytes to read.
- A server usually needs to have a _stable (static) IP address_ so that clients can find it and connect to it.
- `runtime.txt` tells Heroku what version of Python you want to run
- `requirements.txt` is used by Heroku(through pip) to install dependencies of your application that aren't in the Python standard library.
- `Procfile` is used by Heroku to specify the command line for running your app.
- https://dashboard.heroku.com/apps/your_app_name/logs
- git push heroku master
- Being able to handle two ongoing tasks at the same time is called _concurrency_
- One thing a specialized web server can do is dispatch requests to the particular backend servers that need to handle each request. There are a lot of names for this, including _request routing_ and _reverse proxying._
- Splitting requests up among several servers is called _load balancing_
- Cookies are a way that a `server` can ask a `browser` to retain a piece of information, and send it back to the server when the browser makes subsequent requests
- Every cookie has a name and a value, much like a variable in your code; it also has rules that specify when the cookie should be sent back.
- The first time the client makes a request to the server, the server sends back the response with a `Set-Cookie` header. This header contains three things: a cookie `name`, a `value`, and some `attributes`. Every subsequent time the browser makes a request to the server, it will send that cookie back to the server. The server can update cookies, or ask the browser to expire them
- To set a cookie from a Python HTTP server, all you need to do is set the `Set-Cookie` header on an HTTP response
- Similarly, to read a cookie in an incoming request, you read the `Cookie` header
