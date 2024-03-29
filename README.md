# RandomStrings – Using Jakarta EE + MicroProfile
A demo project built with Jakarta EE and MicroProfile – a service that returns random combination of an adjective and a noun as a JSON list.

## Local Build and Run
### Building and Running the Application Locally

The code can be deployed to several runtimes. This is done to illustrate the switching runtimes with minor changes to the code and to observe the performance.

| Runtime          | Build                                      | Run                                                | Address                       |
|------------------|--------------------------------------------|----------------------------------------------------|-------------------------------|
| **Open Liberty** | ```mvn -f pom-liberty.xml clean package``` | ```java -jar target/randomstrings.jar```           | http://localhost:8080/api/rnd |
| **Quarkus**      | ```mvn -f pom-quarkus.xml clean package``` | ```java -jar target/quarkus-app/quarkus-run.jar``` | http://localhost:8080/api/rnd |
| **Helidon**      | ```mvn -f pom-helidon.xml clean package``` | ```java -jar target/randomstrings.jar```           | http://localhost:8080/api/rnd |

**_Note:_** You can run your Quarkus application in `dev mode` that enables live code reloading:

```shell script
./mvnw -f pom-quarkus.xml quarkus:dev
```

### Building and Running the Application Locally in a Container
All containers are multistage build containers and can be found in a [folder structure][10] for each runtime and are marked with `jvm` and `native`, depending on flavour you want to build. The same containers are used to create images for the Cloud deployments as well.

## Building Container Images and Cloud Deployment: Cloud Build and Cloud Run

### Create Artifact Registry Repository
https://cloud.google.com/artifact-registry/docs/repositories/create-repos#docker

### Build images using Cloud Build, add to the registry and deploy to Cloud Run

| Runtime               | Build & Deploy to Cloud Run                                                                     |
|-----------------------|-------------------------------------------------------------------------------------------------|
| **Quarkus – JVM**     | ```gcloud builds submit --substitutions=_APP_RUNTIME="quarkus",_APP_RUNTIME_FLAVOUR="jvm"```    |
| **Quarkus – Native**  | ```gcloud builds submit --substitutions=_APP_RUNTIME="quarkus",_APP_RUNTIME_FLAVOUR="native"``` |
| **OpenLiberty – JVM** | ```gcloud builds submit --substitutions=_APP_RUNTIME="liberty",_APP_RUNTIME_FLAVOUR="jvm"```    |
| **Helidon – JVM**     | ```gcloud builds submit --substitutions=_APP_RUNTIME="helidon",_APP_RUNTIME_FLAVOUR="jvm"```    |
| **Helidon – Native**  | ```gcloud builds submit --substitutions=_APP_RUNTIME="helidon",_APP_RUNTIME_FLAVOUR="native"``` |


## Application Setup and Links
### Port Configuration
Cloud Run uses port 8080 by default. All runtimes are configured to expose that port. These configurations are done in: 

| Runtime         | Port Config                                  |
|-----------------|----------------------------------------------|
| **Quarkus**     | defaults to 8080                             |
| **OpenLiberty** | setup in [server.xml][8]                     |
| **Helidon**     | setup in [microprofile-config.properties][9] |


### Config
Configuration of your application parameters ([specification][2]).

The example class `RandomStringsSupplier` shows you how to configure and define default values for variables.

### Health
The health status can be used to determine if the 'computing node' needs to be discarded/restarted or not ([specification][3]).

The class `ServiceHealthCheck` contains an example of a custom check which can be integrated to health status checks of the instance.  The index page contains a link to the status data.

### Metrics
The Metrics exports _Telemetric_ data in a uniform way of system and custom resources ([specification][4]).

The class `RandomStringsController` contains an example how you can measure the execution time of a request. The index page also contains a link to the metric page (with all metric info).

### Open API
Exposes the information about your endpoints in the format of the OpenAPI v3 specification ([docs][5]).

The index page contains a link to the OpenAPI information of the available endpoints for this project.


## Authors
- [Rustam Mehmandarov][6]
- [Mads Opheim][7]


[1]: https://microprofile.io/
[2]: https://microprofile.io/project/eclipse/microprofile-config
[3]: https://microprofile.io/project/eclipse/microprofile-health
[4]: https://microprofile.io/project/eclipse/microprofile-metrics
[5]: https://microprofile.io/project/eclipse/microprofile-open-api
[6]: https://github.com/mehmandarov
[7]: https://github.com/madsop
[8]: src/main/liberty/config/server.xml
[9]: src/main/resources/META-INF/microprofile-config.properties
[10]: src/main/docker