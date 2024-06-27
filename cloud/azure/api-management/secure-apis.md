# Secure APIs

## Secure APIs by subscriptions

It's easy and common to secure access to those APIs by using subscription keys. Developers who need to consume the published APIs must include a valid subscription key in HTTP requests when they make calls to those APIs. Otherwise, the calls are rejected immediately by the API Management gateway.

### Subscriptions

A subscription key is a unique auto-generated key that can be passed through in the headers of the client request or as a query string parameter. The key is directly related to a subscription, which can be scoped to different areas. Subscriptions give you granular control over permissions and policies.

Subscriptions can be managed in API management under the menu **Subscriptions**.

The three main subscription scopes are:

| Scope      | Details                                                                                                                                                                                                     |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| All APIs   | Applies to every API accessible from the gateway                                                                                                                                                            |
| Single API | This scope applies to a single imported API and all of its endpoints                                                                                                                                        |
| Product    | A product is a collection of one or more APIs that you configure in API Management. You can assign APIs to more than one product. Products can have different access rules, usage quotas, and terms of use. |

### Call API with subscription key

Either use header or query string. The default header name is **Ocp-Apim-Subscription-Key**, and the default query string is **subscription-key**.

#### Header

```
curl --header "Ocp-Apim-Subscription-Key: <key string>" https://<apim gateway>.azure-api.net/api/path
```

#### Query string

```
curl https://<apim gateway>.azure-api.net/api/path?subscription-key=<key string>
```
