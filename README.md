# Nelly
What does Whoa Nelly mean?
Interjection. whoa, Nelly. an exclamation of surprise, especially one in response to an unexpected acceleration of speed.\
<img src="./images/horse.png" alt="drawing" />

## What is Nelly?

Nelly is an NGINX <img src="./images/nginx.png" alt="drawing" height="25" width="50"/> LUA code suite that allows your organization to describe and implement your rate limits at the edge layer.  

## Why Rate Limit?

1. Resource Management: Rate limiting helps in preventing server overload and ensures that a system's resources are used efficiently. By limiting the number of requests, companies can avoid performance issues and maintain a smooth user experience.

1. Security: Rate limiting is a common strategy to protect against various types of attacks, including Distributed Denial of Service (DDoS) attacks. It helps in mitigating the impact of excessive traffic by restricting the rate at which requests are processed.

3. Fair Usage: Rate limiting can be implemented to ensure fair usage of resources among all users. It prevents any single user or application from monopolizing resources and ensures a level playing field for all users.

4. Cost Control: In cloud computing or other pay-as-you-go services, rate limiting can be used to control costs by limiting the number of API requests or interactions. This is especially relevant in scenarios where companies are charged based on the volume of requests.

5. Compliance: Some services or APIs have usage limits imposed by regulatory requirements or service providers. Rate limiting helps in ensuring compliance with these limits.

6. Quality of Service: By controlling the rate of incoming requests, companies can maintain a consistent quality of service for users. This is particularly important for services where timely and reliable responses are crucial.

7. Preventing Abuse: Rate limiting is an effective way to prevent abuse, misuse, or unauthorized access to services. It discourages malicious activities such as brute-force attacks or scraping.

8. Stability: Ensuring a steady and controlled flow of requests contributes to the overall stability of a system. Uncontrolled spikes in traffic can lead to service disruptions, and rate limiting helps in preventing such issues.

## Wait, why at Edge?

Glad you asked!\
Traditionally rate limits have started out at the controller layer of applications, and not at edge infrastructure.
Usually your organization (with remarkable success) has an oops moment and quickly reacts to the need of rate limiting and governance by implementing rate limiting
inside their flagship product.
Firms grow, add more products and services, break up monoliths, and pretty soon the organization finds itself in an unfavorable position 
of having to re-implement rate limits across multiple products, multiple services and multiple languages.
Pushing rate limits to the "edge" layer, and giving it enough context to decision properly will allow uniform application of rate limits across all your 
products and services, and provide a central location for definition and configuration.   This makes it easier for your IT administration, product organization, and 
business organization to collaborate and set the definitions in a common and clear pattern. \
But my rate limiting at the controller has all the context it needs and at Edge it may not!  WRONG (or partially wrong).
With a few minor adjustments in that thinking, and some good code ^_^ you can implement very bespoke product rate limits at edge before it even hits your controller.  
For example, if you want to only allow updates to a PARTICULAR entity five times within a second, because updates generate cascading events
that are taxing/onerous on the systems and downstream systems, you CAN capture that here.  REST patterns allow you to target that particular
condition among many more.  Look at the below configuration in the How it works section!

## How to run Nelly to test it out

### Prerequisistes
1. Docker (for docker compose)
2. A healthy understanding of service discovery
3. A healthy understanding of Redis and how it can be used as a global caching data store
4. A healthy understanding of nginx
5. A healthy understanding of bash ^_^


### Directions
in the root source diretory execute:
```shell
./test.sh
```

