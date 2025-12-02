# Cribl Search 4.2


## New Features

### Authorization

We're excited to introduce [Members and Permissions](cloud#multi-user), addressing the demand for finer-grained access control and authorization. This release empowers you to assign specific access levels and capabilities to users independently at the Organization, Product, and Resource levels.

Cribl Search now offers [resource permission](sharing) levels that allow you to restrict access to specific Datasets and Dataset Providers.

Upcoming enhancements will include fine-grained access control based on values, columns, and fields within Datasets, as well as the ability to control access to specific functionalities within Cribl Search.

### Dashboards

Introducing [Dashboards](dashboards), customizable visual displays of your search data that you can create, manage, and customize with ease. A variety of widget types and visualizations allow you to tailor your Dashboards to best fit specific requirements.

### Notifications

Introducing [Notifications](scheduled-searches), a powerful tool that helps you monitor systems and optimize workflows. You can now set up notifications that are triggered based on the evaluation of search results against a boolean condition. This release provides notifications as bulletin messages, [webhook connections](notification-targets), or integrations with [PagerDuty accounts](notification-targets). Stay informed about critical data events and take immediate action to drive efficiency and effectiveness.

### Dataset Providers

We've added new Dataset Providers so you can easily connect and retrieve the data you need for your analysis.
* [Azure](set-up-azure-api)
* [Tailscale](set-up-tailscale)

[Azure Blob Storage](set-up-azure-blob) now supports Shared Access Signatures (SAS) allowing you to specify which resources to grant access to.

### Enhanced User Experience

A new [export operator](export) allows you to create or update lookup tables directly from search results, providing seamless enrichment to your data.

A new [project-rename operator](project-rename) allows you to easily rename fields by defining a mapping process. This simplifies the task of organizing and clarifying data by assigning more intuitive and meaningful names to fields.

The [strftime function](strftime) allows you to easily convert datetime (date) objects into a format that is easily understandable by humans. This functionality greatly improves data comprehension and usability.

The [strptime function](strptime) allows you to effortlessly convert strings into datetime objects. This functionality empowers you to perform various operations and analyses on date and time data with ease.

We've added a [Lookaround](search-overview#lookaround) feature that lets you easily filter search results by adding or subtracting seconds, minutes, hours, or days, enabling quick exploration of surrounding events.

We improved the [Date Range](build-a-search#time-range) modal! Here’s what changed:
* The **Time Range** tab is now called **Date Range**. Select a start and end date and time from one drop-down.
* Now, when you click a relative time from the list, the modal closes and applies your selection so you can search faster.
* Introducing the **Around** tab. You can use this tab to select a custom date and specify a duration of time "around" that date. This is useful for finding important events that you know happened around a particular date, but you don't know exactly when.
* The **Date Range** modal displays **Current Timezone**, which you can change.
* The time zone picker is more searchable, so you can search by country, city, UTC offset (+8, for example), and time zone name.
