# Container Orchestration

### Problem

* Docker run runs one instance on one docker host
* When number of users increase that one instance is no longer able to handle the load
* In this case you must deploy additional instances buy running docker run multiple times
* You need to track the load and performance and deploy new instances by yourself
* If containers or the whole docker host crashes we need to fix the problems by ourself
* In large applications where thousands of containers are deployed it's almost impossible to manage all these containers

### Solution

* Orchestration solution consists of multiple docker hosts
* In that way even if one fails the application is still accessible
* Some Orchestration solutions can help you automatically scale up the number of instances when users increase
* Orchestration provides advanced networking between the instances
* There are multiple solutions on the market

{% content-ref url="../kubernetes/" %}
[kubernetes](../kubernetes/)
{% endcontent-ref %}
