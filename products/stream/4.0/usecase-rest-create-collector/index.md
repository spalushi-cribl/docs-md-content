# Creating a Custom Collector


Cribl's flagship product introduced [Collectors](collectors) in Logstream 2.2, and they have since evolved to become a critical part of the platform. You can think of Collectors as jobs that retrieve data from an external service. You can also schedule them to run periodically, like a Linux cron job.

We built the Collection framework to be extensible like Cribl Stream [Functions](functions). This means that you can create new Collectors on the fly. This page covers the Collection process, schema files, and their implementation – to give you some context – before walking you though the steps of creating your own Collector. 

## Collection Process 

At its simplest, collection is a two-step process: Discover, and Collect.

### Discover 

This step identifies the items to collect. The Discover call generates a list of items, and Collect tasks will be created for those items. Note that:
- The Discover call can return zero to many items. The Collection phase will run only if Discover returned at least one item.
- Your Collector's purpose (and definition) determines the actual data that Discover will return. 
- If the Collector pulls data from files on disk, the Discover call lists the files in the directory to determine how many items match the criteria (matching by filter and/or date range). Assuming that there are 100 files to collect from, the Discover call will return 100 items (each specifying a file path and size).

### Collect 
        
Each item, from the Discover phase's returned list of items, will trigger one Collect task for Cribl Stream to create and run. More on Collect: 

- Generally, there is a single Collect task per item returned by Discover. 

