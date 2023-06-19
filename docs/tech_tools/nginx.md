### Nginx

- Ngnix acts as both a server and a proxy.
- Nginx can operate in Layer 7 (http) or Layer 4 (tcp)
- Using **stream** context it becomes layer 4 proxy.
- Using **http** context it become layer 7 proxy.
- Block directive
- `sudo nginx`: to start nginx locally.
- `docker run -p 2222:9999 -e APPID=2222 -d nodeapp` (starts in detached state)
- When we create an upstream backend, there's a load balancing algorithm that's assigned by default. That is round robin.
- `sudo nginx -s reload`
- `sudo nginx stop`
- browser is one TCP connection away from nginx. nginx has created 4 tcp connection with the four app in the backend. (Layer 7 proxying)
- one of the load balancing algorithm is called ip_hash. It takes the ip address of the client (using consitent hashing) and finds out one of these four server and stick to it. That means all the request coming to the server from this one ip address will go to one and only one backend. (This is an example of stateful application / sticky session)
- in layer 4 proxying we just want to tell which backend we want to stream to.
- when nginx is layer 4 proxying .. browser connect to nginx .. nginx chooses to connect to one server in the backend. It picks up 1 out of 4. It communicates connection all the way to the backend.
- If we refresh the browser too fast it isn't changing the TCP connection. Sometimes when it changes the tcp connection nginx forwards it to other server in the backend.
- In this case use Telnet. Which will be creating a new TCP connection each time and thus nginx will be forwarding it to different server each time using round-robin
- Router configuring
- Virtual hosting is a method for hosting multiple domain names (with separate handling of each name) on a single server (or pool of servers)

### Spinning an Nginx Container

- Create the nginx.conf file in your directory.
- create the dockerfile with the following content

```Dockerfile
FROM nginx
COPY nginx.conf  /etc/nginx/nginx.conf
```

- `docker build -t nginxapp` (looks for a dockerfile in the current folder, -t is for naming)

### Layer 4 load balancing

- Takes content and forwards it based on basic rules, it knows ip and port and perhaps latency of the target service.
- Pros:
  - great for simple packet-level load balancing
  - Fast and efficient doesn't look at the data.
  - More secure as it can't really look at your packets. So if it was compromised no one can look.
  - Uses NAT
  - One connection between client and server NATed.
- Cons:
  - Can't do smart load balancing on the content, such as switch request based on requested media type.
  - Can't do microservices with this type
  - Has to be sticky as it's a stateful protocol

### Layer 7 load balancing

- This type of proxy actually looks at the content and have more context, it knows you are visiting the /users resources so it may forward it to a different server. Essential and Great for microservices, it knows the content is video:image etc.
- It can also cache.
- It's expensive because it has to decrypt and look and compute.
- `haproxy -f tcp.cfg`