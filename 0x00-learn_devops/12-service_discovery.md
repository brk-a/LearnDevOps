# service discovery

#### preamble
* say there is a server at 10.1.1.1:6543 and a db at 10.1.1.2:8080 and that one wants to deploy the server and db
* example two: MERN app
    * say there are three services: mongoDB, node/express back-end and react front-end that must talk to each other and the user of the app
        * the browser (end user/react) must learn that `example.com` is at IP, say, 10.11.12.13
        * the browser must learn that `api.example.com` is at IP, say, 100.0.101.1
        * the back-end (node) must learn that the db (mongoDB) is at IP, say, 144.288.432.576
    * trivial solution: configure the set-up to static IPs. back and front-ends are at static IPs and are given hostnames w/i DNS. back end is configured to connect to the db at a specific port. in other words *manual configuration*
    * there would be a DNS mapping for the front-end on a service, say, cloudflare
    * the back-end would refer to an `env` variable on start-up viz:

        ```javascript
            //backend.js
            mongoose.connect('mongodb://'+process.env['MONGODB_IP']+':27017/myapp');
        ```

        ```bash
            #when starting the back-end
            MONGODB_IP=10.11.12.13 node backend.js
        ```
    
    * works for small projects
* enter service discovery via hash table: store service IPs in a hash table
    * consider the following nginx config

        ```bash
            #nginx.conf.tmpl
            server{
                server_name example.com;
                location / {
                    proxy_pass http//{{getenv "/ips/frontend"}};
                    proxy_set_header Host $host;
                    proxy_set_header X-Real-IP $remote-addr; 
                }
            }

            server {
                server_name api.example.com;
                location / {
                    proxy_pass http//{{getenv "/ips/api"}};
                    proxy_set_header Host $host;
                    proxy_set_header X-Real-IP $remote-addr; 
                }
            }
        ```

    * traffic could be routed to whatever address the new service is on by changing the value in the  `.env` file
    * problems w. this approach
        * complex: introduced three dependencies: nginx, etcd and confd; engineers would have to learn these
        * prone to error(s): easy to forget to update necessary keys. difficult to update keys when running multiple copies of each version of a service
        * requires custom config files: usually a good idea to use default rather than custom configuration
* enter service discovery using DNS: decouple the application logic from the deployment logic
    * DNS is, simply, a map of hostnames to IPs
    * say we have `http://frontend`. DNS may be used to return/resolve the IP adrress of the correct version(s) of the frontend. all we have to do is edit the DNS config file
    * in other words: configure your services to query a server you control for DNS queries and then the `'mongodb://mongo'+process.env.['MONGODB_IP']+':27017/myapp');` part becomes `'mongodb://mongo:27017/myapp'` viz:

        ```javascript
            //within backend.js, before DNS-based discovery:
            mongoose.connect('mongodb://'+process.env['MONGODB_IP']+':27017/myapp')

            //using DNS-based discovery:
            mongoose.connect('mongodb://mongo:27017/myapp')
        ```

    * its not trivial to deploy one's own DNS server; highly likely that you will use CoreDNS or a cloud provider and/or Kubernetes internal solution(s). setu-up will be viz:
        * web browser talks to reverse proxy
        * reverse proxy talks to cloud DNS
        * reverse proxy talks to back and front end
        * back end talks to db
        * back end talks to reverse proxy
        * reverse proxy talks to web browser

#### when to care about service delivery
* you want *zero downtime deployments*
* you want to use other, more complex, deployment strategies
* you have a significant number of microservices
* your are deploying to several environments e.g. dev, staging, ephemeral or prod and it is getting unwieldy

#### idea of zero-downtime deployments
* start a version of the back and front-end
* wait until the new version is up
* direct traffic to new version
* shut off old version of the back and front-end

#### how the MERN app would operate using zero-downtime
* reverse proxy sits between user(s) and app
* back-end (node) and front-end (react) talk to the reverse proxy
* back-end talks to db (mongoDB)
* user(s) talk(s) to app through reverse proxy