The resultant should be something like:
```shell
joshuateitelbaum@Joshuas src % ./test.sh
[+] Running 7/7
 ✔ redis 6 layers [⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                                                                                                                                                        3.5s 
   ✔ 83c5cfdaa538 Pull complete                                                                                                                                                                          0.8s 
   ✔ af69b9847230 Pull complete                                                                                                                                                                          0.6s 
   ✔ 47328343a4f2 Pull complete                                                                                                                                                                          0.4s 
   ✔ a8bdd61c4004 Pull complete                                                                                                                                                                          0.9s 
   ✔ 6cd44fea95ad Pull complete                                                                                                                                                                          1.0s 
   ✔ 797e3a88bf94 Pull complete                                                                                                                                                                          1.1s 
[+] Building 1.9s (20/20) FINISHED                                                                                                                                                       docker:desktop-linux
 => [nodejs internal] load build definition from Dockerfile.nodejs                                                                                                                                       0.0s
 => => transferring dockerfile: 343B                                                                                                                                                                     0.0s
 => [nodejs internal] load .dockerignore                                                                                                                                                                 0.0s
 => => transferring context: 2B                                                                                                                                                                          0.0s
 => [nodejs internal] load metadata for docker.io/library/node:lts                                                                                                                                       0.7s
 => [nodejs 1/5] FROM docker.io/library/node:lts@sha256:844b41cf784f66d7920fd673f7af54ca7b81e289985edc6cd864e7d05e0d133c                                                                                 0.0s
 => => resolve docker.io/library/node:lts@sha256:844b41cf784f66d7920fd673f7af54ca7b81e289985edc6cd864e7d05e0d133c                                                                                        0.0s
 => [nodejs internal] load build context                                                                                                                                                                 0.0s
 => => transferring context: 26.83kB                                                                                                                                                                     0.0s
 => CACHED [nodejs 2/5] WORKDIR /usr/src/app                                                                                                                                                             0.0s
 => CACHED [nodejs 3/5] COPY package*.json ./                                                                                                                                                            0.0s
 => CACHED [nodejs 4/5] RUN npm install                                                                                                                                                                  0.0s
 => CACHED [nodejs 5/5] COPY index.js ./                                                                                                                                                                 0.0s
 => [nodejs] exporting to image                                                                                                                                                                          0.0s
 => => exporting layers                                                                                                                                                                                  0.0s
 => => writing image sha256:dbece47d76e6afb9b75d1a0585afd73de40ac6fa3a6edf7af55b63cdc07d7a98                                                                                                             0.0s
 => => naming to docker.io/library/sampleapp-nodejs                                                                                                                                                      0.0s
 => [nginx internal] load .dockerignore                                                                                                                                                                  0.0s
 => => transferring context: 2B                                                                                                                                                                          0.0s
 => [nginx internal] load build definition from Dockerfile.nginx                                                                                                                                         0.0s
 => => transferring dockerfile: 535B                                                                                                                                                                     0.0s
 => [nginx internal] load metadata for docker.io/openresty/openresty:latest                                                                                                                              0.7s
 => [nginx 1/5] FROM docker.io/openresty/openresty:latest@sha256:3be14f2f85081bf11b276faccb204999f90f9c04282b7a32c478b87b8ee0f11a                                                                        0.0s
 => [nginx internal] load build context                                                                                                                                                                  0.0s
 => => transferring context: 10.01kB                                                                                                                                                                     0.0s
 => CACHED [nginx 2/5] COPY nginx.conf /usr/local/openresty/nginx/conf/nginx.conf                                                                                                                        0.0s
 => [nginx 3/5] COPY rate_limit.lua /usr/local/openresty/lualib/rate_limit.lua                                                                                                                           0.2s
 => [nginx 4/5] COPY nelly_configuration.lua /usr/local/openresty/lualib/nelly_configuration.lua                                                                                                         0.0s
 => [nginx 5/5] COPY limits.json /usr/local/openresty/nginx/conf/limits.json                                                                                                                             0.0s
 => [nginx] exporting to image                                                                                                                                                                           0.1s
 => => exporting layers                                                                                                                                                                                  0.1s
 => => writing image sha256:c10af67babebbd09f2eb21fd39a7aef7f29a340fbaf50d0a5445d06200ebdda9                                                                                                             0.0s
 => => naming to docker.io/library/sampleapp-nginx                                                                                                                                                       0.0s
[+] Running 5/5
 ✔ Network sampleapp_default                                                                                                                            Created                                          0.0s 
 ✔ Container nodejs                                                                                                                                     Started                                          0.0s 
 ✔ Container redis                                                                                                                                      Started                                          0.0s 
 ✔ Container nginx                                                                                                                                      Started                                          0.0s 
 ! nginx The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested                                                  0.0s 
NodeJS app and openresty responding and warmed up..... starting tests
Testing massive GETS on /api/example
Testing lower plan limit
Got off 60 requests for lower plan limit
Testing upper plan limit
Got off 300 requests for upper plan limit
*****Suite success!!!*****
```

Hey you know what's totally awesome?  Docker.  Even if you never installed any of this before Docker Compose is pretty
damn awesome.  If you run the tests without ever installing the images, it will still work!  It's all idempotent on run,
so the system will pull the images, install them, and run them based on the Dockerfiles supplied.  Dependencies are
also aptly set up as well.

