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
