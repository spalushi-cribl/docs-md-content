# Packs Publication Standards


This page outlines the process for Cribl Community members to publish Cribl Edge [Packs](packs) to the [Cribl Packs Dispensary](https://packs.cribl.io/). It also lists standards that apply to all publicly available Community Packs.


## Publication Overview {#overview}

Publishing your Pack is a five-stage process:

1. **Prepare**
    
    - Prior to starting development, read the [Pack Review Summary](#review-summary) and [Pack Review Checklist](#review-checklist) below.  This will help you create a Pack that can easily pass the review process. Decide on the problems you’re trying to solve, and determine the dataset(s), Sources, and Destinations you’re going to work with.

2. **Develop your Pack**
    
    - Development is generally done in your own Cribl environment, using saved samples to build out pipelines. During this stage, you might consider working collaboratively within Cribl’s Community Slack `#packs` channel. When the Pack is ready for review, export the `.crbl` file using the Cribl UI.


3. **Test your Pack**

    - Cribl strongly recommends that you test the Pack yourself, in your own environment. To test that all content was exported correctly, import your Pack into a fresh Cribl environment, such as a newly created Docker instance, or to a newly created Worker Group in your Cribl environment. Upload the Pack, then use the Pack Review Checklist to verify that the Pack will pass review.

4. **Submit your Pack for review**
   
    - Sign into, or create an account on, the [Cribl Packs Dispensary](https://packs.cribl.io/) site. You can use your [Cribl.Cloud](https://cribl.cloud/signup/) account's email address to create the Cribl Packs Dispensary account.

    - If you don’t have a Cribl.Cloud account, Cribl automatically creates a new one for you when you create your Cribl Packs Dispensary account.

    - Once you sign in, you'll see the Dispensary page with the **Publish Pack** button.

        ![Packs Dispensary controls](packs-standards-01.png)

    - Click **Publish Pack**, and then click **Upload Pack**.

    - Select the `.crbl` Pack file you want to publish, and click **Submit**. You will be prompted to agree to the terms of Cribl's [Pack Developer Agreement](https://cribl.io/legal/packs-developer-agreement/). As a Pack author, you must electronically acknowledge that you have read the Agreement, and that you intend to adhere to its requirements.

      > Once the Pack has been uploaded, Cribl will review your Pack. The review process typically takes a week or more. Once your Pack is reviewed, you’ll receive an email from `packs@cribl.io` either informing you that it was accepted, or providing the reason(s) why it was rejected.
      {.box .info}

5. **Once your accepted Pack is published to the Cribl Packs Dispensary, it's time to celebrate!** 

## Pack Review Summary {#review-summary}

To ensure consistency across published Packs, Cribl evaluates all submitted Packs against the checklist below. Any Pack that fails to satisfy the checklist's requirements will be rejected.

Cribl recommends:

* Check your Pack against the checklist **before** trying to submit the Pack. This way you can avoid having your Pack rejected for something easily fixed.

* Install the **Syslog Pre-processing** (`cribl-syslog-input`) Pack as a reference – it includes clear examples of each item on the checklist.

## Pack Review Checklist {#review-checklist}

### 1. Naming  

Every Pack must have both a Pack ID and a Display Name.

When you view Packs in the Dispensary, the Display Name appears above the Pack ID. For example, **Syslog Pre-processing** is a Display name while `cribl-syslog-input` is a Pack ID:

![Display Name and Pack ID](packs-standards-02.png)

#### Pack ID

The Pack ID: 

* Must start with either `cc-` (community-contributed) or `cribl-` (Cribl-contributed). Packs not satisfying this requirement will be immediately rejected.

* Should (by convention) be all lowercase, using the hyphen (`-`) as a delimiter.

* Should **not** include the word “pack.”

For Packs normalizing to or from Elastic's Common Schema, the Pack ID (but not the Display Name) should include `-source` or `-dest`.

#### Display Name

The Display Name is a human-friendly version of the Pack name that: 

* Omits `cc-` and `cribl-`.
  
* Uses spaces where appropriate, and proper capitalization.

* If it includes a company or product name, matches the company’s preferred spelling, spaces, and capitalization. 

You can view or edit the Display Name on the **Pack Settings** tab.

#### How to Review the Pack ID

This is the same procedure that the Cribl will perform when reviewing your Pack. 

1. Extract the `.crbl` tarball that was submitted to the Dispensary:
   
   `tar zxvf <your_filename>.crbl`

2. View the `package.json` file:
   
   `cat package.json`

3. Check the value of the `name` field: this should be the Pack ID that you want.

   ![Verifying the Pack ID](packs-standards-03.png)

### 2. Pack Settings

You must review your **Pack Settings** to verify that the Pack satisfies all of the following criteria.

#### Display Name

A human-friendly version of the Pack name as defined above.

#### Version

The version number in the submitted Pack must have a corresponding entry in the README Release Notes.

* The minimum version for initial publication is `0.1.0`. A Pack with a lower version **will be rejected**.

* A release that is considered "beta quality" should be designated as version `0.9.0`.

#### Description

Use this as a brief "hook" of one to two sentences. Example: `This pack for Syslog inputs will reduce volume, and address timestamp normalization for Syslog senders that omit timezones.`

#### Author 

Community-contributed Packs may include the author’s name, a company name, or both.

All three of the following examples are valid: 

1. `Jon Smith`
1. `Bazookatron`
1. `Jon Smith - Bazookatron`

Cribl-published Packs must use author’s full name, plus ` - Cribl`. Example: `Michael Donnelly - Cribl`.

#### Data Type, Use Cases, Technologies

Select all that apply.

#### Tags 

Tags are optional but recommended, especially where multiple Packs are intended to work together. Tag examples include: `OCSF`, `AWS`.

#### Logo

The logo is optional, but recommended. You can include a logo associated with the technology addressed by the Pack. For example, a Windows logo for Windows events, an AWS Cloudwatch logo for AWS Cloudwatch data, and so on.

You can set the logo via **Pack Settings** > **Settings** > **Display**. 

### 3. Routes

A Pack submitted must include at least one Route in addition to the default Route.

* A Pack’s Routes page must have a Route, with an appropriate filter, routing to a Pipeline.

* The filter should match **only** the set of data that the Pipeline is designed to process. The filter must **not** be set to `true`. 

* If the Pack deals with multiple datasets, each dataset should have its own Route/Pipeline pair. (For a good example, look at the PAN Pack.)

* The Pack must not use the `default` Route to send events to a Pack-specific custom Pipeline. The `default` Route may have a Pipeline set to `main`, `devnull`, or `passthru` to handle events that do not match the Pack filter(s).

* Each Route must include a short description of the dataset being matched.

* Pairs of Routes and Pipelines should have matching names.

### 4. Pipelines

A Pack submitted must include at least one Pipeline in addition to the stock Pipelines (`devnull`, `main`, `passthru`, `prometheus_metrics`, etc.).

* If the Pack deals with multiple datasets, there must be one Pipeline (and one Route) for each dataset. (For a good example, look at the PAN Pack.)

* The Pack must not use the stock Pipelines. Leave these Pipelines unmodified. 

* Every Pipeline must begin with a Comment Function that gives an overview of the Pipeline’s operation as a whole. This Comment is a synopsis of what the entire Pipeline will be doing. Refer to the **Syslog Pre-processing** (`cribl-syslog-input`) Pack for an example of the expected level of detail.

* If Function Groups are used within the Pipeline, each Function Group must begin with a Comment Function that gives an overview of what will happen within that Function Group.

* Every Function must have a Description that briefly explains exactly what is happening in that particular Function. During review, change the Pipeline view option to display the Description for each Function.

* Using the sample files provided with the Pack, validate that the Functions work as detailed in the Comment at the top of the Pipeline.

### 5. Sample Files

The Pack must include at least one sample file for each of its Pipelines.

Sample files: 

* Must **not** have an expiration date. 

* Must **never** include PII, customer data, or customer hostnames.

* Must be named in such a way that it's obvious which sample file(s) apply to each Pipeline.

* Must contain samples that are adequate for a customer to use to review the operation of a Pipeline. To allow performance analysis of the corresponding Pipeline, a sample should include 20 or more events, with 100 or more recommended.

* May be reused across Pipelines, but it is preferable to include multiple data samples, each specific to a Pipeline’s Route and filter. 

* Must work as expected on a clean installation of the Pack. This applies to all samples listed in the Pack – Pack developers should [test this](#overview) before submitting the Pack.

### 6. README

The `README` is the documentation others will use in selecting and deploying the Pack. The `README` requirements ensure that others will be able to deploy the Pack successfully.

> The default sections for README, as created when you click **Create Pack**, do not match the required and optional sections in the checklist. You will need to delete or replace certain sections.
{.box .warning}

On the Pack’s **Pack Settings** > **README** tab, validate that the README text is correct. The following rules apply to all sections of the README:

* Proper grammar will help the users of your Pack. Use a grammar and spelling checker to make sure the README is clear and readable.

* If the README includes any text that looks like a placeholder, the Pack will be rejected.

* Trademark or product names must be spelled and capitalized exactly as the company that owns them would dictate. This requirement also applies to Pack settings, inline comments, and other uses. Examples: Amazon (not amazon), GCP (not Gcp), CrowdStrike (not Crowdstrike). 

* The README must include all of the sections listed below (sections **6a** through **6f**). The README may include additional sections as necessary.

#### 6a. About This Pack

Think of this initial section as the first paragraph of an essay, or as the only paragraph a person will read when deciding to use the Pack. It must clearly state what the Pack does. Start with the benefits of using the Pack. In sum, this section should answer the question, **Why should anyone use this Pack?** 

If the Pack is tied to a specific data source, dataset, or destination, spell that out clearly here – even if that source or destination is in the Pack name.

#### 6b. Deployment

This **required** section must include all instructions necessary to get the Pack up and running. If the Pack requires any configuration after initial installation - but before it's put it into production - you must specify the configuration steps here.

**Filters**: If applicable, say what filter must be used to route data to the Pack, or explain how to configure the filter. A filter based on `__inputId` is much more efficient than a match against `_raw`.

**Sources**: If the Pack requires a specific Source, include information on how to configure that Source. At a minimum, this needs to cover setting up the Source within Cribl. In some cases, this might include information on how to set up an external environment for collection by Cribl as well.

**Destinations**: If the Pack requires a specific Destination, include information on how to configure that Destination. Does it need a post-processing Pipeline tied to the Pack? 

**Pipelines**: If the Pack includes Functions (within a Pipeline) that need to tailored, enabled, or disabled, include a full explanation and instructions.

**Lookups**: If the Pack includes Lookup files that need to be tailored, you must specify the configuration steps here.

#### 6c. Upgrades

This **optional** section is strongly recommended if you expect others to customize their copy of the Pack and later upgrade the modified Pack.  

* If included, this section should cover the usual set of problems with Pack upgrades as described in [Upgrading an Existing Pack](packs#upgrading), and should explain the specifics of upgrading this particular Pack.

#### 6d. Release Notes

This **required** section is a list of release notes where each release note:

* Starts with the version number and date of release, separated by a space, a hyphen, and a space. For example: `Version 1.2.0 - 2022-07-11`.
  
* Continues with bullet points that describe what has changed and/or is new in the release.

![Release notes](packs-standards-04.png)

Release notes must adhere to the following rules:

* Designate the initial release version `Initial release`.

* The most current version listed must exactly match the Pack version in **Pack Settings** > **Settings**.

* Release notes versions must be listed starting with the most recent (in descending chronological order).

#### 6e. Contributing to the Pack

This **required** section explains how to contribute to the Pack.

* The information provided must be **actionable**. A section reading `[Contributor instructions go here.]` would be rejected.

* Cribl recommends that you refer potential contributors to Cribl’s Community Slack. For example, Cribl-created Packs read as follows:

    ```
    To contribute to the Pack, please connect with us on [Cribl Community Slack](https://cribl-community.slack.com/). You can suggest new features or offer to collaborate.
    ```

#### 6f. License

This **required** section must include text that designates **and links to** a legitimate license.

* Community-contributed Packs must include a license that allows for use by the general public, such as Apache 2.0, MIT, or GNU.

* Built-by-Cribl Packs must use the following snippet:

    ```
    This Pack uses the following license: [Apache 2.0](https://github.com/criblio/appscope/blob/master/LICENSE).
    ```
