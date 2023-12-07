

# Docker networking

The prestashop image available [Prestashop image avaible here](https://hub.docker.com/r/bitnami/prestashop) can be used to deploy an e-commerce application. This application make use of two components. a fronten website and a database for storing persistante data.
We used the [prestashop/prestashop](https://hub.docker.com/r/prestashop/prestashop) instead of bitnami.

## Task 1

Deploy this application inside a network. Make sure the two containers can communicate with each other using their names.
1. Create prestNetwork
   ```docker network create prestaNetwork```
2. Create a mariadb container as a database :
```docker run -d --name mariadb --network prestaNetwork -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=prestashop_db -e MYSQL_USER=prestashop_user -e MYSQL_PASSWORD=1234 -v mariadb_data:/var/lib/mysql mariadb```
3. Create a prestashop container :
```docker run -d --name prestashop --network prestaNetwork -e DB_SERVER=mariadb_ynov -e DB_NAME=prestashop_db -e DB_USER=prestashop_user -e DB_PASSWD=1234 -p 8080:80 -v prestashop_data:/var/data/html prestashop/prestashop```
4.Install ping command on both contianers : 
prestashop container : 
```docker exec -ti -u 0 prestashop apt-get update```
```docker exec -ti -u 0 prestashop apt-get install -y iputils-ping```

mariadb container :

```docker exec -ti -u 0 mariadb apt-get update```
```docker exec -ti -u 0 mariadb apt-get install -y iputils-ping```
5. Do the ping using names of the containers :
   we access inside the prestashop container and we run the command ```ping mariadb```
   we access inside the mariadb container and we run the command ```ping prestashop```
And this way we konw that the two containers can communicate to each other
6. Access from localhost :
   via ce [lien](http://loclahost:8080)

# Task 2

1. We created two networks : prestashop-network and mariadb-network using this commands :
   ```docker network create --subnet=10.0.1.0/24 mariadb-network```
   ```docker network create --subnet=10.0.0.0/24 prestashop-network```
2. We connected the container mariadb to the mariadb-network and we diconnected it from the prestaNetwork
   We did the same thing with the container of prestashop

```docker network connect mariadb-network mariadb
docker network connect prestashop-network prestashop

docker network disconnect prestaNetwork mariadb
docker network disconnect prestaNetwork prestashop
```
3. Create a router using the nginx image :
```docker run -d --name router --network prestashop-network --privileged -p 80:80 nginx```
```docker network connect mariadb-network router```
4. install ping et ip route with the command:
```apt update -y ```
```apt install -y iputils-ping```
```apt install -y iproute2```
5. Configure the route table:
```ip route add 10.0.1.0/24 via 10.0.1.3```
```ip route add 10.0.0.0/24 via 10.0.0.3```

# Team members : 
- Kafia Airouche
- Lydia Ouamrane

