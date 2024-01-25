# Service Bus Module

This module allows the creation of Azure Service Bus Queues with optional monitoring.
For directions on how to import this module, check your options in [Terraform Module Sources](https://developer.hashicorp.com/terraform/language/modules/sources#github).

## Child Modules

| Module Name           | Resource                                                                                                                             | Use                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Requires                                |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| Resource Group        | [azurerm_resource_group](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/resource_group)             | Manages a Resource Group.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | -                                       |
| Service Bus Namespace | [azurerm_servicebus_namespace](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/servicebus_namespace) | Manages a ServiceBus Namespace. Name must be unique across namespace location. Standard location used here is "Germany West Central"/"germanywestcentral".                                                                                                                                                                                                                                                                                                                                                                     | Resource Group                          |
| Service Bus Queue     | [azurerm_servicebus_queue](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/servicebus_queue.html)    | Manages a ServiceBus Queue. Is tied to a Service Bus Namespace.                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Namespace                               |
| Action Group          | [azurerm_monitor_action_group](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_action_group) | Manages an Action Group within Azure Monitor that is notified when alerts fire.                                                                                                                                                                                                                                                                                                                                                                                                                                                | Resource Group                          |
| Metric Alert          | [azurerm_monitor_metric_alert](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_metric_alert) | Manages a Metric Alert within Azure Monitor. Metrics, thresholds, and more are variable. For details, see [documentation](https://learn.microsoft.com/en-us/azure/azure-monitor/reference/supported-metrics/microsoft-servicebus-namespaces-metrics), [main.tf](https://github.com/ServiceBusAssignment/ServiceBusModule/blob/379e32a2b81fc4347685cdb8ac350539a5bc1e06/main.tf#L46) or [variable.tf](https://github.com/ServiceBusAssignment/ServiceBusModule/blob/379e32a2b81fc4347685cdb8ac350539a5bc1e06/variables.tf#L42). | Resource Group, Namespace, Action Group |

### Child Module Folder Structure

```
ServiceBusAssignment
│
├── main.tf
├── variables.tf
│
├── action-group
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
│
├── metric-alert
│   ├── main.tf
│   └── variables.tf
│
├── resource-group
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
│
├── servicebus-namespace
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
│
└── servicebus-queue
    ├── main.tf
    └── variables.tf
```
## Limitations
This module assumes users keep track of whether or not resources already exist. For instance, should Service-A have created a namespace `namespace-a` which Service-D also intends to use, Service-D must import the state file to keep this module from attempting to create the same namespace again. As namespaces must be unique across locations, not importing the state file will result in an error while creating a duplicate resource.

This also goes for all for all other resources.
