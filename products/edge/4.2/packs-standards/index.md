# Packs Publication Standards


This page outlines the process for Cribl Community members to publish Cribl Edge [Packs](packs) to the [Cribl Packs Dispensary™](https://packs.cribl.io/). It also lists standards that apply to all publicly available Community Packs.


## Publication Overview {#overview}

Publishing your Pack is a three-step process:

1.  Prepare and Produce.  

    In this initial phase, feel free to share with other Community members, who can help refine the Pack. For this development phase, consider working collaboratively within Cribl's Community Slack `#packs` channel, or on a publicly available Git repo that you own and maintain - for an example of how to use Git to create and manage Packs, see Cribl's [Dispensary GitHub](https://github.com/criblpacks) project.

2.  Publish the Pack to the Cribl Packs Dispensary™.  

    The submission process, outlined [below](#publish), validates that all required fields are included: Pack name, version, author, and license, if the version is newer than the last one published.

3.  Celebrate!

    > If you've already posted completed Packs to Cribl's GitHub repo, we encourage you to submit them to the Packs Dispensary™. See [Publishing a Pack](#publish).
    {.box .success}

## Community Pack Guidelines

A Cribl Community Pack must be useful, reusable, and subject to the Cribl Pack Developer Agreement (PDA).

### Making Your Pack Useful

Above all, what makes a Cribl Community Pack useful is the value that it provides for your fellow Community members. A Pack will be most useful if it includes Pipelines, along with supporting sample files and Knowledge objects (especially Lookups) as needed.

### Making Your Pack Reusable

Reusability means that the Pack brings value to multiple Cribl Edge users. To make this possible, you should be sure your README provides:
    
1. Instructions for using the Pack, including details on how to configure any relevant Sources and Destinations.  

1. Details about the impact on downstream systems, so that users can prepare for changes that the Pack will make to data flowing through.
        
### Acknowledging the Pack Developer Agreement {#pda}

The [Pack Developer Agreement](https://cribl.io/legal/packs-developer-agreement/) (PDA) appears when you first access the Cribl Packs Dispensary™. As a Pack author, you must electronically acknowledge that you have read the Agreement, and that you intend to adhere to its requirements.

## Before you Begin

Before you start creating your Pack, check the Cribl Packs Dispensary™  to see if something similar has already been published. If your idea for a Pack is new: 

- Post to Cribl Community Slack's the `#packs` channel, asking whether any of your fellow Community members are working on a Pack that's similar to yours.

- If someone is already working along the same lines, then you have a good opportunity to collaborate on the Pack you want to create.

- Read the [docs](packs#creating-a-pack) that explain how to create your Pack from within the Cribl Edge UI.

### Creating the Pack

Here's how you should formulate the information you'll need to create the Pack.

Pack names **should not**: 

- Start with `Cribl` or `Cribl-`, which are reserved for Cribl-created packs.
- Use the word `Pack`, at all.

Pack names **should**:

- Start with `cc` (this indicates that the Pack was contributed by a Cribl Community member).
- Use all lowercase.
- Use dashes to separate words, e.g., `cc-tanium-events`. 

Pack Version numbers should:

- Use `0.1.0` for the initial development version of the Pack.
- Designate (number) subsequent versions as described in [Publishing a Pack](#publish).
    
Pack Descriptions should: 

- Be brief - no more than 1 or 2 sentences, e.g., `This Pack for Syslog inputs will reduce volume, and address timestamp normalization for Syslog senders that omit timezones.`
    
Pack Author names should be in the following format: 

- Your Community name, a dash (`-`) and `cc`, e.g., `art chavez - cc`.
    
Pack License Notes should: 

- Appear at the end of the Pack `README` and appropriately linked, e.g., `This Pack uses the following license: [Apache 2.0](https://github.com/criblio/appscope/blob/master/LICENSE).`
    
### Creating Documentation for the Pack

To properly scope the documentation you write for your Pack, follow these principles:

* The optimal user experience is to have a single Pack that supports all datasets for the relevant device or sender.
* The Pack documentation should list all of the datasets that the Pack supports. 
* If there are known datasets that are relevant but not yet supported, the documentation should list those, too.

For example, consider a device sending data to Cribl Edge, such as a Palo Alto Networks firewall. This device supports multiple datasets, in that Palo Alto Firewall data is really one co-mingled set of data that includes individual datasets like `PAN-Traffic`, `PAN-System`, `PAN-Accept`, and so on.

Think about what datasets would be involved for other devices, and how you would document them. For example, consider an F5 load balancer, various types of routers, or various types of servers. This exercise will help you anticipate what your users will need to see documented for your own Pack.
    
## What a Pack Must Contain

Every Pack must contain some combination of Pipelines, samples, Routes, Knowledge objects (including Lookups), configuration descriptions, support contact information, and release notes. The exact ingredients will vary by Pack.

### Pipelines
    
1. Except for rare cases, each Pack should have at least one Pipeline. Packs without Pipelines are of limited use.

2. Cribl recommends that you provide one Pipeline (and one Route) for each dataset that your Pack supports.

3. Each Pipeline should have an internal **Comment** describing the overall functionality **and benefits** that the Pipeline provides. The procedure for adding a **Comment** is the same as for adding any other @{production} Function. **Comment** is under the **Add Function** > **Standard** drop-down.

4. Within each Pipeline, follow these best practices for Functions: 
    * Use grouping to bundle any Functions that a user should enable or disable together.
    * Add a **Description** to each Function, to make it clear what is happening at each step, along with the Function's purpose and mechanics.

5. Overall, design for supportability and efficiency.
        
### Samples
    
1. Include at least one data sample for each Pipeline. Data samples can be reused across Pipelines, but it is preferable to include multiple data samples, each specific to a Route/filter available in the Pipeline.

2. Remember to remove all sensitive data (e.g., internal host names or IP addresses) from samples.
        
### Routes
    
1.  A Pack includes Routes with appropriate filters.

2.  Each Pipeline should have a corresponding Route on the Pack’s Routes page. (The only exception to this is when the Pack serves as a delivery mechanism for pre-processing and post-processing Pipelines.)   

3.  The filter in that Route should be as generic as possible. For example, if the data is coming from `PAN`, don’t assume you’ll have `sourcetype`. Instead, filter by `_raw.match()`.

4.  Each Route must have a meaningful description.

5.  The final Route (where **Filter** is set to `true`) should route to the `devnull` Pipeline.
        
### Knowledge Objects
    
1.  Include within the Pack all Knowledge objects that the Pack requires – [Lookups](lookups-library), [Parsers](parsers-library), [Global Variables](global-variables-library), [Grok Patterns](grok-patterns-library), and [Schemas](schema-library).

2.  Remember to remove all sensitive data from Knowledge objects – e.g., internal host names and IP addresses.

3.  Lookups are especially important. If it's not practical to include the required Lookup, include instructions for building it.
                
### Configuration Descriptions

Where applicable, the Pack documentation should include clear descriptions for each pre-shipped configuration.

### Support Contact Information
    
The Pack documentation should list your preferred method of contact for support, e.g., your Cribl Community name for Slack DMs, or your email address.

### Release Notes

If the Pack is an update from a previous release, the documentation should include Release Notes.   

## What a Pack Should Contain

A logo and a `README` file are recommended, but optional.

### The Logo

You can include a logo associated with the technology addressed by the Pack, e.g., a Windows logo for Windows events, an AWS Cloudwatch logo for AWS Cloudwatch data, and so on. 

### The README

Including a `README` improves the user experience for your Pack. Create a `README.md` file from the Pack's **Settings** submenu, with detailed answers to the following questions:

1.  What does the Pack do, and why was it created? Here, you can state what value the Pack provides.   
2.  What technologies, data sources, and data destinations does the Pack interact with?  
3.  What other dependencies does the Pack have? As examples: 

    - Does the Pack use an external tool, like Redis?
    - What is the minimum supported version of Cribl Edge? This must be newer than Cribl LogStream v.3.0.4 or Cribl Edge 3.3.0.
    - What deployment restrictions apply? What combination of single instance, distributed, or Cloud deployments of Cribl Edge does the Pack support?

4.  What is required to configure a data source or destination?

    - Include specific examples of configurations when possible.
    - Link to specific sources and destinations to configure, e.g., link to AWS Firehose Source for a Pack that requires Firehose to collect data. 

5.  What else does your user need to know to use the Pack?

    - Include specific instructions.
    
6.  How should your users contact you for support?  
    - The `README` should contain the Pack author's contact info for providing feedback/requesting support, e.g., your Community name or email address.
            
## Versioning Packs {#versioning-packs}

Packs start at version `0.1.0`, and continue through as many "pre-release" versions as needed, until the authors feel that the Pack is ready for production use. During this initial **Prepare** phase, you'll share your idea with other Cribl Community members, to collaboratively refine the Pack.

Next, you'll enter the **Publish** phase, where it's appropriate to release version `1.0.0`. Here, you'll:
- Satisfy all the requirements for publishing a Pack, as documented above.
- Export the Pack by creating a `.crbl` file.
- Upload the Pack to the Cribl Packs Dispensary™, as  outlined [below](#publish). The Dispensary will validate the Pack name, version, author, and license, including by checking that the new version number is greater than the last one.
        
### Versioning an Existing Pack

The easiest way to do this is to make the Pack changes in Cribl Edge, then export the new Pack and upload it to the Cribl Packs Dispensary™.

1.  Update your Pack.

2.  Increment the version number. See [semantic versioning](https://semver.org/) for guidelines.

3.  In the `README`, update the **Release Notes** section to specify the new version and release date, and to describe what has changed. For a good model of how this is done, see the `README` on the Dispensary's **Microsoft Windows Events** Pack.

4.  Export your Pack and save the `.crbl` file locally.

5.  Upload your Pack's `.crbl` file to the Cribl Packs Dispensary™, as outlined just [below](#publish). Remember to version the file.

## Publishing a Pack {#publish}

To submit your Pack:

1. Sign into, or create an account on, the Cribl Packs Dispensary™ [site](https://packs.cribl.io/). You can create an account using the same email address as used on your [Cribl.Cloud](https://cribl.cloud/signup/) account.  

   If you don't have a Cribl.Cloud account, Cribl automatically creates a new one for you when you create your account on the Cribl Packs Dispensary™.

1. Once signed in, you'll see the **Publish Pack** and the **View only my Packs** controls shown below.

    ![Packs Dispensary™: Signed-in view](se-packs-publish.f2a22c9517.png)
    {border="true"}

1. Click **Publish Pack**, and then click **Upload Pack**.

1. Select the `.crbl` Pack file you want to publish, and click **Submit**.

1. The Packs Dispensary™ will quickly verify whether the Pack has valid configurations and whether it meets all of the requirements outlined in this document.

1. If validation is successful, the Dispensary submits the Pack for review, and graduates it to a `Validating` state.

1. Once your Pack is reviewed, you'll receive an email from `packs@cribl.io` informing you whether it was accepted or rejected.  

    Rejected Packs display on the Packs Dispensary™ as `Failed`, with a note about the rationale for rejection. After you've fixed your Pack, you can resubmit it.

## Who Supports Packs

- Community-authored Packs are primarily supported by the Pack author, who also handles feature requests and suggestions.
- Cribl-authored Packs are supported by Cribl. 
- For both kinds of Packs, the greater Cribl Slack Community also provides a wealth of knowledge - see the `#packs` channel.
