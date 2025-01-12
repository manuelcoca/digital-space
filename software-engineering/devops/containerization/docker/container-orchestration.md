# Container Orchestration

### Problem

* `docker run` launches one instance on one Docker host.
* When the number of users increases, that one instance is no longer able to handle the load.
* In this case, you must deploy additional instances by running docker run multiple times.
* You need to track the load and performance and manually deploy new instances.
* If containers or the whole Docker host crashes, we need to address the problems ourselves.
* In large applications where thousands of containers are deployed, it's almost impossible to manage all these containers manually.

### Solution

* An orchestration solution consists of multiple Docker hosts.
* This way, even if one host fails, the application remains accessible.
* Some orchestration solutions can automatically scale up the number of instances when user numbers increase.
* Orchestration provides advanced networking between instances.
* There are multiple orchestration solutions available on the market.

{% content-ref url="../kubernetes/" %}
[kubernetes](../kubernetes/)
{% endcontent-ref %}
