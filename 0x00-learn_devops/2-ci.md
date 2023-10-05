# continuous integration (CI)

* in CI, a dev pushes many, small changes to a central git repo every day
* said changes are verified by an automated process (software, probably)
* said software runs cmprehensive tests to ensure no major issues are seen by customers

#### why CI?
* CI is the first step to devops automation
* helps w. code collaboration
* improves dev speed w/o breaking existing code
* reduces customer churn because broken code is not published
* improves user satisfaction for the reason stated above

#### how to integrate CI into your workflow
* `main` (formerly `master`) branch will be published to users (soon)
* all devs work on individual `feature` branches
    * every feature may have its own branch or
    * a dev may have his(er) own branch where s/he implements all his(er) features