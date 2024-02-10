### An Old Hacker's Tips On Staying Employed

- Don’t take losing a job or having to move jobs too personally, because it’s almost certain to happen at some point.
- Develop Your Personal Brand
- Make sure your boss is afraid: a willingness to do what needs to be done, and to take ownership of things. If possible, important things. In some cases, even things that no one wants to do.
- Make Your Boss Happy
- The "Do It Anyway" Principle
- So the Two-And-Done rule was born, wherein I will state my case the first time, and if whoever is arguing to the contrary does not agree after hearing my position, I’ll let it go. But the next time the opportunity comes up, I will argue my point again. Maybe allowing for a gap of time for people to consider my original point, or maybe allowing me time to refine and rephrase my ideas to be more convincing. If I fail to get my way after the second time though, I am done.

### Qualifications of an Architect

- You should be able to walk into a company and begin categorizing and triaging both the technology and business needs into an easily digestible list that you can continue to iterate over during your time as an Architect.
- You should be able to come up with both a "Best Case/Perfect World" architectural model and a "Path of Least Resistance" architectural model for each of the needs on that list.
- You should be able to tie those business and technological needs with the strategy of the company, and if you can't - you should realize that need probably isn't actually a need.
- You should understand that the architectural model that's chosen to move forward on by the company will be less than optimal, and probably be closer to the "Path of Least Resistance" model that you've developed.
- You should know how to properly estimate the cost of an architectural proposal and if it's anywhere in the ballpark of what your company may spend in implementing it.
- You should learn to read all manner of business agreements/SLAs/etc. The people responsible for understanding these probably don't care enough to help you with them.
- You should learn to split up architectural proposals in phases. This is the best strategy for long-term success of your architecture, as proposing one giant solution or change to the architecture probably isn't going to fly with upper management - but proposing in bit-sized chunks usually will - as management loves to make business decisions on Sunk Costs.
- You should be able to communicate effectively with both tech and business professionals. With this, you should be able to convince them of why architectural decisions are being made without causing controversy. Ultimately, if those architectural decisions are to be successful - you need their cheerleading for them.
- You should recognize that even though you sit in a senior position, there are people all around you that have varying experiences, expertise, and niches and you should rely heavily on their counsel and advice. This is ultimately a team effort.
- You should have some communication infrastructure you put in place to keep both management and stakeholders aware of your progress and plans.
- You should understand how the changes you make in any given system can directly impact multiple people's jobs, processes and other systems - and understanding this is the epitome of what an architect does.
- You should understand that a lot of your job is going to be based around refactoring and making small, gradual changes to an architecture and be ready to receive criticism for any changes you do propose.
- You should acknowledge that the architecture you are being given was probably what could be obtained at the time, and should refrain from judging other's architectural decisions - as business needs and budgets constantly change.

### What mental models do you use everyday?

- When in doubt, chose the adventure
- What's the right thing to do here versus the comfortable one?
- To make a thing inevitable, do as much as you can right now. For instance, to make it inevitable to take a letter to my office the next day, I put it inside my shoes.
- If it’s popular and most people agree with it, walk on. Interesting and unfashionable is where the real fun is - and, likely, the values of tomorrow.
- Do something everyday that you're afraid of

### [Things I learnt the hard way](https://blog.juliobiason.net/thoughts/things-i-learnt-the-hard-way/)

- Spec first, then code: If you don't know what you're trying to solve, you don't know what to code.
- Write steps as comments.
- Gherkin is your friend to understand expectations.
- Unit tests are good, integration tests are gooder.
- Make tests that you know how to run on the command line.
- Don't use Booleans as parameters
- Sometimes, it's better to let the application crash and do nothing
- Data flows beat patterns
- Always use timezones with your dates
- Always use UTF-8: Always convert your strings to UTF-8; save them in the database as UTF-8; return UTF-8 on your APIs.
- One commit per change

### General lessons from scaling large systems at Microsoft

- A transparent and optimizable runtime is as important as availability of good programming abstractions (e.g., lisp macros, lambdas, etc.)
  - When developing systems that must scale _immediately_ to millions of users, it is necessary to be able to reason both about what the code means (e.g., "this code sorts numbers") AND how the code behaves (e.g., "slower for in-memory sorting but much faster over network").
