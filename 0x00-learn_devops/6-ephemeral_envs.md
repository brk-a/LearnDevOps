# ephemeral environments

* temporary deployments that have self-contained versions of the app (generally, every feature branch)
* are created, automatically, on every change
* eliminates the need for a developer environment on the parts of the reviewer(s), the rest of the team and test users
* examples: vercel, layerCI, heroku, netlify, fl0 etc
* are a cross between dev envs and staging envs; staging is replaced, entirely, by the ephemeral env (continouous staging)
* lasts as long as a PR does

#### benefits
* accelerates SDLC
* allows devs to share changes (and communicate more easily) with designers, managers and other stakeholders

#### how databases work in ephemeral envs
* pre-populated data -> representative, anonymised data. personal indentifiable info must be scrubbed from db in order to pass security audit
* undo-able -> db is easy to return to original state if/when data is deleted or modified
* migrated -> db uses schema that is, currently, in prod. also, proposed migrations are run against it
    * non-performant db migrations are a common class of problems uncovered by ephemeral envs

#### when to start an ephemeral env
* tie the life cycle of an ephemeral env to that of a PR or merge rquest (MR)
* a chat-ops bot that allows the creation of a new env for a specific branch w. a small time-out
* ephemeral env for every commit, hibernate them when they are provisioned and wake then up on demand

#### DIY ephemeral envs
* budget a month per month of engineering time of a feature/service to set up and find/hire/orient a decicated env manager
* find a cloud storage provider
* create a slackbot that responds to `/env-create-for-branchName` with a link 