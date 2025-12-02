# Register Cribl.Cloud Organization


To get started with Cribl.Cloud, sign up on the [Cribl.Cloud portal](https://cribl.cloud/signup/?utm_campaign=criblclouddocsreferral&utm_medium=organic&utm_source=cribldocsdeploycloudregisteringcriblcloudportal&utm_content=ctacriblcloudsignuphyperlink), to create your Cribl.Cloud Organization.

Your Organization will have a dedicated portal with its own network and access boundaries, isolating your Cribl resources from other users. For more information, see [Cribl.Cloud Data Isolation](/reference-architectures/reference-arch-full-suite#isolation).

{{< id `free` >}}
By default, your instance of Cribl.Cloud will be on the **free** Cribl.Cloud plan, which includes certain throughput and administration limits. When you need more capacity or features, you can upgrade to a [paid](deploy-cloud#pricing) or [Enterprise](cloud-enterprise) plan. To upgrade, select **Go Enterprise** in your Cribl.Cloud portal.

> You can find the Cribl.Cloud Suite on the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-gwiqiktubqhhk). When you're ready to upgrade, you can use your Enterprise Discount Program (EDP) credits to run Cribl products, billed directly through your AWS account. There is no need for a separate procurement process.
>
{.box .info}

Note that each user can register only one active Organization on the free tier. For details, see notes on [Member Permissions](permissions#organization).

To sign up for a free Cribl.Cloud instance and register your Organization:

1. Go to [https://cribl.cloud/signup/](https://cribl.cloud/signup/?utm_campaign=criblclouddocsreferral&utm_medium=organic&utm_source=cribldocsdeploycloudregisteringcriblcloudportal&utm_content=ctacriblcloudsignuphyperlink) and create an account using your work email address.

1. Follow the instructions to set up your Organization, select its [region](#region) for your Cribl.Cloud deployment, and to provide some information about your goals in using Cribl.

When you complete the registration steps, you will see your new Cribl.Cloud deployment home page. The home page suggests possible actions you can take based on the answers you provided during registration.

From there, consider completing the [Getting Started Guide](/getting-started-guide) tutorial for a walk-through of the main features and common use cases for Cribl Stream.

## Cribl.Cloud Organization Regions {#region}

The Organization region defines where the Leader Node resides.

Once you select a region when registering your Organization, you cannot change it.
You can, however, create [Cribl-managed Stream Worker Groups](/stream/cloud-workers) in different AWS and Azure regions.

In [Cribl Search](/search/), the Organization region is the physical location where searches are executed.
In [Cribl Lake](/lake/), the region defines the location where data is stored.

## Troubleshooting Resources

If you get stuck during registration, [Cribl University](https://cribl.io/university/) has a troubleshooting guide on [How to Log Into Cribl.Cloud](https://university.cribl.io/#/online-courses/b4f1d236-8ebd-4f38-9f9c-d9b301294994) walks you through the registration and login workflow.

To follow the direct course link, first log into your Cribl University account. Once logged in, check out other useful short courses:

- [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0)
- [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73)
