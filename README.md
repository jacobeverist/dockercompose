# JobSnap Container Assemblage

This includes the MongoDB database, the Python Flask API server, and the Apache hosted website with PHP server-side scripts.

First, you need to install Docker Engine on your host machine.  See your OS for details of installation:

https://docs.docker.com/engine/installation/


Next, you need to install Docker Compose.  Again, see the details for your particular OS:

https://docs.docker.com/compose/install/


For an OSX or Windows installation, you need to startup your Docker terminal and start the Docker-Machine, which is a virtual machine running Linux and the Docker Engine.

You can check the status of the Docker Machine by running:

    docker-machine status

You can check its IP address by running:

    docker-machine ip

Record this IP.  You will need it.

Next, open the file, `docker-compose.yml`, and look for the line that says:

      WEB_HOST: "http://192.168.99.100/"
    
Change this IP address to that of the docker-machine IP.


To run the JobSnap database, API and web server, from the Docker terminal, navigate to root directory of the source directory containing this README file.  Run the following commands:

    docker-compose build
    docker-compose up

The first command will build the container images and download all the dependencies.  The second command will start the containers and network them together appropriately.

Now, you need only navigate to the website from your local browser to the IP address of the Docker machine.  In my case, it is http://192.168.99.100

The default configuration runs the setup in testing mode.  This means that Telesign is disabled and the login for the Mongo database is empty.  At the Telesign verification prompt, you can enter any number and you will be verified.  This is so we don't bother the people at Telesign while we are doing development and testing.

To change from testing to deployment configuration, you need to change the line in `docker-compose.yml` from

    JOBSNAP_SETTINGS: config/testing.cfg
    
to

    JOBSNAP_SETTINGS: config/deploy.cfg

This re-enables Telesign and also uses the proper login and password for the Production JobSnap Mongo database.


We also have two more Docker-Compose YAML scripts that setup the servers for testing.  You can execute with either of the following:

    docker-compose -f jobsnap-test.yml up
    docker-compose -f jobsnap-remote-test.yml up


The first runs unit tests on the API server.  The second starts a client and makes remote calls on the API server.








