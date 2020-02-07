# Deployment

The [CLI](https://github.com/adobe/aio-cli) provides out-of-the-box features for developers to manage the lifecycle of their  applications.

This documentation focuses on the application deployment step of this lifecycle.

# Setup Assumptions

In the following chapters of this documentation, it will be assumed that:

- The Custom Adobe Application has been bootstrapped from a [generator](https://github.com/adobe/generator-aio-app/) using the [CLI](https://github.com/adobe/aio-cli)
- There is a **.env** file at the root of the application folder, which contains the following keys and their values:

  - **AIO_RUNTIME_AUTH**, which holds the credentials for the Runtime namespace to use
  - **AIO_RUNTIME_NAMESPACE**, which holds the name of the Runtime namespace to use
  
If you do not own a [Runtime](https://github.com/AdobeDocs/adobeio-runtime) namespace, please [request trial access]().

# Deployment Scenarios

The [CLI](https://github.com/adobe/aio-cli) offers three types of deployment to the developers.

## Local Deployment

Local deployment capabilities are offered to developers who want to test and debug their application before this one gets deployed to the out-of-the-box Content Delivery Network.

### Local Runtime actions and UI

#### Technical Prerequisites

For this scenario, the following prerequisites should be installed on the developer's machine:

- [NodeJS](https://nodejs.org/en/download/) (at least v10). It should also install npm together. We recommend using [nvm](https://github.com/nvm-sh/nvm/blob/master/README.md) to manage NodeJS installation and versions on the developer's machine. 
- [Docker Desktop](https://www.docker.com/get-started)
- [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/overview/index.html) (at least Java 11).

The following commands must be executed to install the required Docker images:

```
docker pull openwhisk/action-nodejs-v10:latest
docker pull adobeapiplatform/adobe-action-nodejs-v10:3.0.21
```

**Note:** Developers on Windows machines should make sure that they are using Linux containers for the images above.
The steps to switch to Linux containers are described in the [Docker for Windows documentation](https://docs.docker.com/docker-for-windows/).

#### Use-Case

This local deployment feature is useful for developers who want to easily get an initial preview of their Custom Application before deploying it to [Runtime](https://github.com/AdobeDocs/adobeio-runtime) and to the out-of-the-box Content Delivery Network, along with local [Runtime](https://github.com/AdobeDocs/adobeio-runtime) actions and UI debugging capabilities.

It also helps developers who want to work on their Custom Application implementation without an appropriate Internet connection. However, the tradeoff will be that the developers will not be able to interact with [Adobe APIs](https://www.adobe.io/apis.html) or other systems that require a connection.

This deployment scenario doesn't require any specific credentials, as both [Runtime](https://github.com/AdobeDocs/adobeio-runtime) actions and application UI are hosted on the developer's machine.

#### Architecture

In this scenario, the [CLI](https://github.com/adobe/aio-cli) will download a [standalone instance](https://github.com/apache/openwhisk/tree/master/core/standalone) of [Apache OpenWhisk](https://openwhisk.apache.org/), which is the open source serverless platform behind [Runtime](https://github.com/AdobeDocs/adobeio-runtime), on the developer's machine.

The [Runtime](https://github.com/AdobeDocs/adobeio-runtime) actions of the application will be deployed to this local [Apache OpenWhisk](https://openwhisk.apache.org/) instance, and executed in NodeJS docker containers spinned up locally from the Docker images that are documented in the **Technical Prerequisites** section above.

The local [Apache OpenWhisk](https://openwhisk.apache.org/) instance runs on port 3233 by default, and the deployed actions will be accessible at:

```
http://localhost:3233/api/v1/web/guest/<appname-appversion>/<action-name>
```

**appname** and **appversion** are both application name and version, which are maintained in the package.json file at the root of the Custom Application source code folder.

**action-name** is the name of the action, which has been choosen by the developer when bootstrapping the application from the generator that was executed with `aio app init <appname>`.

In case of a headful Custom Application, the UI will be served locally from [ParcelJS](https://parceljs.org/cli.html), which is the underlying framework used by the [CLI](https://github.com/adobe/aio-cli) to build the front-end source code.

### Remote Runtime actions and local UI

#### Technical Prerequisites

This deployment scenario requires [Runtime](https://github.com/AdobeDocs/adobeio-runtime) credentials in a .env file at the root of the Custom Application source code folder, as documented in the **Setup Assumptions** above.

#### Use-Case

This feature is useful for developers who want to test their Custom Application in an integrated live environment with minimal deployment time and efforts. 

#### Architecture

The UI is still served locally from [ParcelJS](https://parceljs.org/cli.html), which allows hot updates of the front-end code, which is integrated to [Runtime](https://github.com/AdobeDocs/adobeio-runtime) actions deployed to the developer's namespace.

## Full Deployment

### Focus on the token-vending machine

### Out-of-the-box Content Delivery Network