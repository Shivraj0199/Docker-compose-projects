# Create a wordpress image & container using Docker-compose file

## **Step 1:** Make sure you have installed all the dependencies (Install Docker-compose)

1) sudo su
2) curl -SL https://github.com/docker/compose/releases/download/v2.30.3/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
3) chmod +x /usr/local/bin/docker-compose
4) docker-compose -v

## **Step 2:** Create a project directory 

1) mkdir wordpress-compose
2) cd wordpress-compose

## **Step 3:** Create a wordpress.yml file

1) nano wordpress.yml

```
version: '3.9'

services:
  db:
    image: mysql:5.7
    container_name: wordpress_db
    restart: always
    ports:
      - "3306:3306"
    volumes:
      - myvol:/var/lib/mysql
    networks:
      - mynet
    environment:
      MYSQL_ROOT_PASSWORD: Pass@123
      MYSQL_DATABASE: wordpressdb

  website:
    image: wordpress:latest
    container_name: wordpress_site
    restart: always
    depends_on:
      - db
    ports:
      - "80:80"
    networks:
      - mynet
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: Pass@123
      WORDPRESS_DB_NAME: wordpressdb

networks:
  mynet:
    driver: bridge

volumes:
  myvol:
```

## **Step 4:** Start the containers

``` docker-compose -f wordpress.yml up -d ```

![](https://github.com/Shivraj0199/Docker-compose-projects/blob/main/Wordpress/Img/Screenshot%202025-09-22%20152136.png)

## **Step 5:** Verify containers are running

``` docker-compose -f wordpress.yml ps ```

``` docker ps ```

## **Step 6:** Access wordpress

1) Open in your browser (Make sure port 8080 is open in AWS Security Groups)

``` http://<ec2-public-ip>:8080 ```

2) You’ll see the WordPress installation page

![](https://github.com/Shivraj0199/Docker-compose-projects/blob/main/Wordpress/Img/Screenshot%202025-09-22%20152219.png)

## **Step 7:** Stopping and Restarting

* Stop containers

``` docker-compose down ```

* Restart containers

``` docker-compose up -d ```

## **Step 8: (Optional)** If you have any issue about storage, Increase EBS size

* Choose your EC2 instance
* Go to volumes -> modify volume -> increase the size (16 GB) -> modify
* Do ssh in the power-shell
* Fire the following commands

1) lsblk (check disk)
2) df -h (check new size)
3) sudo growpart /dev/xvda 1 (increase partation)
4) df -h (check new size/verify)
5) sudo xfs_growfs -d / (resize file system)
