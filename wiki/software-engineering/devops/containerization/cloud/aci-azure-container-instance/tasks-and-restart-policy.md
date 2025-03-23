The ease and speed of deploying containers in ACI provides a good platform for executing run-once tasks like tests, image rendering and more.

With a configurable restart policy, you can specify that your containers are stopped when their processes have completed. Because container instances are billed by the second, you're charged only for the compute resources used while the container executing your task is running.

## Restart Policy

| Restart policy | Description                                                                                                                                                                                |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Always`       | Containers in the container group are always restarted. This is the **default** setting applied when no restart policy is specified at container creation.                                 |
| `Never`        | Containers in the container group are never restarted. The containers run at most once.                                                                                                    |
| `OnFailure`    | Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code). The containers are run at least once. |
