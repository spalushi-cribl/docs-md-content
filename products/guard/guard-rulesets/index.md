# Explore Cribl Guard Rulesets


The Cribl Guard rulesets are a collection of predefined rules you can use to
detect common patterns in your event data, such as email addresses, credit card
numbers, API keys, authorization tokens, and more. Cribl Guard uses rulesets,
rather than rigid pattern matching, to help process your data faster. You can
start with the out-of-the-box rulesets in this guide, or create custom rules and
add them to new or existing rulesets.

## Custom Rules for Sensitive Data Scanning

You can create custom rules using regex and anchor terms in the 
[Cribl Knowledge library](/stream/knowledge-library), then add them to new or
existing rulesets.

### Glossary of Terms

|Term|Definition|
|----|----------|
|**Protect**|The toggle on the Cribl Guard homepage that adds a Post-Processing Pipeline to the target Destination with the Cribl Guard Function. Credit consumption begins as soon as data flows through the Cribl Guard Function.|
|**Scanning Rulesets**|In the Cribl Guard Function, scanning rulesets are combinations of rules and anchor terms that identify sensitive data in your event streams.|
|**Pattern to match**|The regular expression pattern that identifies strictly on patterns in your event data. For example, a pattern to match on credit cards would look for strings of numbers between 0-9, including hyphens or periods, in a specific order.|
|**Anchor terms**|The optional terms or characters that you can add to Cribl Guard rules. Anchor terms help improve the accuracy of data matching and limit unnecessary data redaction. <ul><li>For example, adding the anchor terms `cc`, `creditcard`, and `visa` could help identify a 16-digit number string as a credit card instead of a similar, 16-digit UUID. Matches are highlighted in the sample events panel.</li><li>You can add up to a maximum of six anchor terms to a Cribl Guard rule.</li></ul>|

### Examples of Sensitive Data Flagged by Cribl Guard Rules

#### Example 1: Pattern to match and anchor terms provided

In this example, we've added a pattern to match email addresses and provided
anchor terms that help Cribl Guard accurately flag and mitigate sensitive data.

![Cribl Guard rule set up to redact email addresses](guard-email-example.png)
{border="true" scale="90%"}

And this is the preview of the output.

![Cribl Guard output preview with email field redacted](guard-email-redacted.png)
{border="true" scale="90%"}

#### Example 2: Only pattern to match provided

In this example, we only have the pattern to match United States passport
numbers. While Cribl Guard does flag the passport number, it is based only on
the regex logic.

![Cribl Guard rule set up to redact passport numbers](guard-passport-example.png)
{border="true" scale="90%"}

And this is the preview of the output.

![Cribl Guard output preview with passport field redacted](guard-passport-redacted.png)
{border="true" scale="90%"}

## Use Cribl Out-of-the-Box Rulesets

Cribl comes out of the box with the following rulesets. Each ruleset contains
around 20-25 rules, and are organized into the following categories:

