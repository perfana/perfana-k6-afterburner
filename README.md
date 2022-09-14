# perfana-k6-afterburner

Demonstrates how to use a K6 script with Perfana.

This setup is with a Jenkins pipeline and a Maven pom.xml. Other setups are also possible, such as using other pipelines such as Github Actions,
and use plain Java instead of Maven.

Note that for this K6 demo to run, the K6 executable should be installed on the Jenkins runner.

The `script.js` is generated via the [OpenAPI generator](https://github.com/OpenAPITools/openapi-generator).
The input is the OpenAPI spec from Afterburner.

Added to the K6 script are a csv reader to call the employee database with a list of different names.

Requests also have added baggage tag to the HTTP headers to be used in distributed tracing.

The script fetches several values from environment variables, such as `TARGET_BASE_URL` and `TEST_RUN_ID`.

The script pushes test metrics to influxdb by specifying `--out influxdb=${influxUrl}` in the `CommandRunner` config
and supplying the username and password for influx. 

The `pom.xml` shows how the K6 script is used within the Perfana event-scheduler eco system.
Multiple event-plugins are used to send data to Perfana, such as config data, actuator data from the SpingBoot apps
and current git hash. And the K6 load script is automatically stopped when Perfana sees certain
thresholds are exceeded.

Related repositories:
* [perfana-client-as-code-demo](https://github.com/perfana/perfana-client-as-code-demo) - use code instead of Maven
* [perfana-java-client](https://github.com/perfana/perfana-java-client) - connect to Perfana
* [test-events-command-runner](https://github.com/perfana/test-events-command-runner) - used to run the K6 command
* [afterburner](https://github.com/perfana/afterburner) - the SpringBoot application that is tested

