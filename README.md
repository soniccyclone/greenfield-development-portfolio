# Greenfield Development Portfolio
Purpose of this repository is to showcase the standards and strategies I would have in place when tackling a broad range of greenfield projects. (e.g. web scale services)

## Tech Stack
Generally the tech stack will be:
- Python
- Go
- Make (make targets help maintain living source of truth documentation on running the system)
- Vitess/Citus/Yuggabyte (need to experiment with these and choose one, citus seems like it would have the longest lasting payoff so long as it being a plugin really does make it more flexible than the other DB engine "compatible" options)
- Apache Drill/Spark/Flink (need to choose between which of these MPP libraries I would want to use in most cases. Mostly would like to be able to abstract away all MPP work into SQL queries so that other parts of the system are simply orchestrating the MPP work)
- k8s (duh)
- S3
- Typescript React? (I need to have a more solid opinion on front-end technologies. My preference is for vanilla web technologies, maybe even HTMX, but this is a professional plan that needs to ensure hiring is easy as well)
- Kafka (event sourcing fits a whole lot of problems and learning how to setup kafka properly once on k8s using strimzi will pay dividends. And all major cloud providers provide a managed kafka instance. This may also remove the need for streaming based MPP like Flink and sort of Spark and would mean just having the ability to run MPP queries one time is all we need in which case Drill would be simplest)
### Dev environment
Some specifics on how we ensure the ENTIRE tech stack can be run on a local developer machine:
- Everything must be developed inside of docker containers (lots of people still develop on their machine and just build into containers)
- Minikube (need to be running on a k8s environment locally)
- Skaffold.dev (this gives the developer a really nice hot update feedback loop)
- Debugging k8s pods (so is this just single replicas for all local manifests? I am fine letting the quality gate integration tests actually do a multiple replica test. And you should really only need that for a quick load test thing)
- Auto running test cluster (just have a test cluster manifest that is a quality gate on PRs and have a lighter weight version of that which is constantly running in the background on the developer's machine. So like, single replica and probably not running any load balancer type integration tests.)
- Ceph for local object storage (minio license is weird and scary)

## Code Standards
Everything Uncle Bob says, even the stuff I don't agree with since I'm not gonna force everyone around me to learn lisp and functional programming.

But really the most important guiding principle are the SOLID principles. And a requirement for test automation and release automation to be a part of all MVPs.

All projects must have a readme.md explaining themselves and an architecture.md doc that explains how to navigate the repository and where you should put most kinds of files when you need to create one.