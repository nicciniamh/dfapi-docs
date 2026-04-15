# jobs

The jobs module lists processes, gets process information or sends signals to 
running processes. Jobs are submitted with JSON in the data property of the request payload. The status and cancellation of jobs are done using the job id assigned when a job is started. 


## Job Control Objects

Job control objects are JSON objects with specific parameters to define a job. 
If a required property is missing the job will fail before it starts. 

<details>
<summary><b>Example Job Control Object</b></summary>

```javascript
{
   "data": {
      "_id": "",					// Set by the module 
      "args": ["upgrade","-y"],		// job parameters 
      "class": "apt",				// job class - in this case apt
      "state": "",					// set by the module 
      "target": "nodea",			// target node of job
      "client": ""
   }
}
```

</details>

The job control object is standardized into a job queue record using an internal schema ensuring specific types and proper and default values. Thew job queue record is added to the job queue in MongoDB where the picup tasks of the runners will get and run their pending objects. This queue has a default TTL of one hour (3600 seconds) leaving a large enough window for the job to run, and report results.

## Endpoint Format

With the exception of the list endpoint, each endpoint is specified as
`jobs/{jobid}/function` or `jobs/submit`.


* [submit](#submit) - Submit a long running job.
* [status](#status) - Get the status of a job
* [cancel](#cancel) - Cancel a job

### submit

* Role required: lrj
* Usage: `jobs/submit`

### status

* Role required: ljr
* Usage: `jobs/{jobid}/status`

Returns details current status of a submitted or completed job including return codes and job output.

### cancel

* Role required: lrj
* Usage: `jobs/{jobid}/cancel`

Cancels the job identified by jobid.