- In [distributed deployments](collectors#collectors-in-distributed-deployments), you configure Collectors at the Worker Group level, and Worker Nodes execute the tasks. However, the Leader Node oversees the task distribution, and tries to maintain a fair balance across jobs.

## Collector Schema Files {#schema}

The Collector schema files, `conf.schema.json` and `conf.ui-schema.json`, describe the structure of the UI for configuring a new or existing Collector. As an example, here's a Script Collector's Configuration modal:  

![Script Collector Configuration Screen](st-usecase-create-collector.a815caa86f.png)
{border="true"}

The `conf.schema.json` below defines all the fields displayed on that **Collector Settings** form:

```json
{
  "type": "object",
  "title": "",
  "required": ["discoverScript", "collectScript"],
  "properties": {
    "discoverScript": {
      "type": "string",
      "title": "Discover Script",
      "minLength": 1,
      "description": "Script to discover what to collect. Should output one task per line in stdout."
    },
    "collectScript": {
      "type": "string",
      "title": "Collect Script",
      "minLength": 1,
      "description": "Script to run to perform data collections. Task passed in as $CRIBL_COLLECT_ARG. Should output results to stdout."
    },
    "shell": {
      "type":"string",
      "title": "Shell",
      "description": "Shell to use to execute scripts.",
      "default": "/bin/bash"
    }
  }
}
```

The `conf-ui-schema.json` file further refines the field specifications in `conf.schema.json`. In the example below, it specifies that the `discoverScript` and `collectScript` fields should use the `TextareaUpload` widget with 5 rows. This file also gives each text field some placeholder (ghost) text, to display when no data is present in the field.

```json
{
  "discoverScript": {
    "ui:widget": "TextareaUpload",
    "ui:options": {
      "rows": 5
    },
    "ui:placeholder": "Discover script"
  },
  "collectScript": {
    "ui:widget": "TextareaUpload",
    "ui:options": {
      "rows": 5
    },
    "ui:placeholder": "Collect script"
  }
```

Schemas can be simple, like the Script Collector, or complex like the [REST Collector](collectors-rest). This guide doesn't cover all the possibilities of working with schemas, which do get complex. However, you can check out other existing Collectors as examples of how to apply schemas for your own use case. 

## Collector Implementation

The Collector's implementation logic is part of `index.js`, and resides in the same directory as the Collector schema files. You must define the following attributes and methods for the Collector:

```js
// Jobs can import anything from the C object, to see what's available use the
// Stream UI and an Eval Function to discover options.
const { Expression, PartialEvalRewrite } = C.expr;
const { httpSearch, isHttp200, RestVerb, HttpError, wrapExpr, DEFAULT_TIMEOUT_SECS } = C.internal.HttpUtils;

exports.name = 'Weather';
exports.version = '0.1';
exports.disabled = false; // true to disable the collector
exports.destroyable = false;
exports.hidden = false; // true to hide collector in the UI

// Define Collector instance variables here: i.e.:
// let myVar; // Initialize in init, discover, or collect.

// init is called before the collection job starts. Gives the Collector a chance
// to validate configs and initialize internal state.
exports.init = async (opts) => {
  // validate configs, throw Error if a problem is found.
  // Initialize internal attributes here
}

// The Discover task's main job is to determine 'what' to collect. Each item
// to collect (i.e. a file, an API call, etc) is reported via the job object
// and will execute a collection task.
// Note that different instances of the Collector will be used for the Discover
// and Collect operations. Do not set internal Collector state in Discover 
// and expect it to be present in Collect. The _init method will be called
// prior to Discover orCcollect.
exports.discover = async (job) => {
  // Job is an object that the collector can interact with, for example
  // we can use the job to access a logger.
  job.logger().info('Discover called');
  
  // In this case, reporting 2 hard coded items to be collected. Normally,
  // collectors dynamically report items to collect based on input from 
  // an API or library call.
  await job.addResults([{"city": "San Francisco"},{"city": "Denver"}]);
  // Can also add results one at a time using await job.addResult("city": "San Francisco"})
}

// One invocation of the Collect method is made for each item reported by the 
// discover method. The collectible object contains the data reported by discover.
// In our example collect will be called twice, once for each item returned 
// by discover's addResults call: 
// Invocation 1: {"city": "San Francisco"}
// Invocation 2: {"city": "Denver"}
exports.collect = async (collectible, job) => {
   job.logger().info('In collect', { collectible });
   try {
     // Do actual data collection here. In this case we might make a REST API call
     // to retrieve current weather conditions for the city in collectible.
     const myReadableStream = doGetWeather(collectible.city);
   } catch (error) {
     // If the collector encounters a fatal error, pass the error to the job. This
     // will make the error visible in the Job inspector UI.
     job.reportError(error)
     return;
   }
   // Return result of the collect operation should be Promise<Readable>
   // which will be piped to routes or to the configured pipeline and destination.
   return Promise.resolve(myReadableStream)
}
```

## Set Up a New Collector

Now to the exciting part. Here's an overview of how to add a Collector:

1.  Create a directory where Collector files will reside. All files associated with the Collector should be in this directory.
    
2.  Create the schema files and add them to the directory. The following [schema files](#schema) describe the structure of the UI for configuring a new or existing Collector:  
    -   `schema.json`
    -   `ui-schema.json`
        
3.  Create a JavaScript file named `index.js`, with naming attributes and required methods.
    
4.  Test and validate the Collector.
    
5.  Install the Collector in Cribl Stream.

### Before You Start 

A few things to note before we start the process of adding a custom Collector:

1. For a standalone install of a running Cribl Stream instance, add new directories and files to the `$CRIBL_HOME/local/cribl/collectors` directory (e.g., `/opt/cribl`). 
    
2.  The Collector will show up in the UI only if all schema files and the `index.js` file successfully compile. If the Collector is not showing up in the UI, check whether there was a problem compiling one of the files. You can check the errors through the [API Server Logs](/stream/monitoring#types).

3. Develop your Collector in a local test environment, as a best practice. After making changes to the Collector, you might need to restart/and or deploy your system.

4.  Collectors can access all Node.js built-in modules, using the `require` directive to import each module.
    
5.  You can include third-party Node.js modules into a custom Collector by installing them in the Collector’s home directory.
    
6.  Cribl Stream ships with out-of-the-box Collectors which reside in the `$CRIBL_HOME/default/cribl/collectors` directory. Feel free to copy contents from one of the existing Collectors to your `$CRIBL_HOME/local/cribl/collectors` (local directory) as a starting point/example of how to build more-complex schemas.
    
> Never modify a Collector in the default directory (`$CRIBL_HOME/default/cribl/collectors/*`). Because:
> 1. Doing so changes the behavior of a Collector in your installed system. 
> 2. Contents of the default directory will be overwritten during upgrades.
> 
> When creating your own Collector, always make a copy to a directory in the local `$CRIBL_HOME/local/cribl/collector/<yourCollector>`
>
{.box .warning}

### Sample Collector Requirements

For this example, our sample Collector's goal is to generate events – containing a random quote obtained from an internal list – for a list of users. Here is a breakdown of this Collector's requirements:

1.  Provide a random quote to a predefined or random list of usernames.
    
2.  Accept the names of users for whom to generate quotes.
    
3.  Accept randomly generated usernames, by specifying the number of usernames to generate.
    
4.  Generate a single event containing a random quote for each user.
    
5.  Pick the random quotes from a hard-coded list, or optionally, use a REST API instead.

For this sample Collector, we'll reference a [third-party Node module](https://www.npmjs.com/package/username-generator) to generate random usernames, and to give you a basic understanding of how to reference external code packages.

### Set Up Your Environment {#environment}

For this guide, we'll build a custom Collector in a standalone Cribl Stream install running in Docker. To adapt the steps for a distributed environment: 

1. Build the Collector on the Leader Node.

2. The Collector directory will reside in the filesystem at: `$CRIBL_HOME/groups/<workerGroup>/local/cribl/collectors/quote_generator`. For example, to build in the default Worker Group, work in the directory:
`$CRIBL_HOME/groups/default/local/cribl/collectors/quote_generator`.

3. Before running the Collector, commit and deploy changes for the parent Worker Group.

### Deploy a Single Instance of Cribl Stream in Docker 

1.  Start the Docker instance by running the following command:  
    `docker run -d -p 19000:9000 cribl/cribl:latest`
    
2.  List Docker containers by running `docker ps`, as shown here:
    
    ```shell
    $ docker ps
    CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS         PORTS                     NAMES
    544370698fb5   cribl/cribl:3.4.1-RC1   "/sbin/entrypoint.sh…"   4 minutes ago   Up 4 minutes   0.0.0.0:19000->9000/tcp   nervous_wozniak
    ```
    
3.  Access the Cribl Stream UI at port: `http://localhost:19000`
    
4.  Connect to the container:  
    `docker exec -it <Container ID> bash` 
    
    (e.g., `docker exec -it 544370698fb5 bash`)
    
5.  Update the apt installer:  
    `apt update`
    
6.  Install Vim (or an editor of your choice):  
    - vim: `apt install vim`  
    - nano: `apt install nano`
    
After this, remain connected to the Docker container, and follow the steps below to create the Collector.

> Remember to leave the Docker container running while you work on the Collector. Also, remember to explicitly back up your files before stopping the Docker container.
>
{.box .success}

### Build a Collector (Single-Instance Environment)

In your terminal, type the following commands:

1.  `cd /opt/cribl/local/cribl`
    
2.  `mkdir collectors`
    
3.  `cd collectors`
    
4.  `mkdir quote_generator`
    
5.  `cd quote_generator`
    
6.  `cp ../../../../default/cribl/collectors/script/* .`
    
7.  `chmod +w index.js *.json`
    
8.  Edit `index.js` to change the following, to assign a unique name to the Collector:
    
    ```js
    exports.name = 'Quote Generator';
    ```
    
9.  Next, restart Cribl Stream with the following command:  

    `$ /opt/cribl/bin/cribl restart`
    
10.  After Cribl Stream restarts, you must log out and log back in for the new Collector tile to display under [Sources](sources).  

## Configure the Collector in Cribl Stream {#collector}

1. From your [Cribl Stream UI](#environment)'s top nav, select **Data > Sources**, then select **Collectors > Quote Generator** from the **Data Sources** page's tiles or the **Sources** left nav.

    ![Quote Generator Collector (Missing an Icon)](st-usecase-create-collector2.c0f2889ae8.png)
    {border="true"}

2. Click **New Collector** to open the **Quote Generator > New Collector** modal.

3. Enter the following in the **Collector Settings** tab, then click **Save**:

   - **Collector ID**: `Hello_World`
   - **Discover Script**: `echo "hello world"`
   - **Collect Script**: `echo "{ message: \"$CRIBL_COLLECT_ARG\" }"`

    ![Collector Settings Tab](st-usecase-create-collector3.7ffbdf27fd.png)
    {border="true"}

4. On the **Manage Quote Generator Collectors** page, click **Run** next to the Collector. 

    ![Run the Collector](st-usecase-create-collector4.fdd645eaee.png)
    {border="true"}

5. In the **Run configuration** modal, click **Run** again. Note that you can set the Collector's debug level in the modal's **Advanced Options** section. For details, see [Run Configurations and Shared Settings](/stream/collectors-schedule-run#run-config).

    ![Run the Collector](st-usecase-create-collector5.6d1daef271.png)
    {border="true"}

6. After the Collector runs, the following event will be generated:

    ![Run the Collector](st-usecase-create-collector6.3249ca368c.png)
    {border="true"}

    Now that we have a basic shell for our Collector, let's change the UI and `index.js` to add our custom Collector logic. Before making any additional changes, delete the Collector we just created. 

7. On the **Manage Quote Generator Collectors** page, click the check box next to your Collector, then click **Delete selected Collectors**.

    ![Delete the Collector](st-usecase-create-collector7.f15efe4c68.png)
    {border="true"}

### Edit the Schema Files

1. Replace the contents of `conf.schema.json` with the following: 

    ```json
    {
      "type": "object",
      "title": "",
      "properties": {
        "autoGenerateNames": {
          "type": "boolean",
          "title": "Auto generate names",
          "description": "Turn on to autogenerate names, use num names to specify how many names to generate. Turn off to specify a static list of names",
          "default": false
        }
      },
      "dependencies": {
        "autoGenerateNames": {
          "oneOf": [
            {
              "required": ["numNames"],
              "properties": {
                "autoGenerateNames": { "enum": [true] },
                "numNames": {
                  "type": "number",
                  "title": "Num names",
                  "minimum": 1,
                  "maximum": 1000,
                  "description": "The number of names to auto generate, each name will turn into a collection task."
                }
              }
            },
            {
              "required": ["names"],
              "properties": {
                "autoGenerateNames": { "enum": [false] },
                "names": {
                  "type": "array",
                  "title": "Names",
                  "minLength": 1,
                  "description": "List of user names to retrieve quotes for.",
                  "items": {"type": "string"}
                }
              }
            }
          ]
        }
      }
    }
    ```

    Two interesting things to note in the `conf.schema.json` file:
    - When you disable – set to `false`– the boolean toggle `autoGenerateNames`, the UI displays a **Names** field requesting a list of user names for which to retrieve quotes.
    - When you enable  – set to `true`–  the boolean toggle `autoGenerateNames`, the UI displays a **Number of names to enter** field requesting the number of names to auto-generate. 

    The dependency allows dynamic onscreen behavior based on the field's value.

2. Replace the contents of `conf.ui-schema.json` with the following:
    
    ```json
    {
      "names": {
        "ui:field": "Tags",
        "ui:placeholder": "Enter names",
        "ui:options": {
          "separator": ","
        } 
      }   
    }     
    ```
    
    The UI schema, in this case, further refines the widget (“Tags”) used for the Names field. We will see this in action a bit later.

3. Finally, replace the contents of `index.js` with the following:
    
    ```js
    exports.name = 'Quote Generator';
    exports.version = '0.1';
    exports.disabled = false;
    exports.destroyable = false;
    
    const Readable = require('stream').Readable;
    const os = require('os');
    const host = os.hostname();
    const logger = C.util.getLogger('myCollector'); // For use in debugging init, search worker logs for channel 'myCollector'
    
    let conf;
    let batchSize;
    let filter;
    let autoGenerate = false;
    let numNames = 0;
    let names;
    
    exports.init = async (opts) => {
      conf = opts.conf;
      //logger.info('INIT conf', { conf });
      batchSize = conf.maxBatchSize || 10;
      filter = conf.filter || 'true';
      autoGenerate = conf.autoGenerateNames
      names = conf.names || [];
      numNames = conf.numNames ?? 3;
      //logger.info('INIT VALUES', { autoGenerate, names, numNames });
      return Promise.resolve();
    };
    
    exports.discover = async (job) => {
      for (let i = 0; i < names.length; i++) {
        job.addResult({"name": names[i]});
      }
    }
    
    exports.collect = async (collectible, job) => {
      job.logger().debug('Enter collect', { collectible });
      const quote = "Carpe Diem!"; // Hard coded quote for now.
    
      // Must return a readable stream from the collect method. Here we are returning
      // the result string wrapped in a Readable.
      const s = new Readable();
      s.push(JSON.stringify({ host, name: collectible.name, quote }));
      s.push(null);
      
      // Return readable to stream collected data
      return Promise.resolve(s)
    };
    ```
4. Next, restart Cribl Stream with the following command:  

    `$ /opt/cribl/bin/cribl restart`
    
5. After Cribl Stream restarts, you must log out and log back in for the new Collector tile to display under [Sources](sources).

### Configure a New Collector Instance 

1. In the Cribl Stream UI, navigate to the [Quote Generator Collector](#collectors) and click **+ Add New** to open the **Quote Generator > New Collector** modal.

2. Type the following into the **Collector Settings** tab, then click **Save**:

   - **Collector ID**: `firstCollector`
   - **Auto generate names**: `No`
   -  **Names**: `Jane, John, Rover`

    ![New Collector Settings](st-usecase-create-collector8.6bb13133a5.png)
    {border="true"}

3. On the **Manage Quote Generator Collectors** page click **Run** next to the Collector. 

    ![Run the Collector](st-usecase-create-collector9.f26e940cb7.png)
    {border="true"}

4. In the **Run configuration** modal:

   - For **Mode**, click **Preview**.
   - For **Log Level**, select **debug**.
   -  Click **Run** again.

    ![Run Collector Settings](st-usecase-create-collector10.09e77a8517.png)
    {border="true"}

4. If the Collector works, a successful run displays a results dialog, including an event for each name that was added. 

    ![Collector Events](st-usecase-create-collector11.664034b659.png)
    {border="true"}

5. Close the **Preview** dialog.

6. On the **Manage Quote Generator Collectors** page, click **Latest ad hoc run**.

    ![Latest Ad Hoc Run column](st-usecase-create-collector12.b4c3984b19.png)
    {border="true"}

7. This will open a dialog from the [Job Inspector](/stream/monitoring#job-inspector) with information about the run, including job status, items returned by the Discover call, and logs. Click the **Discover Results** tab to see data returned from the Collector’s Discover call:

    ![Discover Results](st-usecase-create-collector13.96a684003e.png)
    {border="true"}

    The Collector method separately. invokes each item returned The Collector argument is the content from each row in the table.

8. Click the **Logs** tab to view logs from the run. This is where you can locate anything logged by `job.logger`.

    ![View Logs](st-usecase-create-collector14.3dd6453311.png)
    {border="true"}

The exceptions are anything logged by our Collector in `init`. Notice that `index.js` defined another logger to use when debugging `init`:

`const logger = C.util.getLogger('myCollector');`

You can view the relevant Worker Process' logs on the [Monitoring](/stream/monitoring#types) page. 

### Integrate a Third-Party Package

Next, we'll integrate a third-party package to randomly generate names in the Discover method. 

1. In your terminal, follow these steps to install npm (Node Package Manager) into the virtual machine running Cribl Stream:

   - `apt update`
   - `apt install npm`
   - When prompted, select a `Region`.
   - When prompted, select a `Timezone`.

2. Run the following commands in your Collector directory: 

   -  cd `/opt/cribl/local/cribl/collectors/quote_generator`    
   - `npm init`: Answer all questions with default answers. The values are not important for our purposes.
   - `npm install username-generator --save`  
        
   This will install the third-party [username-generator](https://www.npmjs.com/package/username-generator) package in `/opt/cribl/local/cribl/collectors/quote_generator/node_modules`.
        
3.  Next, update the Collector's `index.js` with the following content, to use this new package to auto-generate names:
    
    ```js
    exports.name = 'Quote Generator';
    exports.version = '0.1';
    exports.disabled = false;
    exports.destroyable = false;
    
    const UsernameGenerator = require('username-generator');
    const Readable = require('stream').Readable;
    const os = require('os');
    const host = os.hostname();
    const logger = C.util.getLogger('myCollector'); // For use in debugging init, search worker logs for channel 'myCollector'
    
    let conf;
    let batchSize;
    let filter;
    let autoGenerate = false;
    let numNames = 0;
    let names;
    
    exports.init = async (opts) => {
      conf = opts.conf;
      //logger.info('INIT conf', { conf });
      batchSize = conf.maxBatchSize || 10;
      filter = conf.filter || 'true';
      autoGenerate = conf.autoGenerateNames
      names = conf.names || [];
      numNames = conf.numNames ?? 3;
      //logger.info('INIT VALUES', { autoGenerate, names, numNames });
      return Promise.resolve();
    };
    
    exports.discover = async (job) => {
      if (autoGenerate) {
        // Auto generate usernames using 3rd party package.
        names = [];
        for (let i = 0; i < numNames; i++) {
          names.push(UsernameGenerator.generateUsername('_'));
        }
        job.logger().info('Successfully generated usernames', { numGenerated: names.length });
      }
      for (let i = 0; i < names.length; i++) {
        job.addResult({"name": names[i]});
      }
    }
    
    exports.collect = async (collectible, job) => {
      job.logger().info('Enter collect', { collectible });
      const quote = "Carpe Diem!"; // Hard coded quote for now.
    
      // Must return a readable stream from the collect method. Here we are returning
      // the result string wrapped in a Readable.
      const s = new Readable();
      s.push(JSON.stringify({ host, name: collectible.name, quote }));
      s.push(null);
    
      // Return readable to stream collected data
      return Promise.resolve(s)
    };
    ```

4. Now, in Cribl Stream's UI, update the existing Collector to use the auto-generate feature. Add these settings, then click **Save**:   

    - **Auto generate names**: `Yes`     
    -  **Num names:** `10`

    ![Update Existing Collector](st-usecase-create-collector15.c112fbb473.png)
    {border="true"}   

5. Run the Collector, notice that 10 Events are now displayed and each username is randomized:

     ![Update Existing Collector](st-usecase-create-collector16.c645dd1426.png)
     {border="true"}  

 6. Next, update the Collector's `index.js` with the following content to create a static list of random quotes in the Collect method. You can optionally retrieve a random quote from a REST API instead.
    
    ```js
    exports.name = 'Quote Generator';
    exports.version = '0.1';
    exports.disabled = false;
    exports.destroyable = false;
    
    const UsernameGenerator = require('username-generator');
    const Readable = require('stream').Readable;
    const os = require('os');
    const host = os.hostname();
    const { httpSearch, isHttp200, RestVerb } = C.internal.HttpUtils;
    
    const logger = C.util.getLogger('myCollector'); // For use in debugging init, search worker logs for channel 'myCollector'
    
    // Quotes to randomize
    const quotes = [
      "Spread love everywhere you go. Let no one ever come to you without leaving happier. -Mother Teresa",
      "When you reach the end of your rope, tie a knot in it and hang on. -Franklin D. Roosevelt",
      "Always remember that you are absolutely unique. Just like everyone else. -Margaret Mead",
      "Don't judge each day by the harvest you reap but by the seeds that you plant. -Robert Louis Stevenson",
      "The future belongs to those who believe in the beauty of their dreams. -Eleanor Roosevelt",
      "Tell me and I forget. Teach me and I remember. Involve me and I learn. -Benjamin Franklin",
      "The best and most beautiful things in the world cannot be seen or even touched - they must be felt with the heart. -Helen Keller",
      "It is during our darkest moments that we must focus to see the light. -Aristotle",
      "Whoever is happy will make others happy too. -Anne Frank",
      "Do not go where the path may lead, go instead where there is no path and leave a trail. -Ralph Waldo Emerson",
      "You will face many defeats in life, but never let yourself be defeated. -Maya Angelou",
      "The greatest glory in living lies not in never falling, but in rising every time we fall. -Nelson Mandela",
      "In the end, it's not the years in your life that count. It's the life in your years. -Abraham Lincoln",
      "Never let the fear of striking out keep you from playing the game. -Babe Ruth",
      "Life is either a daring adventure or nothing at all. -Helen Keller",
      "Many of life's failures are people who did not realize how close they were to success when they gave up. -Thomas A. Edison",
      "You have brains in your head. You have feet in your shoes. You can steer yourself any direction you choose. -Dr. Seuss",
      "If life were predictable it would cease to be life and be without flavor. -Eleanor Roosevelt",
      "In the end, it's not the years in your life that count. It's the life in your years. -Abraham Lincoln",
      "Life is a succession of lessons which must be lived to be understood. -Ralph Waldo Emerson",
      "You will face many defeats in life, but never let yourself be defeated. -Maya Angelou",
    ]
    
    let conf;
    let batchSize;
    let filter;
    let autoGenerate = false;
    let numNames = 0;
    let names;
    
    exports.init = async (opts) => {
      conf = opts.conf;
      //logger.info('INIT conf', { conf });
      batchSize = conf.maxBatchSize || 10;
      filter = conf.filter || 'true';
      autoGenerate = conf.autoGenerateNames
      names = conf.names || [];
      numNames = conf.numNames ?? 3;
      //logger.info('INIT VALUES', { autoGenerate, names, numNames });
      return Promise.resolve();
    };
    
    exports.discover = async (job) => {
      if (autoGenerate) {
        // Auto generate usernames using 3rd party package.
        names = [];
        for (let i = 0; i < numNames; i++) {
          names.push(UsernameGenerator.generateUsername('_'));
        }
        job.logger().info('Successfully generated usernames', { numGenerated: names.length });
      }
      for (let i = 0; i < names.length; i++) {
        await job.addResult({"name": names[i]});
      }
    }
    
    exports.collect = async (collectible, job) => {
      const quote = quotes[Math.trunc(Math.random()*100)%quotes.length];
      // Must return a readable stream from the collect method. Here we are returning
      // the result string wrapped in a Readable.
      const result = { host, name: collectible.name, quote };
      job.logger().info('collect returning quote', { collectible, quote });
      const s = new Readable();
      s.push(JSON.stringify(result));
      s.push(null);
    
      // Return readable to stream collected data
      return Promise.resolve(s)
    };
    
    ```

7.  Run the Collector to randomly retrieve the quotes from the list:

    ![Run the Collector](st-usecase-create-collector17.ec542d1ba2.png)
    {border="true"}  

8. In the next few steps, you'll back up the files, by opening a shell window on your local machine and running the indicated commands. 
   
9. List Docker containers by running `docker ps`, as shown here:
      
    ```shell
    $ docker ps
    CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS         PORTS                     NAMES
    208db8d1d8f7   cribl/cribl:latest   "/sbin/entrypoint.sh…"   6 hours ago   Up 6 hours   0.0.0.0:19000->9000/tcp   serene_galileo
    ```

    In this example, the **Container ID** is 208db8d1d8f7. You'd paste this into the next commands.

10. Run: `docker exec -it 208db8d1d8f7 tar cvzf /tmp/container_source.tgz /opt/cribl/local/cribl/collectors/quote_generator`

11. Run: `docker cp 208db8d1d8f7:/tmp/container_source.tgz`  

 You can locate the archive file containing the source code in the current directory. After you back up the source code, it is safe to shut down the Docker container. 

### Building a Collector (Distributed Environment)

When you create a Collector in a distributed environment, the directory path is slightly different from the single–instance example above. E.g., for the `default group`, you should create Collectors on the Leader Node in the directory: `opt/cribl/groups/default/local/cribl/collectors`.

The format of the path is: `/opt/cribl/groups/<workerGroup>/local/cribl/collectors`

For example, assuming you want to create a new Collector in group is `myGroup`, the path would be:  
`/opt/cribl/groups/myGroup/local/cribl/collectors`.

You'd proceed as follows:

1.  Connect to the Leader Node.
    
2.  `cd $CRIBL_INSTALL/groups/default/local/cribl`  

    `$CRIBL_INSTALL` refers to the directory where Stream is running, e.g.: `/opt/cribl`
    
3.  `mkdir collectors`
    
4.  `cd collectors`
    
5.  `mkdir quote_generator`
    
6.  `cd quote_generator`
    
7.  `cp ../../../../default/cribl/collectors/script/* .`

Building the Collector in a distributed environment works the same as in single-instance environment with exception of a few differences:

1.  You must add the files to a Worker–Group directory, as described above.
    
2.  You must commit and deploy changes to the Workers. 
    
Given the extra steps, we recommend that you first build the Collector in a standalone environment, before deploying it to a distributed environment.
