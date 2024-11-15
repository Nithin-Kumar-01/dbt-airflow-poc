Use DBT profile instead of Airflow connections
==============================================


If you don’t want to use Airflow connections, or if there’s no readily-available profile mapping for your database, you can use your own dbt profiles.yml file. To do so, you’ll need to pass the path to your profiles.yml file to the profiles_yml_filepath argument in ProfileConfig.

For example, the code snippet below points Cosmos at a profiles.yml file and instructs Cosmos to use the my_snowflake_profile profile and dev target:

from cosmos.config import ProfileConfig

profile_config = ProfileConfig(
    profile_name="my_snowflake_profile",
    target_name="dev",
    profiles_yml_filepath="/path/to/profiles.yml",
)




Usefull URLS
============

https://astronomer.github.io/astronomer-cosmos/profiles/index.html

https://www.phdata.io/blog/how-to-utilize-astronomer-cosmos-to-streamline-dbt-workflows/#elementor-toc__heading-anchor

https://medium.com/hipay-tech/using-airflow-in-production-a-ci-cd-approach-e0b23a9aefc3

https://docs.getdbt.com/reference/artifacts/manifest-json

https://github.com/astronomer/cosmos-use-case

https://blog.det.life/making-data-pipeline-production-ready-taking-dbt-model-to-production-with-astronomer-5183d7a383c1

Deploy Your Project Locally
===========================

1. Start Airflow on your local machine by running 'astro dev start'.

This command will spin up 4 Docker containers on your machine, each for a different Airflow component:

- Postgres: Airflow's Metadata Database
- Webserver: The Airflow component responsible for rendering the Airflow UI
- Scheduler: The Airflow component responsible for monitoring and triggering tasks
- Triggerer: The Airflow component responsible for triggering deferred tasks

2. Verify that all 4 Docker containers were created by running 'docker ps'.

Note: Running 'astro dev start' will start your project with the Airflow Webserver exposed at port 8080 and Postgres exposed at port 5432. If you already have either of those ports allocated, you can either [stop your existing Docker containers or change the port](https://docs.astronomer.io/astro/test-and-troubleshoot-locally#ports-are-not-available).

3. Access the Airflow UI for your local Airflow project. To do so, go to http://localhost:8080/ and log in with 'admin' for both your Username and Password.

You should also be able to access your Postgres Database at 'localhost:5432/postgres'.

4. To set up your Snowflake Environment, first run the dbt creation script in include/creationscripts directory after editing it with your own password and then log in with the dbt_user it creates and run the following:

CREATE OR REPLACE DATABASE DEMO_dbt

This will create a warehouse, user, and database for us to use for this project

5. Next we'll create the tables we need for this project in our Snowflake Environment. To do this, log in as the dbt user and run the sql bookings1, bookings2, and customers statements from the include/creationscripts directory to create the bookings1, bookings2, and customers tables. 

6. Now that we have our Snowflake environment set up, go back to your Airflow UI and create a new Snowflake connection called "snowflake_conn" with the credentials for the dbt user. Here's an example:

{
  "account": "account12345",
  "warehouse": "dbt_dev_wh",
  "database": "demo_dbt",
  "region": "us-east-1",
  "role": "dbt_dev_role",
  "insecure_mode": false
}

7. Next, we'll also create some variables in the Airflow UI to store our dbt_user and password. The first variable will have the key of 'dbt_user' and value 'dbt_user'. The second variable will have the key of dbt_password and value of whatever password you set earlier. 

8. Now you're ready to run your DAG! Open up the Airflow UI and activate/manually trigger the dbt Snowflake DAG!
