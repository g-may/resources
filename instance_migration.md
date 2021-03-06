---

copyright:

  years: 2017, 2018

lastupdated: "2018-06-29"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:gif: data-image-type='gif'}
{:tip: .tip}

# Migrating Cloud Foundry service instances and apps to a resource group
{: #migrate}

To make your experience with using {{site.data.keyword.Bluemix}} simpler and more flexible, we have introduced [resource groups](/docs/resources/resourcegroups.html#rgs), which are conceptually similar to Cloud Foundry spaces. However, resource groups include several additional benefits, such as finer-grained access control by using IBM Cloud Identity and Access Management (IAM), the ability to connect service instances to apps and service across different regions, and an easy way to view usage per group.
{:shortdesc}

We are starting to move services from Cloud Foundry to benefit from resource groups, which means that when you see the ![Migrate this service instance to a resource group](images/migrate.svg "Migrate this service instance to a resource group") icon next to one of your services on your dashboard, you must start a migration plan for your service instances or apps that are created through the [{{site.data.keyword.Bluemix_notm}} {{site.data.keyword.dev_console}}](https://console-demo3.bluemix.net/docs/apps/index.html#create) to move from their current Cloud Foundry org and space to a resource group. Until an {{site.data.keyword.Bluemix_notm}} service moves from using Cloud Foundry orgs, spaces, and roles to using IAM and resource groups, you can’t migrate your existing Cloud Foundry service instances to a resource group.

When you migrate existing Cloud Foundry service instances or {{site.data.keyword.dev_console}} apps to a resource group, the group that you choose can't be changed after the migration is complete. So, it's essential that you plan how you want to organize resources in the account before you migrate. This might mean that you need to create one or more resource groups, if you have a billable account, before migrating. 

You can try organizing your resources in resource groups the same way you organized resources in Cloud Foundry spaces. For more information about using resource groups, see [Best practices for organizing resources into resource groups](/docs/resources/bestpractice_rgs.html#bp_resourcegroups).
{: tip}


## Why migrate?

### Cloud Foundry service instances

Services that support Cloud IAM access control and organization within resource groups have several benefits:

* By using fine-grained access control, you can set access to individual service instances or a group of resources organized in a resource group. 
* By using access groups and resource groups to organize users and resources, you set only the minimum number of access policies. For example, if you have a set of developers that you want to all have access to resources for a development environment, you can organize all of those users into a developers access group and then add all the resources that they need access to into a single resource group. Then, you can set a single policy for the access group to have access to all resources in the resource group.
* You can view usage by resource group similar to the way you could view usage by Cloud Foundry orgs.
* You can connect to apps and services in any Cloud Foundry space, which allows connections for apps and services from different regions. When you migrate, the connection is done automatically by turning your original Cloud Foundry service instance into an alias and creating a linked instance in a resource group of your choice. The following graphic depicts how the connection by using an alias works.

![Binding a service instance to a Cloud Foundry space to create an alias](images/alias.svg "Binding a service instance to a Cloud Foundry space to create an alias")

### {{site.data.keyword.dev_console}} apps

Previously, {{site.data.keyword.dev_console}} apps could be associated only with Cloud Foundry service instances. Now, if you migrate your apps to a resource group, you can associate your apps with service instances that belong to a resource group and support Cloud IAM access control. 

## Who can migrate?
{: #whocanmigrate}

### Required access for service instances 

Users must have specific access to migrate Cloud Foundry service instances to a resource group:

* A user must have the Developer role on the Cloud Foundry space or the Organization manager Cloud Foundry role on the organization to which the instance belongs.
* A user must have at least the Viewer IAM role for managing the resource group to which the instance is going to be migrated.
* A user must have at least the Editor IAM role on the service.

For more information about assigning the correct access, see [Cloud Foundry access](/docs/iam/cfaccess.html#cfaccess) and [IAM access](/docs/iam/users_roles.html#platformrolestable).

To check out what access you have, click **Manage** &gt; **Security** &gt; **Identity and Access** from the console menu bar, and then click **Users**. Click your name and review your **Access policies** for assigned IAM roles and **Cloud Foundry access** to see which orgs you have access to and your assigned Cloud Foundry roles.
{: tip}

### Required access for {{site.data.keyword.dev_console}} apps

Any user who can access an {{site.data.keyword.dev_console}} app can migrate it. However, migrating an app does not migrate services associated with the app. Service instances must be migrated separately.

## How does migration work?

### Migrating service instances

When you migrate a service instance from a Cloud Foundry org and space to a resource group, a new linked service instance is created in the resource group. The original instance in the Cloud Foundry org and space becomes an [alias](/docs/resources/connecting_apps.html#what_is_alias). The alias counts towards the quota for your organization, but you are billed for your usage of the service instance in the resource group.

![Migration of a Cloud Foundry service instance to a resource group](images/migration.gif){: gif}

Service instances are migrated one at a time when you are notified on the dashboard by the ![Migrate this service instance to a resource group](images/migrate.svg "Migrate this service instance to a resource group") icon that is associated with your Cloud Foundry service instance.

Before you start the migration process, review your service documentation to see if there are any additional, service-specific changes that you might have to make when migrating your service instance to a resource group. For example, you might need to migrate data from old instances to new instances or update the credentials used for your app if you delete the Cloud Foundry alias. Applications that make a direct call to the API of a service that has been migrated need to update the API call to use either an IAM API key or access token.
{: tip}

1. Open the **More actions** menu.
2. Select **Migrate to a resource group** to get started.
3. Select a resource group.
4. Click **Migrate** and the instance is migrated for you.
5. Since you can migrate only one instance at a time, you can continue migrating eligible instances after you migrate the first one.

After you successfully migrate an instance, you see it reflected in the Services section of your dashboard. The alias remains in the Cloud Foundry section of the dashboard. You can use the ![Link icon](images/link.svg "Link icon that represents an alias") in the Cloud Foundry section of the dashboard to identify the aliases.

### Migrating {{site.data.keyword.dev_console}} apps

Apps are migrated one at a time by clicking the ![Migrate this service instance to a resource group](images/migrate.svg "Migrate this service instance to a resource group") icon associated with each entry in your App List view.

1. Select the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg), and select the developer portal of interest such as Watson, Mobile, or Web Apps, for example.
2. Select **Apps** which displays the lists **Apps (Action required)** and **Apps (migrated)**.
3. For each entry in the **Apps (Action required)** list, click the **Migrate** icon ![Migrate this service instance to a resource group](images/migrate.svg "Migrate this service instance to a resource group").
4. Select or create a new resource group.
5. Click **Migrate**, and the app is migrated for you.
6. Confirm that the app now shows in the **Apps (migrated)** list.
7. Since you can migrate only one app at a time, you can continue migrating eligible apps after you migrate the first one.


## Next steps

After you migrate your Cloud Foundry service instances to a resource group, you need to ensure that the users in your account have the required level of access to the resources in the account resource groups. You might also want to provide access to manage the resource group, so that users can create new service instances in the account resource groups.

For more information about assigning access to resources in your resource groups, see [Assigning access to resource groups and the resources within them](/docs/resources/bestpractice_rgs.html#assigning-access-to-resource-groups-and-the-resources-within-them).

Also, make sure to review the documentation for your service to see if any updates for your existing apps must be made after the migration is complete. 


## Troubleshooting

If you run into any issues with migrating Cloud Foundry service instances, check out [Troubleshooting migrating service instances](/docs/resources/ts_migration.html).
