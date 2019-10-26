# Environments

Different environments are just a trick to meet various stages of production life cycle - `development`, `testing`/`staging` and release into `production`. The number of stages can vary from project to project depending on complexity and habits, but the classical is a 3-stage pipeline. The main things here are _isolation_ and _duplication_: all stages must be isolated from each other and the `staging` environment must duplicate a `production` environment. ==You should be ready for unexpected consequences if principles are not satisfied==. It's common practice for teams and studios to roll out `staging` instances with data from the `production` database to test recent updates.

!!! abstract "All stages have their own instances of every project entity"
    - Client build;
    - Backend instance;
    - Developer account;
    - Url;
    - Analytics account;
    - And any other things.

_So you should have 1 separated Goblin Backend instance for every stage with its own cluster and database._

The `development` environment instance is free for onboarding, has minimum power and workloads also don't represent a cluster. The `staging` environment instance is represented as cluster but has minimum workloads so you can pick `S1-h1` pack. So and `production` environment instance will have the maximum workload - you should pick the right pack based on your _DAU_ and _CCU_ needs.