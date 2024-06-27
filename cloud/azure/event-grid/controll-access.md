# Controll access

Azure Event Grid allows you to control the level of access given to different users to do various management operations such as list event subscriptions, create new ones, and generate keys. Event Grid uses Azure role-based access control (Azure RBAC).

### Built-in roles <a href="#built-in-roles" id="built-in-roles"></a>

Event Grid provides the following built-in roles:

<table><thead><tr><th width="322">Role</th><th>Description</th></tr></thead><tbody><tr><td>Event Grid Subscription Reader</td><td>Lets you read Event Grid event subscriptions.</td></tr><tr><td>Event Grid Subscription Contributor</td><td>Lets you manage Event Grid event subscription operations.</td></tr><tr><td>Event Grid Contributor</td><td>Lets you create and manage Event Grid resources.</td></tr><tr><td>Event Grid Data Sender</td><td>Lets you send events to Event Grid topics.</td></tr></tbody></table>