## How it works

Have a look at this file: https://github.com/bersama-systems/nelly/blob/main/src/sampleApp/limits.json \
I hope you find it interesting!!!\
It is!  
```json
[
  {
    "name": "get ",
    "verb" : "GET",
    "uri" : "\/api\/example",
    "limit_key":  ["ngx.var.http_x_account_id", "ngx.var.request_method", "ngx.var.uri"],
    "limits" : [
      {
        "condition" : {
          "name": "Plan Type 1",
          "lhs": "ngx.var.http_x_account_plan",
          "operator": "eq",
          "rhs" : "1"
        },
        "threshold": 300,
        "interval_seconds": 60
      },
      {
        "condition": {
          "name": "Fallback threshold",
          "lhs": "1",
          "operator": "eq",
          "rhs" : "1"
        },
        "threshold": 60,
        "interval_seconds": 60
      }
    ]
  },
  {
    "name": "get with id",
    "verb" : "GET",
    "uri" : "\/api\/example\/\\d+",
    "limit_key":  ["ngx.var.http_x_account_id", "ngx.var.request_method", "ngx.var.uri"],
    "limits" : [
      {
        "condition" : {
          "name": "Plan Type 1",
          "lhs": "ngx.var.http_x_account_plan",
          "operator": "eq",
          "rhs" : "1"
        },
        "threshold": 300,
        "interval_seconds": 60
      },
      {
        "condition": {
          "name": "Fallback threshold",
          "lhs": "1",
          "operator": "eq",
          "rhs" : "1"
        },
        "threshold": 60,
        "interval_seconds": 60
      }
    ]
  },
  {
    "name": "put with id",
    "verb" : "PUT",
    "uri" : "\/api\/example\/\\d+",
    "limit_key":  ["ngx.var.http_x_account_id", "ngx.var.request_method", "ngx.var.uri"],
    "limits" : [
      {
        "condition" : {
          "name": "Plan Type 1",
          "lhs": "ngx.var.http_x_account_plan",
          "operator": "eq",
          "rhs" : "1"
        },
        "threshold": 5,
        "interval_seconds": 1
      },
      {
        "condition": {
          "name": "Fallback threshold",
          "lhs": "1",
          "operator": "eq",
          "rhs" : "1"
        },
        "threshold": 1,
        "interval_seconds": 1
      }
    ]
  },
  {
    "name": "get ",
    "verb" : "GET",
    "uri" : "\/api\/example/composite_condition",
    "limit_key":  ["ngx.var.http_x_account_id", "ngx.var.request_method", "ngx.var.uri"],
    "limits" : [
      {
        "condition" : {
          "name": "Plan Type 1",
          "lhs": {
            "name": "Plan Type 1",
            "lhs": "ngx.var.http_x_account_plan",
            "operator": "eq",
            "rhs" : "1"
          },
          "operator": "or",
          "rhs" : {
            "name": "Plan Type 99",
            "lhs": "ngx.var.http_x_account_plan",
            "operator": "eq",
            "rhs" : "99"
          }
        },
        "threshold": 300,
        "interval_seconds": 60
      },
      {
        "condition": {
          "name": "Fallback threshold",
          "lhs": "1",
          "operator": "eq",
          "rhs" : "1"
        },
        "threshold": 60,
        "interval_seconds": 60
      }
    ]
  }
]

```

name: the name of the limit configuration\
verb: the HTTP verb that is part of the selector \
uri: the URI or path not including query parameters \
limit_key: how we UNIQUELY identify the counter in Redis.  Note that it uses:  Account ID (from headers), request method, and the URI \
condition: the "statement" that is evaluated dynamically (to a simple boolean ) in the limit to determine which limit to pick.  Conditions may contain other conditions or be a string to evaluate. \
limits: array of plan based limits.  If an account is on the "higher" plan, it will get 300 requests per minute.  Else, the default fallback condition will be used, or 60 requests per minute

General principles:
1. The engine will try to find the best node match based on the incoming request URL and verb
2. The engine will then find the best plan fit based on the professed plan
3. The engine will then employ the amalgamation key (seeded from pretend upper layers of authentication etc) and construct a redis key that uniquely identifies this customer on this node.
4. The engine will use redis to transact with the rate limit ledger
5. If limit thresholds have been exceeded it limits, otherwise it lets traffic through.

