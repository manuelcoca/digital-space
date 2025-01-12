# Jobs & CronJobs

## Jobs

A Job is used to run a set of Pods (like ReplicaSet) to perform a given task to completion.&#x20;

The difference to ReplicaSet is, that a Job will not create new Pods, if they are terminated. By default, a Pod or ReplicaSet tries to restart a Pod as soon as it gets terminated which is why it can't be used for Jobs.&#x20;

Finished Job Pods will have the Status **`Completed`**.

Example use-cases of Jobs:

* Generating Reports
* Sending Mails
* Processing Data
* etc.

### Running multiple Pods to finish a Job

With the `completions` property (see below) a set of Pods can be defined to run the Job. By default, the jobs pods are created one after the other, which means the second Pod will start if the first finish. If one Job Pod fails, it will create a new Job Pod until the specifed `completions` amount is completed.

To run the Jobs in parallel, define the `parallelism` property (see below).

### YAML Configuration

Same as on ReplicaSet it has a template for defining the Pods

```
apiVersion: batch/v1
kind: Job
metadata:
  name: math-add-job
spec:
  template:
    spec:
      completions: 3    # Run 3 Pods to finish Jobs
      parallelism: 3    # Creates 3 Pods in parallel instead of sequently
      containers:
      - name: math-add
        image: ubuntu
        command: ["expr",  "3", "+", "2"]
      restartPolicy: Never
  backoffLimit: 4
```

## CronJobs

CronJobs are scheduled Jobs.

```
apiVersion: batch/v1
kind: Job
metadata:
  name: math-add-job
spec:                        # CronJob spec.
  schedule: "*/1 * * * *"
  jobTemplate:               # Job template same as above
    spec:
      completions: 3         # Run 3 Pods to finish Jobs
      parallelism: 3         # Creates 3 Pods in parallel instead of sequently
      template:              # Pod template and specs
        spec:          
          containers:
          - name: math-add
            image: ubuntu
            command: ["expr",  "3", "+", "2"]
          restartPolicy: Never
```

## Commands

<table data-header-hidden><thead><tr><th width="224"></th><th></th></tr></thead><tbody><tr><td>Create Job</td><td><strong><code>$ kubectl create -f &#x3C;job.yml></code></strong></td></tr><tr><td>Get Jobs</td><td><strong><code>$ kubectl get jobs</code></strong></td></tr><tr><td>Delete ReplicaSet and all underlying PODs</td><td><strong><code>$ kubectl delete job &#x3C;job-name></code></strong></td></tr><tr><td>Describe Job</td><td><strong><code>$ kubectl describe job &#x3C;job-name></code></strong></td></tr></tbody></table>
