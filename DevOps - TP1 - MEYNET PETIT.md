# DevOps: TP1 - Docker

## Database.
*__User / password in Dockerfile ?__*
Plain text user/password in "Dockerfile" isn't very secure. It is never recommended to put these informations in a human readable file. Using the "-e" when running the image is better.
``
docker run -e "POSTGRES_USER=guest POSTGRES_PASSWORD=guest"
``

*__Add local scripts to docker image folder "docker-entrypoint-initdb.d"__*
1. Create locals SQL scripts.
2. Append ``ADD [script_name] /docker-entrypoint-initdb.d`` to the Dockerfile.
3. Rebuild the image from the Dockerfile.
4. At image startup, the database is now populated with the scripts added.

The ``-v /my/own/datadir:/var/lib/postgresql/data`` option is required if we want the data to be persistent. Now our local directory "datadir" is binded to the "postgresql/data" directory. It means that even if the container is destoyed, datas will stay in our local directory.
*You can also specify it in the Dockerfile with the ``VOLUME`` instruction but like the user/password info, it is more secure to put it in command line*

## Backend API
*__Why using multistage build ?__*
In the first step, we compiled our java code and run it with the container. 
The second step introduce the multistage build. The container will first compile the code with openjdk then get the output file generated and run it with openjre. It is very useful when you are in a developer team: everyone compile on the same image so we are sure we are all using the same jdk/jre versions.
*__Why using Maven ?__*
Here we have only one file to compile and it's a simple "Hello World". If we have a lot of files, it will be endless to compile them one by one. That's why we use Maven. It groups all dependencies in a single file, which allows us to run a single file with all dependencies.
*__How to interact with Postgres DB from Web API ?__*
First we have created a network called "my-net" with ``docker network create my-net``.
After that, when we run both the postgres and the api containers, we specfied ``--network my-net`` and finally on the api one we added a binding between port 8080 from the container to port 8080 from the host with ``-p 8080:8080``. It allows us to navigate through the database with a web navigator (eg: http://localhost:8080/departments/IRC/students to get all students in the IRC department).

## HTTP Server
*__Httpd.conf__*
To use the reverse proxy properly we have to enable the "mod_proxy" and the "mod_proxy_http" in the httpd.conf file, then add the ``Virtualhost`` section. The container is connected to the same network as the back api and the DB so we can retrieve the back api from it's hostname.

## Docker-Compose
*__Useful subcommands__*
``docker-compose up`` to start the docker-compose
``docker-compose down`` to shutdown the docker-compose
``docker-compose help`` to see help on docker-compose sub-commands

## Docker Hub
