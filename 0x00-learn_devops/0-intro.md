# DevOps

* a methodology that allows engineering teams to build products while continuously getting user feedback
* traditional programming methodology was like a ford factory; in goes the code etc and out comes the product
* devops is like an infinite loop; plan -> code -> build -> test -> release -> deploy -> operate -> monitor -> plan etc

#### devops engineering
* practical use of devops w/i software engineering teams; that is, being able to build, test, release and monitor apps
* three pillars:
    * pull request automation -> devs share code changes using get tools (GH, GL, BB)
        * a set of code changes in git tools is called a _pull request_(PR)
        * code changes go to the main codebase if PR is approved
    * deployment automation -> the deployment process is automated
        > "can you make a build in one step?" - stack overflow's founder (2000)
        * get a feature to a set of users (test audience) before roliing it out to the masses
        * start new versions of services w/o causing downtime
        * roll back to prior a version in case something goes wrong
    * app performance management -> app performance is measured and monitored
        * metrics: numeric measurements of production process(es)
        * logging: text descriptions of occurrences during processing
        * monitoring: converting metrics and logs into health metrics
        * alerting: notify a dev if previous stage detects a problem 
* 

#### what can a devops engineer automate?
* continuous integration (CI)
* per-change ephemeral environments
* security scans
* notifications to reviewers

#### ideal devops world
* PRs are made, reviewed and merged in 24h
* have the right tools in place to facilitate deployment w/o having too much custom code