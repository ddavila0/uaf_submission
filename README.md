# Introduction

The UAF Infrastructure is an HTCondor pool that puts together grid resources from several US institutes making use of GlideinWMS for dynamic provisioning on demand.

## Components

The main components of the infrastructure are the following: **Schedds**, **Collector** & **Negotiator**, **Startd**, **Frontend** and **Factory**.

### 1. Schedds.
Many people refere to this component as the "queue" because this is the place where jobs are queued. A user will normally login into one of these machines to submit jobs from there usign the *condor_submit* command.

Currently there are 12 schedds listed on the Frontend configuration, but fewer show to be actually connected to the Collector.

Schedds listed in the Frontend configuration file:
* **cmssubmit-r1.t2.ucsd.edu**
* **uafino.physics.ucsb.edu**
* **uaf-1.t2.ucsd.edu**
* **uaf-2.t2.ucsd.edu**
* **uaf-3.t2.ucsd.edu**
* **uaf-4.t2.ucsd.edu**
* **uaf-5.t2.ucsd.edu**
* **uaf-6.t2.ucsd.edu**
* **uaf-7.t2.ucsd.edu**
* **uaf-8.t2.ucsd.edu**
* **uaf-9.t2.ucsd.edu**
* **uaf-10.t2.ucsd.edu**

Schedds showing in the Collector when executing the folowing command:<br>
``` condor_status -pool glidein-collector.t2.ucsd.edu -schedd ```

* **uaf-1.t2.ucsd.edu**       
* **uaf-3.t2.ucsd.edu**       
* **uaf-4.t2.ucsd.edu**       
* **uaf-7.t2.ucsd.edu**       
* **uaf-8.t2.ucsd.edu**       
* **uaf-10.t2.ucsd.edu      
* **uafino.physics.ucsb.edu** 


### 2. Collector & Negotiator
These are two separate components that normally live in the same host. The Collector could be seen as the central piece of the pool, it holds the information about the status of the pool. 
The negotiator is in charge of matching jobs to resources in a process known as the *Matchmaking*

Currently these components live in the following host:<br>
**glidein-collector.t2.ucsd.edu**


###3. Frontend
The main function of this component is to calculate the resources required in the pool and to request pilots to the Factory to fullfil that demand. It calculates this by querying the Schedds on the pool to know how many idle jobs there are and the site(s) where they are willing to run on, and then querying the Collector to know the amount of resources currently available in the pool and the site(s) they belong to.

Currently this component lives in the following host:<br>
**glidein-frontend.t2.t2.ucsd.edu**

###4. Factory.
The Factory keeps a catalog of all the grid resources and the main attributes of their worker nodes like the number of cores, memory, disk, etc. It also knows the entry point to each of the grid sites also known as Gatekeeper. The Factory is the one allowed to send remote jobs or "pilots" to the sites to requesting a resource allocation. These pilots will be queued or will "land" in a site's batch system and later on when they get to run they will startup a condor Startd that will connect back to the pool's Collector mentioned above.

###5. Startds.
This is the HTCondor component that represents the resources in the pool. On "grid land" the Startds are started by and within the pilots when they get to run in a given site. These startds will represent an allocation of resources in a given site and these resources are available through the UAF infrastructure.

A pilot is nothing else that a generic job that will execute a Startd instance that is properly configured to connect to a ceratin Collector, once connected, the startd can be assigend with actual user's jobs queued on the Schedds mentioned above. 
 
Although most of the Startds within the pool are provisioned by GlideinWMS, there are some of them running on the Schedds nodes, in this case the same node that runs the Schedd component also runs a Startd, so it is fair to say that not all the provisioning is done by GlideinWMS. These Startds are mostly used to test the users's submissions before sending real jobs.


