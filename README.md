

dockerera is Instand out of the box IaaS and PaaS using docker, couchbase, cloudera:

![dockerera logo](https://github.com/dockerera/dockerera/raw/master/dockerera-logo.png)(TM)

![docker](https://docs.docker.com/img/nav/docker-logo-loggedout.png)
![couchbase](https://www.nimic.net/wp-content/uploads/2014/04/couchbase_logo.png)
![cloudera](http://inside-bigdata.com/wp-content/uploads/2013/10/Cloudera-Logo.png)

Distributed IaaS Like Amazon or Azur
Ultra Performant much better then Any Other Cloud Plattform 
using 
to View Logs and realy big Processes in Realtime ! dockerera supplys cloudera deployment in minuts!
so even if you only whant to run cloudera or couchbase only in production you should watch at dockerera!

At Present Porting Longshore man Concepts into dockerera
Integrating HIPACHE Routers
Integrating Apache Routers

dockerera automates application deployment using Docker. Just create a Docker repository (or use a service), configure the cluster using AWS or DirektSPEED Hosting (or whatever you like) and deploy applications using a Heroku-like CLI tool or Our Web Interface WebUI.

[Main GitHub project page](https://github.com/dockerera)

## Why make this?

We created dockerera because we love using Docker but were frustrated with the lack of production-ready deployment options that were available at the time. We looked closely at Deis, Flynn, Dokku and others, but they either did not meet our requirements or were explicitly marked as not ready for production. 

This Project got Born out of Many Little IDEAS that i got as i Planed to Start my New Internet Service
Now i Ended up and Did my Own IaaS With High Performance Ultra High I/O Blasting Perfromance.
And i Offer that Know How to Anyone Who Needs it.

dockerera mergs the two Old Projects Container Harbor and Varius others into DockerEra Distribution

Goals:
Building a Docker Powered Cloud Plattfrom where you can run Hadoop Tasks in and Store Infinity Data For Processing in Realtime.

Compatiblity to:
Project Atomic DockerHosts
Project CoreOS DockerHosts
Nativ Docker Running Hosts
Linux Windows MAC

Using Ubuntu + MAAS + NODE.JS + cloudera + Couchbase + Packer.io + Docker.io and all the Dependencys they got and build on.


## Who made it?

libcontainer is sponsored/developed by MichaelCrosby
Docker is sponsored/developed by Docker Inc
Vagrant is sponsored/developed by MitchellH
dockera is sponsored/developed by DirektSPEED Europe

## How does it work?

dockerera has 3 core components: a controller, one or more Routers and cloudera stack. It also uses a Docker registry and Couchbase as its configuration database and Distributed file Storage.

### Controller

The DockerEra controller is a service which orchestrates the deployment of Docker applications across a cluster and controls how traffic is routed to individual application instances. It is a REST API that can be used via HTTTP with the CLI tool or else What. Launching a new version of an application is as simple as `dockerera --app my.app.com deploy docker.repo.com/image:tag`. Your application will be deployed to 2 or more nodes (depending on the size of your cluster and its available resources). Versioning and rollbacks can be achieved using image tags.

[NODEJS Controller Repository](https://github.com/dockerera/controller)

### Routers

Routers dynamically direct incoming web traffic to the correct application instances we are using Hipache from Docker Inc. Multiple routers can be utilized to distribute traffic and eliminate single points of failure.

[NODEJS Router Repository](https://github.com/dockerera/router)

### CLI

The command line tool is an interface to the Longshoreman controller service. It allows users to describe the state of the application cluster, deploy new instances of applications (with zero-downtime), add and remove hosts, add and remove application environmental variables and more. See the link below for full documentation.

[NODEJS CLI Repository](https://github.com/dockerera/cli)

### Other Components

#### Registry

dockerera uses a Docker registry to coordinate application versioning and deployment. Docker registries are outside of the scope of this project, so if you're unfamiliar with them please [read more here](https://github.com/dotcloud/docker-registry). 
Setting up an S3-backed private registry is fairly simple. Just follow [these instructions](https://github.com/dotcloud/docker-registry).

#### Configuration Store

We are currently using Couchbase to store and distribute the cluster's state. DockerEra uses PubSub to notify Routers of updates to the internal application routing table. We're looking into support for CouchBase as a single point of failure exists if the Redis instance is not redundant and we use Couchbase for Every thing there is no question :D.

![Diagram](http://i.imgur.com/I0POpX4.png)

## Quick Start

This guide will walk you through creating a Longshoreman powered cluster (we're using EC2 running Ubuntu in this example).

To create an application cluster using Longshoreman, you'll need at least 2 server nodes. However we recommend using 5 for enhanced robustness. Here's how they're broken down: 1 router, 1 controller, 2 application nodes and a Redis box (using a Redis hosting provider will work well too). In the 2 node set up the router, controller and Redis db can live on a single server (but that's not recommended). Actually, the whole thing can run on a single server if you're just taking a test drive, but I digress.

### 1. Launch a controller

1. Launch an EC2 instance and log into the box.
2. Install Docker
3. Start the controller with `sudo docker -d -p 80:80 run -e REDIS_HOST=$REDIS_HOST_IP -e REDIS_PORT=6379 longshoreman/controller`

### 2. Launch the router(s)

1. Launch an EC2 instance and log into the box.
1. Install Docker
1. Start the router with `sudo docker run -e -p 80:80 -d REDIS_HOST=$REDIS_HOST_IP -e REDIS_PORT=6379 longshoreman/router`
1. Configure your load balancer (ELB, etc.) to direct traffic to the router instance(s).

### 3. Deploy container nodes

1. Launch 1 or more EC2 instances.
1. Install Docker
1. Edit the Docker config with `vi /etc/default/docker`
1. Set `DOCKER_OPTS="-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"` to enable the Docker Remote API
1. Restart Docker with `sudo service docker.io restart`
1. Create an AMI if you'd like to speed up this step next time you launch a container node.

### 4. Configure and deploy applications using the CLI

1. Run `longshoreman init` to configure your credentials. Enter the Longshoreman controller domain and your token. The token is auto-generated and is stored in Redis (`GET token`).
1. `longshoreman hosts:add <container-node-ip>` to make Longshoreman aware of your nodes.
1. `longshoreman apps:add my.app.domain` to add a new service or application to your cluster.
1. `longshoreman --app my.app.domain envs:set FOO=bar` to configure your application's runtime settings.
1. `longshoreman --app my.app.domain deploy my.docker.reg/repo:tag` to deploy the first version of your application.
1. Point your domain to your load balancer's CNAME and Bob's your uncle.

Check out [the CLI repository](https://github.com/longshoreman/cli) for full documentation.


========================


## Workflow
# Enter Accounts of Cloud Providers or Baremetals via Conical MAAS
# A Master Controller Installation can Handle the Deployment of the Whole Infrastructure and Scale it Complet
# A Slave is a DockerHost running ubuntu core. 
  - Containers can Choose if Running on HDFS or CouchBase or Local Mounts
  - All Configs and Infos as also data get shared via Couchbase - Long Time Storage via HDFS or Physical Disks
  - Using CloudEra Analyze of Running Processes and Logs.
  - 
# First Test Results The DockerEra Cloud Structure is as Performant as Amazon Cloud or Azure The Diffrents are Realy the Same if you use Windows or Linux as Couchbase Host

DockerEra for Windows is Planed but not focused.


# Ready Services are:
Route53 Clone
S3 Clone
EC2 Clone
More Soon




# TODO adding example of this use case
http://www.ebaytechblog.com/2014/05/12/delivering-ebays-ci-solution-with-apache-mesos-part-ii/#.VBkHBt_RekA
in dockerera in less then 30 min ! Deploy 1000 EC2 Instances Running Jenkins and Process Jobs! powered by Direkt SPEED Europe and dockerera !!!!
