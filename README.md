# 42

To deploy **WordPress** and **MySQL** containers together using **Docker Compose**, follow these steps. This will set up WordPress with a MySQL database and allow you to access the WordPress frontend.

### Step-by-Step Guide

#### 1. Install Docker and Docker Compose
Ensure Docker and Docker Compose are installed and running on your system.

#### 2. Create a Project Directory
Create a new directory for this setup, which will contain the Docker Compose file.

```bash
mkdir wordpress-docker
cd wordpress-docker
```

#### 3. Create the Docker Compose File
Create a `docker-compose.yml` file inside the `wordpress-docker` directory with the following content:

```yaml
version: '3.8'

services:
  db:
    image: mysql:latest
    container_name: wordpress-mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress
    restart: always
    depends_on:
      - db
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: user
      WORDPRESS_DB_PASSWORD: password
    volumes:
      - wordpress_data:/var/www/html

volumes:
  db_data:
  wordpress_data:
```

### Explanation of `docker-compose.yml`

- **db service**:
  - Uses the MySQL 5.7 image for compatibility with WordPress.
  - Sets up environment variables for MySQL root password, database name, user, and password.
  - Maps the database data to the `db_data` volume for data persistence.

- **wordpress service**:
  - Uses the latest WordPress image.
  - Depends on the `db` service, ensuring that the MySQL database starts first.
  - Maps port `8080` on the host to port `80` in the WordPress container so that you can access WordPress via `http://localhost:8080`.
  - Configures WordPress to connect to the MySQL database with the provided credentials.

### 4. Start the Docker Compose Setup

In your terminal, run the following command to start the services:

```bash
docker-compose up -d
```

This will download the necessary images, create the containers, and start them in detached mode (`-d` flag).

### 5. Access the WordPress Frontend

1. Open a web browser.
2. Go to `http://localhost:8080`.

   This should open the WordPress installation page. You can set up WordPress by following the on-screen instructions.

### 6. Verify the Containers are Running

To confirm that both containers are running, use:

```bash
docker-compose ps
```

You should see both the `wordpress` and `wordpress-mysql` containers listed and marked as "Up".

