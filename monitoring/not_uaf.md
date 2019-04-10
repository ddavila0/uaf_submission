# Monitoring
Enything related to the monitoring of the UAF infrastructure should be put in here.

## Suggested designs for the job and pilot monitoring

### Using the job wrapper
**Description:** Every job that runs inside a pilot is wrapped by a script, we could use that script to push information about the job every time is started/finished/
The pilot information could be submitted at pilot startup and before it goes away.

**Pros:**
* No pressure on the schedds
* No need to store the pilot's ClassAd in the job's ClassAd

**Cons:** 
* Multiple connections ( 1 per job execution)
* Pilot needs to store password/certificate to push data 
 
### Querying the condor history
**Description:** Querying the history of condor in each of the schedds one could get information about the "history" of the job

**Pros:**
* One single point of data source, no need to share password/certificate to push data

**Cons:** 
* Pressure on the schedds due to enlarged job classAd and having to answer queries
 
