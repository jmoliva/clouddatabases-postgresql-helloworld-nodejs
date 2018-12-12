# clouddatabases-postgresql-helloworld-nodejs overview

clouddatabases-postgresql-helloworld-nodejs is a sample IBM Cloud application which shows you how to connect to an IBM Cloud Databases for PostgreSQL service to a IBM Cloud Foundry application written in Node.js.

## Running the app on IBM Cloud

1. If you do not already have an IBM Cloud account, [sign up here][IBMCloud_signup_url]

2. [Download and install IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

   The IBM Cloud CLI tool tool is what you'll use to communicate with IBM Cloud from your terminal or command line.

3. Connect to IBM Cloud in the command line tool and follow the prompts to log in.

    ```shell
    ibmcloud login
    ```

    **Note:** If you have a federated user ID, use the `ibmcloud login --sso` command to log in with your single sign on ID.

4. Create your database service.

      The database can be created from the command line using the `ibmcloud resource service-instance-create` command. This takes a
      service instance name, a service name, plan name and location. For example, if we wished to create a database service named "example-psql" and we wanted it to be a "databases-for-postgresql" deployment on the standard plan running in the us-south region, the command would look like this:

      ```shell
      ibmcloud resource service-instance-create example-psql databases-for-postgresql standard us-south
      ```
      Remember the database service instance name.

5. Make sure you are targeting the correct IBM Cloud Cloud Foundry org and space.

   ```shell
   ibmcloud target --cf
   ```
   
   Choose from the options provided.

6. Create a Cloud Foundry alias for the database service.
   
   ```shell
   ibmcloud resource service-alias-create alias-name --instance-name instance-name
   ```

   The alias name can be the same as the database service instance name. So, for our database created in step 4, we could do:

   ```shell
   ibmcloud resource service-alias-create example-psql --instance-name example-psql
   ```

7. Clone the app to your local environment from your terminal using the following command:

   ```shell
   git clone https://github.com/IBM-Cloud/clouddatabases-postgresql-helloworld-nodejs.git
   ```

8. `cd` into this newly created directory. The code for connecting to the service, and reading from and updating the database can be found in `server.js`. See [Code Structure](#code-structure) and the code comments for information on the app's functions. There's also a `public` directory, which contains the html, style sheets and javascript for the web app. For now, the only file you need to update is the application manifest.

9. Update the `manifest.yml` file.

   - Change the `name` value. The value you choose will be the name of the app as it appears in your IBM Cloud dashboard.
   - Change the `route` value to something unique. This will make be the base URL of your application. It should end with your selected IBM Cloud region url (ex in Frankort here) `.eu-de.mybluemix.net`. For example `example-helloworld-nodejs.eu-de.mybluemix.net`.

   Update the `service` value in `manifest.yml` to match the name of your database service instance name.

10. Push the app to IBM Cloud. When you push the app it will automatically be bound to the service.

  ```shell
  ibmcloud cf push
  ```

Your application is now running at host you entered as the value for the `route` in `manifest.yml`.

The node-postgresql-helloworld app displays the contents of an _examples_ database. To demonstrate that the app is connected to your service, add some words to the database. The words are displayed as you add them, with the most recently added words displayed first.

## Code Structure

| File | Description |
| ---- | ----------- |
|[**server.js**](server.js)|Establishes a connection to the PostgreSQL database using credentials from VCAP_ENV and handles create and read operations on the database. |
|[**main.js**](public/javascripts/main.js)|Handles user input for a PUT command and parses the results of a GET command to output the contents of the PostgreSQL database.|

The app uses a PUT and a GET operation:

- PUT
  - takes user input from [main.js](public/javascript/main.js)
  - uses the `client.query` method to add the user input to the words table

- GET
  - uses `client.query` method to retrieve the contents of the _words_ table
  - returns the response of the database command to [main.js](public/javascript/main.js)



[databases_for_postgreSQL_url]: https://console.bluemix.net/catalog/services/databases-for-postgreSQL/
[IBMCloud_signup_url]: https://console.bluemix.net/registration/?cm_mmc=Display-SampleApp-_-IBMCloudSampleApp-DatabasesForPostgreSQL

