# ARA testing

This is a simple ARA docker compose infrastructure for testing a
standalone ansible-ara docker container.

# Setup

this is a 3 container compose:

- ara-web is the contributer web frontend
- ara-api is the Django server and API endpoint.
- ansible-ara is the actual container I want to build.

# Usage

The following will build all 3 images as :latest. You can optionaly rebuild any image by using the service name from the docker-compose file.

```
docker-compose build [ara-web|ara-api|ansible-ara]
```

To run :

```
docker-compose up
```

This will start the API container and the frontend ara-web - properly configured to talk to each other.

The ansible-ara container will start up and run an ansible playbook ( see [env] ) and exit. The following will happen:

- Docker compose will map the env directory to /tmp/workdir
- The API end point is set (based on Docker Compose networking)
- The wrapper script start setting the ara environment files
- ansible galaxy will pul down ansible dependencies (requirements.yml)
- The ansible playbook (playbook.yml) is run against the inventory (inventory.ini)

The current env is really just a simple test that runs against itself with a local connection ( no ssh ). The playbook does a simple ping.

You can keep running it by issuing the :

```
docker-compose run ansible-ara
```

# Running outside of docker compose.

The ansible-ara container can also be run standalone as folows:

```
docker run -it --rm -v $(pwd):/tmp/workbook -w /tmp/workbook ansible-ara
```

But currently it is still development - so it is better to run :

```
docker run -it --rm -v $(pwd):/tmp/workbook -w /tmp/workbook ansible-ara /bin/bash
```

as an Ansible shell and run the commands manully until all is set.