- Strong distinction between testing and developments modes is crucial
  - if you’re performing queries on streams of data, test mode should not cache queries, while prod mode might.
- Robust and comprehensive test suites are a mandatory prerequisite to development.
  - you should do this _before_ you write code, or your debugging will basically consist of running something over a cluster of dozens of badly-behaving machines, and simply waiting for the error
- Use simple tools that you understand deeply, and can reuse constantly.
- Deployment should be a single command.

### Effective Pagination

- `https:<your-shop-domain>.myshopify.com/admin/products.json?page=25&limit=100`
- This would generate a SQL query like this: `SELECT * FROM products LIMIT 100 OFFSET 2400`
- The above query scales poorly because the bigger the offset, the slower the query.
- Relative cursor pagination remembers where you were so that each request after the first continues from where the previous request left off. The downside is that you can no longer jump to a specific page. The easiest way to do this is remembering the id of the last record from the last page you’ve seen and continuing from that record, but it requires the results to be sorted by id. With a last id of 67890 this would looks like: `SELECT * FROM products WHERE products.id > 67890 ORDER BY products.id ASC LIMIT 100`
- Sorting by something other than id is possible by remembering the last value of the field being sorted on. For example, if you’re sorting by title, then the last value is the title of the last record in the page. If the sort value is not unique, then if we used it alone we would potentially be skipping records.
- The solution is to set a secondary sort column on a unique value, like id, and then remembering both the last value and last id. In that case the query for the second page would look like this:

```SQL
SELECT * FROM `products` WHERE (`products`.`title` > "Pants" OR (`products`.`title` = "Pants" AND `products`.`id` > 2)) ORDER BY `products`.`title` ASC, `products`.`id` ASC LIMIT 2
```

- To ensure the query is performant as the number of records increases you’d need a database index set up on title and id

### Practicing Programming

1. Write your resume. List all your relevant skills, then note the ones that will still be needed in 100 years. Give yourself a 1-10 rating in each skill. Math, CS, writing, and people skills are for the most part timeless, universal skills. Most specific technologies, languages and protocols eventually expire, to be replaced by better alternatives.
2. Make a list of programmers who you admire. Try to include some you work with, since you'll be borrowing them for some drills. Make one or two notes about things they seem to do well — things you wish you were better at.
3. Go to Wikipedia's entry for computer science, scroll down to the "Prominent pioneers in computer science" section, pick a person from the list, and read about them. Follow any links from there that you think look interesting. [List of Computer Scientists](http://en.wikipedia.org/wiki/List_of_computer_scientists)
4. Read through someone else's code for 20 minutes. For this drill, alternate between reading great code and reading bad code; they're both instructive. If you're not sure of the difference, ask a programmer you respect to show you examples of each. Show the code you read to someone else, and see what they think of it.
5. Make a list of your 10 favorite programming tools: the ones you feel you use the most, the ones you almost couldn't live without. Spend an hour reading the docs for one of the tools in your list, chosen at random. In that hour, try learn some new feature of the tool that you weren't aware of, or figure out some new way to use the tool.
6. Pick something you're good at that has nothing to do with programming. Think about how the professionals or great masters of that discipline do their practice. What can you learn from them that you can apply to programming?
7. Get a pile of resumes and a group of reviewers together in a room for an hour. Make sure each resume is looked at by at least 3 reviewers, who write their initials and a score (1-3). Discuss any resumes that had a wide discrepancy in scoring.
8. Listen in on a technical phone screen. Write up your feedback afterwards, cast your vote, and then talk about the screen with the screener to see if you both reached the same conclusions.
9. Conduct a technical interview with a candidate who's an expert in some field you don't know much about. Ask them to explain it to you from the ground up, assuming no prior knowledge of that field. Try hard to follow what they're saying, and ask questions as necessary.
10. Get yourself invited to someone else's technical interview. Listen and learn. Try to solve the interview questions in your head while the candidate works on them.
11. Find a buddy for trading practice questions. Ask each other programming questions, alternating weeks. Spend 10 or 15 minutes working on the problem, and 10 or 15 minutes discussing it (finished or not.)
12. When you hear any interview coding question that you haven't solved yourself, go back to your desk and mail the question to yourself as a reminder. Solve it sometime that week, using your favorite programming language.

