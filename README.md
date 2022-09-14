# perfana-k6-afterburner

Demonstrates how to use a K6 script with Perfana.

This setup is with a Jenkins pipeline and a Maven pom.xml. Other setups are also possible, such as using other pipelines such as Github Actions,
and use plain Java instead of Maven.

Note that for this K6 demo to run, the K6 executable should be installed on the Jenkins runner.

Related repositories:
* [perfana-client-as-code-demo](https://github.com/perfana/perfana-client-as-code-demo) - use code instead of Maven
* [perfana-java-client](https://github.com/perfana/perfana-java-client) - connect to Perfana
