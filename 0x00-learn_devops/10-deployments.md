# blue-green deployment

*blue-green* deployment is a strategy of deploying new apps. one creates a new instance of an app and directs traffic to it. an old instance is, then, be retired

#### how  it works
* there are two clusters of the app: a blue one and a green one
    * the blue one is the old version/instance; green is new
    * each is independent of each over (does not rely on the other)
    * each has the same access to shared resources e.g. a db (the db is neither blue nor green)
    * users (aka production load) are sent to a green cluster as soon as it is deployed and is ascertained to work as expected
    * one blue is retired for every green that, well, gets the *green light*

#### where b-g deployments shine
* easy to understand -> set up two identical prod envs; each takes turns being blue or green
* powerful -> longer-running tasks eg downloads carry on running in the *blue* even after traffic has been routed to green
* extendable to workflows -> is the basis of a number of deployment sub-workflows
* works well w. teams that deploy a few times a day

#### where b-g deployments do not shine
* difficult to make hot fixes -> older cluster may be running longer-running tasks and, as such, unavailable.
    * example: users use v1 of an app. v2 is deployed and users are directed to it. say, for one reason or another, v2 works atypically. one would have to deploy v3 to the env that v1 runs in, however, v1 is running longer-running tasks there. what now? 
* resource allocation is not convenient -> load trasfer bweteen clusters is not straight-forward. cannot transfer the prod load all at once; there may not be sufficient resources to handle the surge. also, how do you tell it is not a DDoS attack on the new cluster? 
* clusters can affect each other -> say a cluster accesses a shard\[ed\] service e.g. a call to a db. said call may be returned to an unintended cluster
* not suitable for CI scenarios where there are many services being deployed many times a day

#### extensions
* rainbow deployments -> have an arbitrary #prod envs (not just blue and green). a cluster is retired (shut off) when all longer-running jobs are processed
* acceptance tests -> test a new instance of an app against the prod db in the env that will, soon, be the prod env. AQ and product stakeholders choose to or not to *accept* the dev team's latest release
* canary deployments -> have a typical b-g set-up. route, say, 5% of your users, at random, to the new cluster and check whether or not said users give negative feedback