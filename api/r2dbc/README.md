# TODO - Java Spring Boot (with R2DBC) API

1. [Requirements](#requirements)
2. [Getting started with the app](#getting-started)
    1. [Configure the code](#configure-code)
    2. [Build the code](#build-code)
    3. [Run the app](#run-app)

## Requirements <a name="requirements"></a>

This sample was created using the following techologies and they must be installed before proceeding.

* [Java 8+](https://www.java.com/en/download/)
* [Maven v.3+](https://maven.apache.org/)

## Getting started with the app <a name="getting-started"></a>

### Configure the code <a name="configure-code"></a>

Configure the MariaDB connection by [adding a db.properties file to the Java project](https://docs.oracle.com/javase/tutorial/essential/environment/properties.html) called "db.properties" into a folder called `static` that lives within the [resources folder](src/main/resources).

Example implementation:

```
host=sky000XXX.mdb000XXXX.db.skysql.net
port=5002
username=DB0000XXXX
password=*************
database=todo
clientSslCert=classpath:static/skysql_chain.pem
```

**Configuring MariaDBClient.java**

The (database) environmental variables from `db.properties` (located in resources/static/) are used within the [R2DBCConfig.java](src/main/java/com/mariadb/backend/config/R2DBCConfig.java) for the MariaDB R2DBC Connector configuration settings:

```java
conf = MariadbConnectionConfiguration.builder()
                    .host(props.getProperty("host"))
                    .port(Integer.parseInt(props.getProperty("port")))
                    .username(props.getProperty("username"))
                    .password(props.getProperty("password"))
                    .database(props.getProperty("database"))
                    .build();
```

**Configuring [R2DBCConfig.java](src/main/java/com/mariadb/backend/config/R2DBCConfig.java) for the MariaDB cloud database service [SkySQL](https://mariadb.com/products/skysql/)**

MariaDB SkySQL requires SSL additions to connection. 

```java
MariadbConnectionConfiguration.builder()
    .host(props.getProperty("host"))
    .port(Integer.parseInt(props.getProperty("port")))
    .username(props.getProperty("username"))
    .password(props.getProperty("password"))
    .database(props.getProperty("database"))
    // Add the following ***************************** 
    .sslMode(SslMode.ENABLE_TRUST)
    .clientSslCert(props.getProperty("clientSslCert"))
    // ***********************************************
    .build()
```

Also, remember to add the following to the db.properties (located in resources/static/) file:

```
clientSslCert=classpath:static/skysql_chain.pem
```

### Build the code <a name="build-code"></a>

Once you have retrieved a copy of the code you're ready to build and run the project! 

Build the project by executing the following CLI command:

```
$ mvn package
```

### Run the app <a name="run-app"></a>

Once you've pulled down the code and have verified built the project you're ready to run the application! 

1. Execute the following CLI command 

```bash
$ mvn spring-boot:run
```

The following steps also exist within the ["Build and run"](../../#build-and-run-the-app-) section of the root README, and are for startin the React.js project once this API project has been started.

2. Navigate to the [../../client](client) folder and execute the following CLI command to start the React.js application.

```bash 
$ npm start
```

3. Open a browser window and navigate to http://localhost:3000.
