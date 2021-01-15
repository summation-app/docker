# 1 API for all your apps' data

[![](summation_dataflow.png "Summation DataFlow")](https://www.summation.app/assets/img/summation_dataflow.png)


A data gateway to securely query databases & third-party APIs from apps using SQL & REST, respectively.  Authenticates users via JSON Web Tokens (JWTs), checks if the query/request is allowed, and then proxies data to your app.

Features:
* Supports every major SQL database/warehouse (MySQL, PostgreSQL, SQL server & <a href='https://docs.summation.app/project/frequently-asked-questions#which-databases-are-supported'>more</a>)
* Supports authentication via Firebase, Okta, AWS Cognito, Auth0, and JWT
* Clients for the <a href='https://github.com/summation-app/web_client'>web</a> (Javascript), <a href='https://github.com/summation-app/iOS_client'>iOS</a> (Swift), and <a href='https://github.com/summation-app/android_client'>Android</a> (Kotlin)
* Centralized logging to AWS, DataDog, Splunk, Elasticsearch & more

# Installation

Please ensure you have [Docker](https://docs.docker.com/get-docker/) installed.  Then from the command line:

    curl -o docker-compose.yml https://cdn.jsdelivr.net/gh/summation-app/docker/docker-compose-production.yml
    export ADMIN_PASSWORD=YOUR_PASSWORD_HERE
    docker-compose up # add '-d' to the end of this command to run in the background
    
You can then login by visiting: http://yourhost:8080

# Security

Summation is designed to be secure by default, by ensuring that only queries/requests that have been pre-approved during development can be executed in production.  User IDs may be extracted from their JWT token and bound into the queries to ensure correct access controls.  Pre-approval is designed to be as seamless as possible, by allowing developers to automatically allow any query/request that's made with the development token.  In production, a separate production token is passed to the gateway, and only those queries & requests that have been approved get executed.  All credentials for databases & APIs are encrypted with the ADMIN_PASSWORD and stored in the database.  Additional role-based access control & source control-based allowed queries are in development.

# the problem we're solving
Every time your apps need to read or write data to a database or third-party API, server-side code needs to be written.  This code is highly repetitive, and typically handles user authentication, then queries the data required, and encodes the result back into JSON for your app to consume.  Doing this directly from the mobile or web app would be a lot simpler, but:

* you can't allow arbitrary SQL queries from apps, due of [SQL injection](https://xkcd.com/327/)
* you can't directly call third-party APIs, as exposing your API keys in your source code is a major security risk
* even if you could, third-party APIs often prevent requests from web browsers with CORS

## we're a good fit when:
* you have mobile/web apps that need to exchange data with your SQL databases & third-party APIs, but you don't want to wait for you or your backend developers to write APIs for your apps to query
* you & your developers all know SQL, and you want the full power of SQL available

## we're *not* a good fit if:
* you only use noSQL databases
* you don't use any third-party cloud APIs, like Stripe or Twillio

# Documentation

 - [Quick start guide](https://docs.summation.app/quick-start/installation)
 - [F.A.Q.](https://docs.summation.app/project/frequently-asked-questions)
 - [full API documentation](https://docs.summation.app/web-client-api/database-queries)

# Support

* We monitor StackOverflow questions [tagged with #summation](https://stackoverflow.com/questions/tagged/summation)
* [Bug Reports](https://github.com/summation-app/summation/issues)

# Development & Testing

* To run the test suite:

    python tests/test_server.py & pytest test.py


