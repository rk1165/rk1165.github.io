+++
title = 'Project Ideas'
date = 2025-10-21

+++

### Project ideas to work on

- Make a REST API for generating random social media usernames, add a database, user authentication, Stripe payments, and analytics.
- Build your own api gateway.
- password manager which will interact with bitwarden for creating/updating/deleting/reading from terminal?
- Make a bitcask-style DB.
- Make a git clone.
- Make a load balancer that handles OAuth2/jwt auth for the services it proxies.
- Make a Docker clone.
- Parallel torrent / file downloader should be fun. With progress bars and shit
- Booking.com scrape
- Sync between cross browsers - Apple notes and Google keep
- Travel plan together with friends
- Summarise job listing using gpt and try to find startup opportunities
- To get a better understanding of the kernel, students could:
  - print "hello world" during the boot process;
  - design their own scheduler;
  - modify the page-handling policy; and
  - create their own filesystem.
- Should implement the following:
  - an HTTP client and daemon
  - a DNS resolver and Server
  - a command-line SMTP mailer
- RSA is easy enough to implement that everyone should do it

### Portfolio vs Resume

- Every CS major should build a _portfolio_
  - A portfolio could be as simple as a personal blog, with a post for each project or accomplishment. A better portfolio would include per-project pages, and publicly browsable code.
  - Contributions to open source should be linked and documented.
- [Github is my resume](http://pydanny.blogspot.com/2011/08/github-is-my-resume.html)

### Chat APP

- Chat app. For the purposes of practicing concurrency, it's fine to just set up a stupid simple TCP connection that you can actually just use "netcat" to connect to on the command line; don't waste a lot of time here if you don't want to go down a large rabbit hole on making this slick.
- The chat server that is managing multiple connections and sending the received messages out to all the other users will have a nice amount of concurrency it in. Run the whole thing under the race detector, because you may not have an easy time triggering race conditions on your own, but the detector will do a good job of pointing out the problems.
- The server can just open a socket, and for all incoming connections wrap a bufio around it and have a goroutine reading lines. Another goroutine can be responsible for writing the lines. You don't need auth and you don't even need "names", or formatting, or "chat rooms", or anything else; just very raw messaging. (But those are all fine followups if you feel inclined!)
- To give you an idea what I'm thinking of here, it's probably on the order of ~500 lines when its done.
- I would recommend sending it to all the other users; if A says "hello", B and C get it but you don't send it back to A.
- Sending it to everyone in the first pass is easier, but sending it only to the one who didn't send it will be a good exercise.
