### JWT

- JSON Web Token
- Completely Stateless
- The token consists of 3 Parts, Header, Data, Signature
- Pros:
  - Stateless; Great for APIs; Secure; Carry useful info (username); Can store info that derive UX; No need for centralized database
- Cons:
  - Sharing secrets in Microservices; Key management; very tricky to consume correctly; storage of Refresh tokens; Token Revocation and Control
- Asynchronous Access Token
- X-Content-Type-Options : nosniff. to avoid mime and media type sniffing
- [Caddy](https://caddyserver.com/docs/getting-started)

### Hacking out of a network Computerphile "hacking out"

- Here's how to replicate the experiments in this video. I'm going to assume you're on a Linux machine. Similar commands will be possible on a Mac but I don't own one. Let us imagine that there are four machines:
  - `host.org` -- the machine you are logged into
  - `webproxy.org` -- webproxy for the network you are logged into
  - `server.org` -- a server you own and have root access to
  - `target.org` -- the server you are trying to access
- On an open network you can access the top webpage `https://target.org` either through a browser or with curl `curl --max-time 3 https://target.org`
- **Experiment 1 -- the network you are on blocks `target.org` at its firewall**
- You can get round this in three ways
  - with an ssh tunnel used as a socks proxy
  - with sshuttle
  - with a VPN (but paid for VPNs are costly and free VPNs are awful)
- To simulate the firewall blocking `target.org` on your host you would add a rule to firewall -- for ufw on linux this looks like
  - `sudo ufw deny out to ${target.org IP} port https`
  - `sudo ufw deny out to ${target.org IP} port http`
- You can see that `curl --max-time 3 https://target.org` is now blocked
- To open a tunnel on port 1080 via server.org `ssh -f -ND 1080 username@server.org`
- Now you can do `curl --max-time 3 -x socks5h://127.0.0.1:1080 https://target.org`
- Don't forget to kill the ssh command before continuing.
- A better alternative is to use `sshuttle sudo sshuttle -r username@server.org ${target.org IP}/32` to just redirect specific traffic for `target.org` or to redirect absolutely everything `sudo sshuttle -r username@server.org 0.0.0.0/0`
- **Experiment 2 -- network also blocks port 22 so you can't ssh**
- If your network blocks port 22 (ssh) then you can get around this by redirecting ports on your server so you can ssh into a non standard port. So on server.org:
  - `sudo iptables -t nat -I PREROUTING -p tcp --dport 4444 -j REDIRECT --to-ports 22`
  - `sudo ufw allow in to port 4444`
- This redirects port 4444 to port 22 (ssh) and opens a hole in your firewall for port 4444.
- Now the ssh command you need on host.org looks like this: `ssh -f -ND 1080 username@server.org -p 4444`
- **Experiment 3 -- network blocks everything apart from their own web proxy**
- There are a number of ways you can get ssh through a web proxy. The catch is that the tunnelled traffic will end up on port 443 (https) not ssh.
- On `server.org` you will need to redirect port `443` to port `22` (this will cause problems if `server.org` is running HTTPS already).
- On server.org you would need
  - `sudo iptables -t nat -I PREROUTING -p tcp --dport 443 -j REDIRECT --to-ports 22`
  - `sudo ufw allow in to port 443`
- (In the video I do this differently as I'm using my server to pretend to be the web proxy here.) In the video I then use a command like this:
  - `ssh -o "ProxyCommand nc -X connect -x webproxy.org:3128 server.org 443" username@server.org`
- You can also do this with corkscrew or proxytunnel with slight variants in the commands.
- More advanced techniques
  - [Tunnelling over ICMP](https://en.wikipedia.org/wiki/ICMP_tunnel)
  - [Tunnelling over DNS](https://github.com/yarrick/iodine)