* [Default](#default)  
* [Secrets and credentials](#credentials)
* [Bank and credit cards](#financial)
* [Medical codes and licenses](#health)
* [Network authentication and configs](#network)
* [Personally Identifying Information (PII)](#personally-identifying-information)

### Default Ruleset

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **ABA Routing Number** | 9-digit numerical codes used to identify United States federal or state chartered banking institutions. | routing number; aba routing; aba |
| **US Social Security Number** | 9-digit numerical codes issued to United States citizens, permanent residents, and working residents to identify individuals for tax, employment, and social benefit purposes. | ssn; social; socialsecurity; socialsecuritynumber |
| **US Passport** | Forms of documentation for citizens and nationals to be used for international identification recognition. | us; passport|
| **US Bank Account** | Identify individual accounts with a bank or financial institution. Most account numbers range from 8-12 digits long but can contain up to 17 digits. | bank; account; us; acct |
| **US Individual Tax Identification Number** | 9-digit unique identifiers used for federal tax purposes in the event an individual taxpayer is not eligible for a social security number.| itin; tax identification number |
| **DSA Private Key** | Can be used to authenticate to services. It works with a public and private key set.| dsa; key; dsa_key |
| **EC Private Key** | Can be used to authenticate to services. | ec; key; ec_key |
| **Email** | Electronic mail delivers messages over computer networks between one or many address destinations. | email; em; mail; mailto |
| **Encrypted Private Key** | Keys that have been encrypted (usually with a passphrase). This can span DSA, EC, RSA, or any private key. | key; private_key |
| **General Private Key** | A generic private key can represent any type of cryptographic key standard. | key; private_key |
| **HTTP Basic Authentication Header** | For HTTP calls, basic authentication allows for a request to authenticate using a username and password, alongside the host. | user; username; password; pwd; pass; passwd; authorization; basic |
| **HTTP Cookie** | Contains stored HTTP cookies associated with the server (i.e., previously sent by the server). | cookie |
| **JSON Web Token** | A compact, URL-safe means of representing claims to be transferred between two parties. | jwt; token; decoded_token; password; secret |
| **PlainText Private Key** | A private key that has not been encrypted and is stored or used in its original, readable form. | apikey; privatekey; api_key; private_key; key |
| **PuTTY Private Key** | Provides a quick and simple way to connect to remote hosts. | key; private_key; privatekey; private-lines |
| **RSA Private Key** | Can be used to authenticate to services. It works with a public and private key set.  | key; private_key; beginrsaprivatekey; endrsaprivatekey; rsaprivatekey |
| **SSH DDS Public** | A protocol for secure remote login, command-line and other secure network services over an insecure network. | sshpass; pass; pwd; passwd |

### Credentials

Secrets and credentials rules are organized into the following categories of
rulesets:

- [AI](#ai-credentials) 
- [Amazon](#amazon-credentials)
- [Azure](#azure-credentials)
- [Communication](#communication-credentials)
- [Cryptographic keys](#cryptographic-keys)
- [Financial](#financial-credentials)
- [Google](#google-credentials)
- [SaaS](#saas-credentials)
- [Social](#social-credentials)
- [Web](#web-credentials)

#### AI Credentials

| Rule | Description | Anchor terms |
|--------------- |--|--|
| **AI71 API Key** | Provides access to the company AI models: Falcon LLM models. | ai71; api_key |
| **Anthropic Admin Key** | An AI safety and research company. The Admin API allows you to programmatically manage an organization's resources. | anthropic; sk; ant; admin |
| **Azure OpenAI API Key** | A cloud computing service created by Microsoft. Open AI is an AI service which enables one to train its own LLM to fit its needs. | azure; aoai; azure_openai_api_key; |
| **Clarifai API Key** | An AI company specialized in computer vision. The service is proposed via an API and allows to create and train AI models, label images, identify elements in them. | clarifai; clarifai_api_key |
| **Claude API Key** | An AI safety and research company. Claude is their dialogue and creative content generation model. | anthropic; claude; claude_api_key; anthropic_api_key |
| **Cohere API Key** | Builds NLP-based features (generation, summarization, etc.) into applications, by leveraging large language models. The API allows to interact with such models. | cohere; co; client |
| **Coze API Personal Access Token** | An AI application development platform. PATs are used to authenticate requests to the Coze API. | coze; pat |
| **Fireworks AI API Key** | An API for integrating artificial intelligence capabilities into applications. API keys are used to authenticate requests and manage access to the API's features. | fw; key; fw_key |
| **Google Gemini API Key** | A family of large multimodal AI models and also the name of the AI assistant powered by these models. | gemini; gemini_api_key; secure-1psid |
| **Groq API Key** | A service allowing to integrate LLMs into low-latency applications. The API allows the user to retrieve the model(s), and complete text strings. | groq; groq_api_key |
| **Hugging Face User** Access Token | A company that creates machine learning tools. The Hub API provides endpoints for retrieving information and performing actions like model and dataset creation. | hugging face; hf; token |
| **Langchain API Key** | A framework for developing applications powered by large language models (LLMs). API keys are used to access the Langchain API and thus use LLMs. | langchain; lc; langchain_api_key |
| **Llama Cloud API Key** | Provides a suite of machine learning capabilities through its API, enabling users to integrate advanced AI functionalities into their applications. API keys are used to authenticate clients and manage access to the API resources securely. | llama; cloud; api; key; llama_cloud_api_key |
| **Mistral API Key** | Provides a way for developers to integrate Mistral's models into their applications and production workflows. | mistral; api; key; sdlsk |
| **OpenAI API Key** | An AI-based service that can be used to perform any task that involves understanding or generating natural language, code, or images. It provides a powerful API to interact with a wide variety of models. | token; openai_api_key; openai.api_key; oai; key |
| **Perplexity API Key** | A web search engine that uses a large language model to process queries and synthesize responses based on web search results. | perplexity; pplx; api; key |
| **Vercel API Access Token** | A platform for developers that provides tools, workflows, and infrastructure to build and deploy web apps faster with automatic scaling. | vercel; auth; bearer; token; turbo |
| **X AI API Key** | Allows integrating Grok in an application. The API Key is used to authenticate requests to the API. | x; xai; api; key; |

#### Amazon Credentials

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **AWS Secret Key v1** | An AWS credential for IAM users. These credentials are used by the AWS IAM SDK to sign API requests. The access key is the second component of this credential and often shares a credential or configuration file with the access key. | aws_secret_access_key; credentials; secret access key; secret key; set-awscredential |
| **AWS Access Key ID** |A component of an AWS access key, which is used for programmatic access to AWS services. It functions as a unique identifier for the user or role making a request to AWS APIs.|aws_secret_access_key; credentials; secret access key; secret key; set-awscredential|

#### Azure Credentials

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **Azure DevOps Personal Access Token** | Allows for a user to authenticate between a command line interface or integrated development environment and the Azure DevOps product. | azure; azure_token |
| **Azure Event Grid Access Key** | Enables a client application to authenticate with an Event Grid namespace topic, custom topic, partner namespaces, and domains. | eventgrid_topickey; aeg-sas-key; topickey |
| **Microsoft Office 365 OAuth Context** | Enables a client application to obtain authorized access to protected resources like web APIs.| microsoft; login; oauth; client_id; client_secret |

#### Communication Credentials

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **MailChimp API Key** | An email marketing service. It supports an API to integrate any application with the emailing services. | mailchimp; api key; bearer |
| **MailGun API Key** | Allows sending emails and performing other actions linked to the Mailgun account programmatically.  | mailgun; api key; bearer |
| **PagerDuty API Token**|A cloud company providing an incident response platform for IT departments.|pagerduty; token|
| **SendGrid API Key** | A communication platform for transactional and marketing emails. It offers a REST API to programmatically send emails and perform all sort of actions with a SendGrid account. | sendgrid; sg; server.password |
| **Slack App Token** | A business communication platform. It offers chat rooms in the form of channels organized by topics as well as private groups and direct messaging. | slack; xapp; my_slack_token; bearer |
| **Slack Configuration Token** | App configuration tokens are used to create and configure Slack apps using Slack API. | slack; access_token; xoxe |
| **Slack Signing Secrets** |A unique, confidential key used to verify the authenticity of incoming requests from Slack to your app, ensuring that requests are legitimate and haven't been tampered with. | slack; signing; slack_signing_secret; signing_secret |
| **Slack User Token** | An access token issued for a user within a Slack workspace, allowing an application to perform actions on behalf of that user. | slack; token; slack_token; xoxs; xoxp |
| **Slack Webhook Secret** | A single, long string containing a secret that should never be exposed online, as it allows unauthorized parties to post messages to your channel.  | slack; email_subscription_slack; hooks.slack |
| **Slack Bot Token** | Used to authenticate requests made by the bot to the Slack Web API, allowing the bot to interact with channels, users, and other Slack features on behalf of the app.| slack; slack_client; slack_token; xoxb |
| **Twilio API Key** | Used to authenticate with Twilio's REST APIs. | twilio; sk |
| **Twilio Master Credentials** | The primary authentication details for a Twilio account, granting full access to its resources and functionalities. | twilio; auth_token; account_sid |

#### Cryptographic Keys

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **DSA Private Key** | Can be used to authenticate to services. It works with a public and private key set.  | dsa;key;dsa_key |
| **EC Private Key** | The elliptic-curve cryptography (ECC) algorithm can be used to authenticate to services. | ec; key; ec_key |
| **Encrypted Private Key** | Keys that have been encrypted (usually with a passphrase). This can span DSA, EC, RSA, or any private key. | key; private_key |
| **General Private Key** | Can represent any type of cryptographic key standard. | key; private_key |
| **PGP Private Key** | Pretty Good Privacy (PGP) provides cryptographic privacy and authentication for data communication.  | Pgp; private_key  |
| **PlainText Private Key**| A private key that has not been encrypted and is stored or used in its original, readable form. | apikey; privatekey; api_key; private_key; key |
| **PuTTY Private Key** | A free Windows Telnet and SSH client. It provides a quick and simple way to connect to remote hosts. | key; private_key; privatekey; private-lines |
| **RSA Private Key v1** | Can be used to authenticate to services. It works with a public and private key set.  | key; private_key; beginrsaprivatekey; endrsaprivatekey; rsaprivatekey |

#### Financial Credentials

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **Bitcoin Crypto Wallet** | A crypto wallet stores and protects private keys. | bitcoin; crypto; wallet |
| **PayPal Braintree Access Token**|An authentication token used to securely access the Braintree API.|paypal; braintree; privatekey; publickey; client_id; client_secret|
| **Shopify App Secret** |A unique, confidential key assigned to a Shopify private or public app. | shopify; shopify_app_secret; shp; shpat; shppa |
| **Shopify App Key** | Also known as a client ID or API key, this is a public identifier for a Shopify app.  | shopify; shopifyappkey; shopifyappsecret; api_key; api_secret |
| **Square Token** | A legacy access token used to authenticate requests to the Square API. | square; sq0atp; bearer |
| **Square Token v2**| A modern, OAuth-based access token for the Square v2 API. | square; api; token; sandbox_accesstoken; api_token; eaaae |
| **Square Credentials** | A general term that can refer to various authentication tokens used to access the Square API.   | square; squareup; squareup_api_key; squareup_api_secret; sq0idp; sq0csp |
| **Stripe Keys** | A publishable key and a secret key. The publishable key is used on the client-side, the secret key is a highly sensitive credential used on the server-side to make API calls to Stripe.| stripe; stripe_secret_key; stripe_api_key; sk; sk_live |
| **Stripe Webhook Secret** | A unique, confidential key used to sign and verify webhook events sent from Stripe to your application. | stripe; stripe_wh_secret; whsec |

#### Google Credentials

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **Google API Key** | Used to authenticate requests to any Google API (such as Maps for instance).| key; gcp_key; gcp_api_key; googlemapskey; googlekey; x-goog-api-key |
| **Google Cloud API Key** | Provides resources to help clients process and store data on a cloud. | key; gcp_key; google_cloud_key; google-api-key; google-cloud-apikeys; googlekey; x-goog-api-key |
| **Google OAuth2 Key** | Allows users to build applications that can access information from a user's Google account after they authorized the application. | google; oauth; access; token; authorization; authentication |

#### SaaS Credentials

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **Box Links** | A direct URL to a file or folder located within Box. These links are also known as bookmarks within the Box web application. | box, box.com, url, link |
| **Checkout.com API Secret Key** | A payments platform for e-commerce and mobile applications.  | checkout, cko |
| **Databricks Personal Access Token** | Supports services to manage workspaces, DBFS, clusters, instance pools, jobs, libraries, users and groups, tokens, and MLflow experiments and models. | databricks; token; dapi |
| **Docker Swarm Join Token** | A secret key that allows a new node to join a Docker Swarm.  | swarm; token; swmtkn; join_token; join-token; join |
| **Docker Swarm Unlock Key** | A cryptographic key used to manually unlock a Docker Swarm manager node after its Docker daemon has been restarted. | swarm; token; swmkey; unlock_key; unlock-key; unlock |
| **Dropbox Links** | A direct URL to a file or folder located within Dropbox. | dropbox; dropbox.com; link; url |
| **Dynatrace Token** | Used to automate monitoring tasks and export different types of data into third-party reporting and analysis tools. | dynatrace; token; dynatrace_api_token; api-token |
| **Github Enterprise Token** | GitHub accounts can be controlled programmatically.  | github; access; token; githubaccesstoken; githubtoken |
| **GitLab Enterprise Token** | An open-source code hosting website that provides issue tracking, continuous integration and deployment pipeline. | gitlab; developer; pat |
| **Heroku Key** | A cloud provider, the platform key allows developers to authenticate on Heroku Platform in scripts and applications. | heroku; key; heroku_api_key |
| **JIRA API Token** | A ticket management platform. | token; jira_password; jira.myaccesstoken; jira |
| **Okta API Token** | An identity and access management company. | okta; token; okta_token |
| **Okta API Application Keys** | Applications that use Okta for authentication can be set and associated to a user account, they are attributed a client_id and a client_secret to authenticate communications between the applications and Okta. | okta; keys; okta_keys; client_id; client_secret |
| **PostgreSQL Connection Information**|An open-source relational database management system.|postgre; pgsql; psql; dbpassword; db_password|
|**Postman API Key**|A software that allows developers to build and test APIs.|postman; pmak; key|

#### Social Credentials

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **Facebook Access Token**| An opaque string that identifies a user, app, or Page and is used by an app to make Graph API calls. | facebook; access; token |
| **Facebook Secret** | A unique key assigned to a Facebook app.  | facebook; secret; token |
| **Twitter Access Token**| An OAuth token that allows an application to act on behalf of a user. | twitter; consumer; key; consumer_key; consumer_secret; twitter_consumer; social_auth |

#### Web Credentials

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **HTTP Basic Authentication Header** | For HTTP calls, basic authentication allows for a request to authenticate using a username and password, alongside the host. | user; username; password; pwd; pass; passwd; authorization; basic |
| **HTTP Cookie** | A request header that contains stored HTTP cookies associated with the server (that is, previously sent by the server). | cookie |
| **JSON Web Token** | A compact, URL-safe means of representing claims to be transferred between two parties. | jwt; token; decoded_token; password; secret |
| **Lightweight Directory Access Protocol (LDAP)** | A protocol used when accessing directory information services. | ldap_pass; ldap_pwd; pwd; pass; cred |
| **Microsoft Office 365 OAuth Context** | The OAuth 2.0 authorization code grant type, or auth code flow, enables a client application to obtain authorized access to protected resources like web APIs.| microsoft; login; oauth; client_id; client_secret |
| **SSH DDS Public** | A protocol for secure remote login, command-line and other secure network services over an insecure network. | sshpass; pass; pwd; passwd |

### Financial

Financial rules are organized into the following categories of rulesets:

- [United States](#united-states-financial)
- [Global](#global-financial)

#### Global Financial

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **American Express Card**  | 15 digit card numbers starting in 34 or 37. | amex; american express; cc; ccn; credit card; card; cardnumber |
| **Discover Card** | 16 digit card numbers starting in 6011, 644-649, or 65. | discover; ccn; cc; credit card; card; cardnumber |
| **Diners Card** |14 digit card numbers starting in 300-305, 36, or 38. | diners; cc; ccn; credit card; card; cardnumber |
| **IBAN** | An internationally agreed system of identifying bank accounts across national borders. | iban; bank; account; |
| **JCB Card**|16 digit card numbers starting in 3528–3589.|jcb; cc; ccn; credit card; card; cardnumber|
| **Maestro Card**|16 digit card numbers starts in 5018, 5020, 5038, 5893, 6304, 6759, 6761, 6762, 6763.|maestro; cc; ccn; credit card; card; cardnumber|
| **MasterCard** | 16 digit card numbers starting in 2221-2720 or 51-55. | mastercard; master card; cc; ccn; credit card; card; cardnumber |
| **Visa Card**  | 16 or 19 digit card numbers starting in 4. | visa; cc; ccn; credit card; card; cardnumber |

#### United States Financial

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **ABA Routing Number**| 9-digit numerical codes used to identify United States federal or state chartered banking institutions. | routing number; aba routing; aba |
| **American Express Card**  | 15 digit card numbers starting in 34 or 37. | amex; american express; cc; ccn; credit card; card; cardnumber |
| **Discover Card** | 16 digit card numbers starting in 6011, 644-649, or 65. | discover; ccn; cc; credit card; card; cardnumber |
| **MasterCard** | 16 digit card numbers starting in 2221-2720 or 51-55. | mastercard; master card; cc; ccn; credit card; card; cardnumber |
| **Visa Card**  | 16 or 19 digit card numbers starting in 4. | visa; cc; ccn; credit card; card; cardnumber |
| **US Bank Account** | Identify individual accounts with a bank or financial institution. Most account numbers range from 8-12 digits long but can contain up to 17 digits. | bank; account; us; acct |

### Health

Health rules are organized into the following categories of rulesets:

#### United States

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **HIPAA PHI National Drug Code** | A unique, 10 or 11-digit identifier for drugs in the United States. | ndc; drug_code; drug |
| **Medicare Beneficiary Identifier** | An 11-character, randomly generated identifier used to identify individuals enrolled in Medicare.   | medicare; mbi; id; beneficiary |
| **National Provider Identifier**| A unique 10-digit identification number assigned to health care providers by the Centers for Medicare & Medicaid Services (CMS). | npi; id; provider id |

### Network

Network rules are organized into the following categories of rulesets:

#### Global

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **IP Address** | Numerical labels used to assign devices to a network for internet protocol communication. | ip; address; |
| **Standard MAC Address** | A unique identifier assigned to a network interface controller (NIC) for use as a network address in communications within a network segment. | mac; addr |
| **URL**|A web address that uniquely identifies a specific resource on the internet.|url; link|

### Personally Identifying Information (PII)

PII rules are organized into the following categories of rulesets:

- [Asia Pacific](#asia-pacific)
- [Australia](#australia)
- [European Union](#european-union)
- [United Kingdom](#united-kingdom-uk)
- [United States/Global](#united-statesglobal)

#### Asia Pacific

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **Australian Business Number** | 11-digit unique identifier, given to all entities that are registered with the Australian Business Register. | abn; au_abn; australiabusinessnumber |
| **Australian Company Number** | 9-digit number issued by the Australian Securities and Investments Commission to every company registered under the Commonwealth Corporations Act 2001. | acn; au_acn; australiacompanynumber |
| **Australian Medicare Number** | An identifier issued by the Australian Government that enables the recipient to receive rebates on medical expenses through Australia's medicare system.  | medicare; au_medicare |
| **Australian Tax File Number** | A 9-digit number issued by the Australian Taxation Office for each tax entity including individuals, funds, companies, trusts, or partnerships. | tfn; au_tfn; taxfilenumber |
| **Indian Individual Identity Number** | A 12-digit unique number that is issued to each individual by Government of India. | aadhaar; uidai |
| **Indian Passport** | An 8-digit alphanumeric number identifying an individual Indian passport holder.| passport; indian passport; passport number |
| **Indian Permanent Account Number** | A 10-digit unique alphanumeric number issued by the Indian Income Tax Department to track tax and income related transactions for a given tax-associated entity. | pan; permanent account number |
| **Indian Vehicle Registration** | All motorized vehicles (and trailers) on public roads in India are tagged with a unique registration or license number. | rto; vehicle; plate; registration |
| **Indian Voter ID** | An identity document issued by the Election Commission of India to adult domiciles of India who have reached the age of 18.  | voter; votercard; voteridcard; epic; |
| **Singapore National Registration Identification Card**| A compulsory identity document issued to citizens and permanent residents of Singapore.| nric; ic; fin; national registration identity card; foreign identification card |
| **Singapore Unique Entity Number** | A standard identification number for all registered entities, similar to a company's tax ID number.| uen; unique entity number; business registration; acra |

#### European Union

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **Finnish Personal Identity Code** | Issued to a person who is registered in Finland’s Population Information System. | hetu; henkilötunnus; personbeteckningen; personal identity code |
| **France Social Security Number (INSEE/NIR)** | A 15-digit identifier used to manage social benefits and healthcare. | securite sociale; insee; nir; code secu; national identity code |
| **Italian Drivers License** | An identifier for individuals who have permission to operate vehicles of various kinds within Italy. | driver; drivers; license; driverlicense; driverslicense; dl |
| **Italian Identity Card** | A personal recognition document that is valid in Italy.  | id; idcard; carta; cie; documento |
| **Italian Passport** | A unique identifier for individuals who hold an Italian passport. | passport; passaporto; passaporto italiana; numero di passaporto |
| **Italian Personal Identification Code** | Also known as a fiscal code, this is a 16-character unique identifying tax code for an entity. | codice; fiscale; tax; id; personal; code; tin |
| **Italian VAT Code** | A 13 character alphanumeric identifier used in Italy, for value-added tax purposes. | iva; vat; vat number |
| **Polish PESEL Number** | The national identification number in Poland. | pesel; numer identyfikacyjny; dowod osobisty; nr-pesel |
| **Spanish Foreigners ID Card** | A personal, unique and exclusive number that is assigned to foreigners who, for economic, professional, or social reasons, are engaged in activities related to Spain and require identification in this country. | nie; numero extranjero |
| **Spanish Personal Tax ID** | Necessary for dealing with the Tax Agency when the interested party does not have a Spanish National ID (DNI) or Foreigner Identification Number (NIE). | nif; documento nacional de identidad |
| **Spanish National ID** | The document that accredits the identity of Spaniards. | dni; documento nacional de identidad |

#### United Kingdom (UK)

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **UK Drivers License Numbers Great Britain** | The official document which authorises its holder to operate motor vehicles on highways and other public roads. | driver; drivers; license; driverlicense; driverslicense; dl |
| **UK Drivers License Numbers Northern Ireland** | The official document which authorises its holder to operate motor vehicles on highways and other public roads. | driver; drivers; license; driverlicense; driverslicense; dl |
| **UK National Health Service Number** | Helps healthcare staff and service providers identify patients correctly and match personal details to health records. | nhs; national health service number; patient; id |
| **UK National Insurance Number** | Ensures that National Insurance contributions and tax are recorded against an indivdual's name only. | ni; nino; national insurance number |
| **UK Passport Number** | Forms of documentation for citizens and nationals to be used for international identification recognition. | uk; passport |

#### United States/Global

| Rule | Description | Anchor terms |
| :---- | :---- | :---- |
| **US Social Security Number** | Nine-digit numerical codes issued to United States citizens, permanent residents, and working residents to identify individuals for tax, employment, and social benefit purposes. | ssn; social; socialsecurity; socialsecuritynumber |
| **US Passport** | Forms of documentation for citizens and nationals to be used for international identification recognition. | us; passport; |
| **US Vehicle Identification Number** | 17-digit unique identifiers used by manufacturers, insurance companies, and law enforcement to identify and track motor vehicles. | vin; vehicle identification number |
| **US Individual Tax Identification Number** | 9-digit unique identifiers used for federal tax purposes in the event an individual taxpayer is not eligible for a social security number.  | itin; tax identification number |
| **Email** | Delivers messages over computer networks between one or many address destinations. | email; em; mail; mailto |
| **Phone** | A unique digit sequence assigned to a phone line for the purpose of making and receiving telephone calls. Each phone number is composed of a country code, area code, and a local number. | phone; ph |
