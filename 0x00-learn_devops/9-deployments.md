# rolling deployments

* *rolling deployments* (think continuous deployment) ia astrategy to deploy a new version of an app w/o causing down time

#### how they work
* create a single instance of the new version
* make sure the new instance is working properly
* send traffic to the new version
* shut a single instance the old version
* repeat until all instances have been upgraded

#### where rolling deployments shine
* well-supported
    * relatively straight-forward, in most cases, to implement
    * natively supported in some orchestrators eg. kubernetes, elastic beanstalk
* no huge bursts
    * do not require a lot of computing resources to implement
* easily reverted
    * roll-backs are easy to implement

#### where rolling deployments do not shine
* speed
    * slow to implement; takes O(m*n) time to deploy m instances assuming each deployment takes n time, no error adjustments
    * one can reduce the time by increasing the *burst limit* (the #instances at a time being created and/or shut)
* compatibility with APIs
    * example: say you use rolling depoyment strategy to update an API from v1 to v2. at one point in time, the front-end will be served, to varying proportions, by both

    > in other words: you start with 100% of v1 and end with 100% of v2. in between, you have various combinations of both versions such that a request made to v1 will be
    handled and responded to by v2

    <img src="/errors.jpeg"/>

    * make API backward-compatible to solve this problem
 