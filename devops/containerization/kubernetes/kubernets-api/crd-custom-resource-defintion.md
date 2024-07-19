# CRD - Custom Resource Defintion

Kubernetes has default resources like Deployments, ReplicaSets etc. which all are create by the Controllers.

Kubernetes allows to create custom Resources and Controllers to extend the API and create custom objects and controllers. Example:

* Creating a FlightTicket Resource for booking fligh tickets
* Each time an object of kind FlightTicket is created, the controller should book a flight with the specified properties
* If the object is deleted, the booking should be cancled

## Custom Resource

<figure><img src="../../../../.gitbook/assets/IMG_6165 2.JPG" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/IMG_6166.JPG" alt=""><figcaption></figcaption></figure>

## Custom Controllers

For each custom resource, a custom controller needs to be created.
