# Docker networking

The prestashop image available [Prestashop image avaible here](https://hub.docker.com/r/bitnami/prestashop) can be used to deploy an e-commerce application. This application make use of two components. a fronten website and a database for storing persistante data.

## Task 1

Deploy this application inside a network. Make sure the two containers can communicate with each other using their names.
1. Create prestNetwork
   ```docker network create prestaNetwork ```
3. Create a mariadb container as a database :
   ```docker run -d --name mariadb -e ALLOW_EMPTY_PASSWORD=yes -e MARIADB_USER=bn_prestashop -e MARIADB_PASSWORD=bitnami -e MARIADB_DATABASE=bitnami_prestashop --network prestaNetwork bitnami/mariadb```
4. Create a prestashop container :
``` docker run -d --name prestashop -p 8081:8080 -p 443:8443 -e ALLOW_EMPTY_PASSWORD=yes -e PRESTASHOP_DATABASE_USER=bn_prestashop -e PRESTASHOP_DATABASE_PASSWORD=bitnami -e PRESTASHOP_DATABASE_NAME=bitnami_prestashop --network prestaNetwork bitnami/prestashop``

5.Install ping command on both contianers : 
```docker exec -ti -u 0 prestashop apt-get update
docker exec -ti -u 0 prestashop apt-get install -y iputils-ping```

Access from localhost :
Error

# Task 2

- Now create two networks with the cidr specified in the schemas. Please make use of `--subnet` for adding a subnet to the network when creating it. The name of the networks are `ynov-frontend-network` for the frontend website container and `ynov-backend-network` for the database container.

Hint: in order for the two containers to talk to each other,

- make use of a third container that can be called `gateway` or `router` and connect this container to the two above networks.
- Inside the router gateway, configure the route table by using ip below command

`ip route add <FROM_CIDR_REPLACE_ME> via <GATEWAY_IP_FROM_EACH_NETWORK_SIDE>`
do the same command for the two networks.

Note: The goal for the second task is not to deploy the application but to make sure the containers can talk to each other using their ips.

# Submission

You need to do the job in a team of 2 maximum. No single submission allowed.

- Produce a pdf of your work. Put down all required screenshots that prove your work with descriptions how how you achieve things
- Create a public github repository and in moodle submit only your repository url.
- Create a clean read me file (table of content, sections, screens and so on that show your work)
- In the read me file, dont forget to put your team members names
