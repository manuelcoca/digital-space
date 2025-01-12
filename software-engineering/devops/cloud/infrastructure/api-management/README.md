# API Management

API Management helps organizations to publish and protect APIs to external, partner, and internal developers. Each API consists of one or more operations, and each API can be added to one or more products. To use an API, developers subscribe to a product that contains that API, and then they can call the API's operation.

The API Management Service also acts like an abstraction and brings following features:

* Provide API documentation
* Allow users to register and approve them
* Throttle, rate limit and quota your APIs
* Monitor health of your APIs and quickly identify errors
  * Also identify who is calling the API's
* Bring modern formats like JSON and REST to existing APIs
* Connect to APIs hosted anywhere on the Internet or on-premises and publish globally
* Gain analytic insights on how your APIs are being used

## API Management components <a href="#api-management-components" id="api-management-components"></a>

Azure API Management is made up of an _API gateway_, a _management plane_, and a _developer portal_. These components are Azure-hosted and fully managed by default. API Management is available in various [tiers](https://learn.microsoft.com/en-us/azure/api-management/api-management-features) differing in capacity and features.

* The **API gateway** is the endpoint that:
  * Accepts API calls and routes them to appropriate backends
  * Verifies API keys and other credentials presented with requests
  * Enforces usage quotas and rate limits
  * Transforms requests and responses specified in policy statements
  * Caches responses to improve response latency and minimize the load on backend services
  * Emits logs, metrics, and traces for monitoring, reporting, and troubleshooting
* The **management plane** is the administrative interface where you set up your API program. Use it to:
  * Provision and configure API Management service settings
  * Define or import API schema
  * Package APIs into products
  * Set up policies like quotas or transformations on the APIs
  * Get insights from analytics
  * Manage users
* The **Developer portal** is an automatically generated, fully customizable website with the documentation of your APIs. Using the developer portal, developers can:
  * Read API documentation
  * Call an API via the interactive console
  * Create an account and subscribe to get API keys
  * Access analytics on their own usage
  * Download API definitions
  * Manage API keys

## Products

Products are how APIs are surfaced to developers. Products in API Management have one or more APIs, and are configured with a title, description, and terms of use. Products can be **Open** or **Protected**. Protected products must be subscribed to before they can be used, while open products can be used without a subscription. Subscription approval is configured at the product level and can either require administrator approval, or be autoapproved.

## Create an API Management Service

{% tabs %}
{% tab title="Basics" %}
1. **`Pricing tier`**
   * Consumption plan allows to only pay as you go. This plan is recommended for smaller applications (less then a few million calls per month)
   * Isolated plan is the most expensive and Azure will guarantee to run on isolated hardware
{% endtab %}

{% tab title="Virtual Network" %}
Same as on other Services. Allow access only from Virtual Networks.
{% endtab %}
{% endtabs %}

## Menu/Configuration

<details>

<summary>APIs</summary>

Connect an external API or In-Cloud Services like Azure Functions to your API-Management Service to expose them. Azure also allows to Import OpenAPI specification to create an API within the API Management Service.

After an API is added, Azure gives and visual tool to create Inbound and Outbound policies (like IP filters, limit call rate, set http headers etc.)  to controll the communication.&#x20;

</details>

<details>

<summary>Products</summary>

Create a new product to give people (e.g public, or other business) access to your API.

1. **`Requires subscription`**
   * An Azure subscription is required to access API
2. **`Requires approval`**
   * Access will be approved by an Administrator
3. **`Subscription count limit`**
   * Limit access to set amount (e.g only 1000 users)

</details>

<details>

<summary>Subscriptions</summary>

Manage subscriptions. Read more about [here](secure-apis.md#subscriptions).

</details>
