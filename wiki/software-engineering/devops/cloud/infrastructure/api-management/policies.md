In Azure API Management, policies allow the publisher to change the behavior of the API through configuration. Policies are a collection of Statements that are executed sequentially on the request or response of an API.

Policies are applied inside the gateway that sits between the API consumer and the managed API. The gateway receives all requests and usually forwards them unaltered to the underlying API. However a policy can apply changes to both the inbound request and outbound response

## Policy configuration

Policies are a collection of Statements that are executed on inbound or outbound requests.

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies>
```

## Policy expressions

Policy expressions can be used as attribute values or text values in any of the API Management policies:

```xml
<policies>
    <inbound>
        <base />
        <set-header name="x-request-context-data" exists-action="override">
            <value>@(context.User.Id)</value>
            <value>@(context.Deployment.Region)</value>
      </set-header>
    </inbound>
</policies>
```

## Advanced Policy configuration

Advanced policies:

-   **Control flow** - Conditionally applies policy statements based on the results of the evaluation of Boolean expressions.
-   **Forward request** - Forwards the request to the backend service.
-   **Limit concurrency** - Prevents enclosed policies from executing by more than the specified number of requests at a time.
-   **Log to Event Hub** - Sends messages in the specified format to an Event Hub defined by a Logger entity.
-   **Mock response** - Aborts pipeline execution and returns a mocked response directly to the caller.
-   **Retry** - Retries execution of the enclosed policy statements, if and until the condition is met. Execution will repeat at the specified time intervals and up to the specified retry count.

### Control flow

```xml
<choose>
    <when condition="Boolean expression | Boolean constant">
        <!— one or more policy statements to be applied if the above condition is true  -->
    </when>
    <when condition="Boolean expression | Boolean constant">
        <!— one or more policy statements to be applied if the above condition is true  -->
    </when>
    <otherwise>
        <!— one or more policy statements to be applied if none of the above conditions are true  -->
</otherwise>
</choose>
```

### Forward request

```xml
<forward-request timeout="time in seconds" follow-redirects="true | false"/>
```

### Limit concurrency

```xml
<limit-concurrency key="expression" max-count="number">
        <!— nested policy statements -->
</limit-concurrency>
```

### Log to Event Hub

```xml
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">
  Expression returning a string to be logged
</log-to-eventhub>
```

### Mock response

```xml
<mock-response status-code="code" content-type="media type"/>
```

### Retry

```xml
<retry
    condition="boolean expression or literal"
    count="number of retry attempts"
    interval="retry interval in seconds"
    max-interval="maximum retry interval in seconds"
    delta="retry interval delta in seconds"
    first-fast-retry="boolean expression or literal">
        <!-- One or more child policies. No restrictions -->
</retry>
```
