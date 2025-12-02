# Initial Cribl.Cloud Setup


Your first step to using Cribl.Cloud is to sign up on the Cribl.Cloud portal (see [Registering a Cribl.Cloud Portal](#register) below), to create your Cribl.Cloud [Organization](cloud-portal#organization).

Your Organization will display a dedicated [portal](cloud-portal#workspaces), a network and access boundary that isolates your Cribl resources from all other users. Each Cribl.Cloud account provisions a separate AWS account. Your [instances](cloud-portal#managing) of Cribl Stream, Cribl Edge, and Cribl Search are deployed inside a virtual private cloud (VPC) in this account.

{{< id `free` >}}
The portal will initially be on a **free** Cribl.Cloud plan. Certain throughput and administration limits apply to a free account. When you need more capacity and/or options, it's easy to upgrade to a [paid](deploy-cloud#pricing) or [Enterprise](cloud-enterprise) plan – just select the **Go Enterprise** button at the top of your portal.

> The Cribl.Cloud Suite is listed on [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-gwiqiktubqhhk). When you're ready for a paid plan, you can use your Enterprise Discount Program (EDP) credits here to run Cribl products, billed through your AWS account – with no need for a separate procurement process.
> 
> Cribl is SOC 2 (Service Organization Control 2) Type II security compliance [certified](https://cribl.io/news/cribl-achieves-soc2-certification/).
>
{.box .info}

## Registering a Cribl.Cloud Portal {#register}

Ready to take the plunge? Here's how to register and manage a Cribl.Cloud instance.

First, if you haven't already signed up on Cribl.Cloud:

1. Start at: [https://cribl.cloud/signup/](https://cribl.cloud/signup/?utm_campaign=criblclouddocsreferral&utm_medium=organic&utm_source=cribldocsdeploycloudregisteringcriblcloudportal&utm_content=ctacriblcloudsignuphyperlink) 
1. Register with your work email address.
1. Use the verification code from Cribl's email to confirm your registration.
1. On the **Create Organization** page, optionally enter an **Organization Name** (a friendly alias for the randomly generated ID that Cribl will assign to your Organization).
1. Select an AWS Region to host your Cribl.Cloud Leader and Cribl-managed Workers. Cribl currently supports the following Regions:
   - `US West (Oregon)`
   - `US East (Virginia)`
   - `US East (Ohio)`
   - `Europe (Frankfurt)`
   - `Europe (London)`
   - `Asia Pacific (Sydney)`
   - `Canada (Central)`

Note that each user can register only one active Organization. For details, see notes on [Member Permissions](cloud-managing#member-roles).

> ##### {{< id `troubleshooting` >}} Troubleshooting Resources
>
> [Cribl University](https://cribl.io/university/)'s Troubleshooting Criblet on [How to Log Into Cribl.Cloud](https://university.cribl.io/#/online-courses/b4f1d236-8ebd-4f38-9f9c-d9b301294994) walks you through the whole registration and login flow. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0) and [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses.
>
{.box .info}
