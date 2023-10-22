# autoscaling

autoscaling automates horizontal scaling so that the number of workers is proportional to the load on the system

#### providers of such services (Sun, 22 Oct, 2023)
* amazon AWS EC2 spot instances
* CNCF kubernetes horizontal pod autoscaling

#### autoscaling vs serverless
* autoscaling is, usually, discussed on *c.* 1 hour chunks of work; if one took the concept of autoscaling to its limit (towards zero), the result is serverless
* serverless defines resources that are started quickly and whose chunks-of-work measure is *c.* 100ms
* serverless is, primarily, used for services that are somewhat fast to start and are stateless
    * example: web server or notification service
* autoscaling is, primarily, used for services that are somewhat slow to start and have state
    * example: a CI run
* serverless resources, unlike containers, operate by trigger; they turn on and/or off in response to a condition/constraint etc