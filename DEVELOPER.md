api
===
project defining the REST API itself. See api/api.json for a full
description of the API.

www
===
project defining the web application

Generated Code
==============
The .apidoc file lists the specific targets which are generated by
apidoc. To regenerate code, clone github.com/gilt/apidoc-cli and

    apidoc upload flow splashpage ./splashpage.json --version `sem-info tag latest`
    
    apidoc update

Database
========

    See https://github.com/flowcommerce/splashpage-postgresql

Running in prod
===============

    docker run -d -p 80:9000 flowcommerce/splashpage:0.1.12 production

Running locally
===============

In one screen:

    $ sbt
    sbt> project api
    sbt> run 9001

In another screen:

    $ sbt
    sbt> project www
    sbt> run

Goto http://localhost:9000 in your browser

Initial production setup:

    sudo yum install http://yum.postgresql.org/9.4/redhat/rhel-6-x86_64/pgdg-ami201503-94-9.4-1.noarch.rpm
    sudo yum install postgresql94

    psql -U root -h splashpage.crqe2ozpjr64.us-east-1.rds.amazonaws.com postgres
      psql> CREATE DATABASE splashpage;
      psql> CREATE ROLE api WITH LOGIN PASSWORD 'PASSWORD';
      psql> GRANT ALL ON DATABASE splashpage TO api;

    echo "splashpage.crqe2ozpjr64.us-east-1.rds.amazonaws.com:5432:splashpage:api:PASSWORD" > ~/.pgpass
    chmod 0600 ~/.pgpass

    cd /web/splashpage-postgresql
    sem-dist
    scp -i /web/keys/ssh/mbryzek-key-pair-us-east.pem dist/schema-0.0.1.tar.gz ec2-user@54.175.54.172:~/
    ssh -i /web/keys/ssh/mbryzek-key-pair-us-east.pem ec2-user@54.175.54.172
    tar xfz schema-*.tar.gz
    cd schema-*

    sudo yum install git

    sem-apply --user api --host splashpage.crqe2ozpjr64.us-east-1.rds.amazonaws.com --name splashpage