### gRPC (Remote Procedure Call)

- Client Library : One library for popular languages.
- Protocol : HTTP/2 (hidden implementation)
- Message Format : Protocol buffers as format
- gRPC modes
  - Unary RPC : client server request response.
  - Server streaming RPC : client make one request to server, but it is expecting a stream of response from the server.
  - Client streaming RPC : Client is constantly sending information, e.g. uploading a huge file and server can optionally respond or not
  - Bidirectional streaming RPC : gaming is another example, chatting

### Sidecar pattern

- Two or more processes living in the same host can communicate with each other using IPC
- Pros: Decoupling thick libraries and references
  - Applications can evolve independently
  - Polyglot : Each sidecar application can be written in its own language
- Cons : Latency; Complexity
- One of the most used cases of sidecar pattern is the service mesh : such as the linkerd, and envoy, where process or a container sits next the application and proxy the request and enables logging, service discovery and so much cool features which makes clients become thin in a microservices architecture.

### Lessons of a startup engineer

- Every decision is a business decision
- Build exactly what you need and nothing more. This can come up in many ways:
  - Excessive services (e.g API + web app + admin tools, when 1 monolith would have sufficed)
  - Anticipating a feature that's not being built yet
  - Anticipating data model correctness
- Invest in fundamentals
  - Quick and easy deploys - one click or script with easy revert plan
  - Fast deploys - ship a feature and be able to check it while it's still in your headspace
  - Error monitoring (Sentry) - know when something broke in production
  - Linting
  - Basic testing
  - CI
- Stick to boring and simple (technology)
- Do it right the first time

### Unix and Microservice Platforms

- Consider all the functionality expected of a modern, production-caliber web service. The list is massive, but to name a few: authentication, authorization, metrics, monitoring, alerting, provisioning, deployment, testing, logging, and tracing! And these requirements are fractal. If you zoom in, you'll find each area is rich enough to have more than a couple of venture-funded startups attempting to address just that one area. Even if you use this functionality-as-a-service from one of these vendors, you still need to do the integration work.
- This is where the second sentence of the Unix philosophy comes in : write programs to work together.
- For example, instead of configuring metrics once for each service, you might write a single reusable library that can be embedded into each service to act as the glue between that service and the common metrics service.
- Twitter did precisely this with their [Finagle RPC system](https://twitter.github.io/finagle/). Functionality like request metrics, traffic splitting, retries, and more was coded once and then shared across every service at Twitter.
- Service meshes. While still nascent, service meshes hold promise. They do one thing well: enhance network traffic with important operational functionality. Service meshes work together with other programs: by running as sidecars, embeddable with each service. And they utilize universal interfaces: TCP, HTTP, etc. As a result, a service mesh is coded once and immediately applicable to every service in every language.

### Joel's 12 steps for better code

1. Do you use source control ?
2. Can you make a build in one step ?
3. Do you make daily builds?
4. Do you have a bug database?
5. Do you fix bugs before writing new code?
6. Do you have an up-to-date schedule?
7. Do you have a spec?
8. Do programmers have quiet working conditions?
9. Do you use the best tools money can buy?
10. Do you have testers?
11. Do new candidates write code during their interview?
12. Do you do hallway usability testing?

### How do you do capacity planning

- Your application use resources. The main ones are :
  - disk I/O
  - network bandwidth
  - CPU
  - databases
  - locks
- It's easy to measure how much CPU your application is using. you can use the clock_gettime sys call
- You can use `dstat` and `iftop` to see how much network gets used.

### Effective Logging
- we should log things that had happened instead of things we are going to do.
- `log.info("Made request to REST API. [url={}]", url)`
- If you did some operations which actually worked, but there have been some issues - that’s a `WARNING`. But if you did some operation and it simply didn’t work - that’s an `ERROR`.
- `INFO` is for business, `DEBUG` is for technology. For instance, `DEBUG | Started cron job to send newsletter of the day. [subscribers=24332]`