# Splunk to Exabeam


In many organizations, the IT department uses a tool like Splunk for operational logging, while the Security team relies on Exabeam to prevent insider threats. These tools use separate agents to access the same data, leading to some data-sharing conundrums:

 - Installing the Exabeam agent in parallel with Splunk would duplicate the data. 
 - Some servers, like domain controllers, allow only a single agent. In this case, you can't feed two platforms with the same data. 
 - Querying Splunk for the data would introduce extra latency and overhead costs. 
 - Forwarding data directly from Splunk Universal Forwarders (UFs) is a nonstarter. Classic logs from Splunk UFs embed newlines and special characters, which break Exabeam’s parser.

Cribl Stream can help you unblock these issues: Ingest data directly into Cribl Stream from Splunk UFs running on the domain controller, and transform the events in Stream before routing them to Exabeam. 

> While this example is written around a Splunk-to-Exabeam scenario, you can use the same general techniques to connect and transform data between several other upstream and downstream services.
>
{.box .success}

## Transform Data from Splunk to Exabeam's Format

In Cribl Stream's core, you can easily design a Pipeline that modifies the original Splunk event to fit the format that Exabeam expects. Some  Cribl Stream Functions useful for this transformation are:

- [Serialize](serialize-function): Remove all of the newlines and spaces, and then transform the data into `JSON` format.
- [Mask](mask-function): Remove the special characters, and replace them with space.
- [Eval](eval-function): Create a new field called `Message`, and remove everything else using the **Remove Fields** option. 

In this guide, we'll show you how to:
1. Capture sample logs.
2. Apply the Functions (outlined above) to transform the sample log into Exabeam's expected format.
3. Validate your fields with Exabeam Parsers.
4. Stream your event to Exabeam to test the entire sequence.  

## Capture Sample Logs 

Start with [sample logs](usecase-sample-logs) of the event data you plan to work with. In our example, we'll copy and paste the log sample straight into Cribl Stream.

Once you've pasted the log sample, enter a unique **File Name** on the modal's left side, then select **Save as Sample File** at right.

![Saving captured data](st-usecase-exabeam-step1b-4101.png)
{border="true"}

In your production environment, you can filter incoming events in real time (e.g., using a filter expression like `Windows Security logging`) to identify your in-scope Exabeam data.

## Configure Pipeline

Next, this section shows relevant Functions that you can assemble into a processing [Pipeline](pipelines) to transform the sample log into Exabeam's expected format. 

### Serialize Function 

First, use a Serialize Function to change the event's format into `JSON`. Then, remove the extra lines and spaces, because Exabeam treats each newline as a separate event.

![Reformatting the event](st-usecase-exabeam-step2-4101.png)
{border="true"}

### Mask Function

Next, use a Mask Function to remove extra `\n` and `\r` characters. (Otherwise, Exabeam's regex filter would reject the characters and drop whole fields that need to be matched and extracted.)

In this example, we are using a Mask Function to remove these special characters and replace them with spaces.

![Extraneous newlines/returns to remove](st-usecase-exabeam-step3-4101.png)
{border="true"}

### Eval Function

As the last step in Cribl Stream, use an Eval Function to enrich the outbound events, by adding key-value pair that defines and populates a field conforming to the Exabeam schema. Name the new field  `Message`. (The `Message` field will populate `Raw Message` in the Exabeam Data Lake.) Remove everything else using **Remove Fields**. 

![Eval Function to add an Exabeam-specific field](st-usecase-exabeam-step4-4101.png)
{border="true"}

## Goat the Fields? {#parser}

Use Exabeam's Auto Parser Generator (APG) tool to validate your fields. If you don't have the APG tool, contact Exabeam support to request access via your ECP account. This tool validates matching parsers based on your event. 

![APG tool access in Exabeam](st-usecase-exabeam-step5.a12c84ba15.png)
{border="true"}

In the APG tool, select **New Parser**.

![New Parser option](st-usecase-exabeam-step5b.ed61a33810.png)
{border="true"}

On the **Create Parser** page, select **Copy and paste raw log lines**. 

In the text box, paste the **Message** field value from the your sample file and select **Upload Log Sample**. 

![Paste Message Field](st-usecase-exabeam-step6.4814deb284.png)
{border="true"}

> Copy the *Message** field value to your clipboard for a later step in [Stream it to Exabeam](#stream_exabeam). 
>
{.box .success}

Select **View Parsing Details** to view parsers matching this event type.

![New Parser option](st-usecase-exabeam-step9b.492f1f34a7.png)
{border="true"}

Validate that all the fields you need are populated: `src_ip`, `dest_host`, `user`, etc. 

![Paste Message Field](st-usecase-exabeam-step7.fe29bf7b7a.png)
{border="true"}

> Do not change Field Names in Exabeam (e.g., `src_ip` to `source_IP`). Exabeam has its own Field Name format that matches its Advanced Analytics template. 
>
{.box .warning}

At the bottom of the **Extraction Preview** page, select **Configuration Files** to inspect the parsers. If required, you can download the parser to change the regex configurations. 

![Parser Details](st-usecase-exabeam-step8.587eac691f.png)
{border="true"}

## Stream It to Exabeam {#stream_exabeam}

Our final test is to send a single event to Exabeam, and validate the results in Exabeam's Data Lake or Advanced Analytics.

> Before testing, configure your Exabeam implementation's `Asset Name` and `Username` to a dummy account. Exabeam's Advanced Analytics has hundreds of models out of the box, and you do not want to ruin these models while testing out your data. 
>
{.box .danger}

1. From Cribl Stream Home, select a Worker Group, and then select **Data** > **Destinations** > **Syslog**.
2. Select **Add Destination** to configure and save a new [Syslog Destination](destinations-syslog) to export data to Exabeam.  

   Configure the **Address**, **Port**, **Message format**, **Timestamp format**, and other options to match your Exabeam implementation.

![Configure Syslog](st-usecase-exabeam-step9a-4101.png)
{border="true"}

3. Reopen the Syslog Destination. On its **Test** tab, copy and paste the **Message** field's value you copied onto your clipboard [above](#parser).

![Syslog Test tab](st-usecase-exabeam-step9.ef2c636229.png)
{border="true"}

4. Within a few seconds, you should see your event display in the Exabeam Data Lake. Search for your forwarder IP/Host (Example syntax: `Forwarder:”IP/host`). 

![Validating event in Exabeam](st-usecase-exabeam-step10.6c1c1189dc.png)
{border="true"}

5. You can validate that you have the right parser by matching the `exa_parser_name` with the parser in the Auto Parser Generator. 

![Validating parser in Exabeam](st-usecase-exabeam-step11.f4027b12fd.png)
{border="true"}
