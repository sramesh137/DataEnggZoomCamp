## Table of Contents

- [Introduction to Docker](#introduction-to-docker)
- [Setting up Docker](#setting-up-docker)
- [Docker Desktop Installation on Mac](#docker-desktop-installation-on-mac)
- [PostgreSQL Setup using Docker Compose](#postgresql-setup-using-docker-compose)
- [Accessing pgAdmin](#accessing-pgadmin)
- [Data Ingestion](#data-ingestion)

# Introduction to Docker

Docker is a platform designed to help developers build, share, and run applications with containers. Containers allow you to package an application with all its dependencies into a standardized unit for software development.

## Use Cases

- **Simplified Configuration:** Docker allows you to configure your application once and run it anywhere.
- **Code Pipeline Management:** Docker can be used to manage and streamline the development, testing, and deployment of applications.
- **Developer Productivity:** Docker containers can be started and stopped quickly, which speeds up the development process.
- **Isolation:** Each container runs in its own isolated environment, which helps in avoiding conflicts between different applications.

## Setting up Docker

To get started with Docker, you need to install Docker Desktop on your machine. Follow the installation guide below to set up Docker on your Mac.

## Docker Desktop Installation on Mac

To install Docker Desktop on your Mac, follow these steps:

1. **Download Docker Desktop:**
    - Visit the [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop) page.
    - Click on the "Download for Mac" button.

2. **Install Docker Desktop:**
    - Open the downloaded `.dmg` file.
    - Drag the Docker icon to the Applications folder.

3. **Start Docker Desktop:**
    - Open Docker from the Applications folder.
    - Follow the on-screen instructions to complete the setup.

4. **Verify Docker Desktop installation:**
    - Open a terminal and run the following command:
        ```sh
        docker --version
        ```
    <img width="804" alt="image" src="https://github.com/user-attachments/assets/9b1d432a-5166-4b61-a861-7c59d376b178" />

    - You should see the Docker version information.

You have successfully installed Docker Desktop on your Mac.

## Docker "Hello World"

<img width="804" alt="image" src="https://github.com/user-attachments/assets/1501e2ca-365d-48b6-b29c-e6fae1dd6808" />

## Run Docker Ubuntu

<img width="804" alt="image" src="https://github.com/user-attachments/assets/f4a689e9-53bf-4505-b1ed-88e243a7dbf6" />

## Run Docker with Python Image

To run Docker with the `python:3.12.8` image in interactive mode and use `bash` as the entrypoint, follow these steps:

1. **Pull the Python Docker image:**
    ```sh
    docker pull python:3.12.8
    ```

2. **Run the Docker container:**
    ```sh
    docker run -it --entrypoint bash python:3.12.8
    ```

This command will start a Docker container with the `python:3.12.8` image in interactive mode and use `bash` as the entrypoint.

## PostgreSQL Setup using Docker Compose

To set up PostgreSQL using Docker Compose, follow these steps:

1. **Create a `docker-compose.yml` file:**

    ```yaml
    services:
    db:
        container_name: postgres
        image: postgres:17-alpine
        environment:
        POSTGRES_USER: 'postgres'
        POSTGRES_PASSWORD: 'postgres'
        POSTGRES_DB: 'ny_taxi'
        ports:
        - '5433:5432'
        volumes:
        - vol-pgdata:/var/lib/postgresql/data

    pgadmin:
        container_name: pgadmin
        image: dpage/pgadmin4:latest
        environment:
        PGADMIN_DEFAULT_EMAIL: "pgadmin@pgadmin.com"
        PGADMIN_DEFAULT_PASSWORD: "pgadmin"
        ports:
        - "8080:80"
        volumes:
        - vol-pgadmin_data:/var/lib/pgadmin 

    volumes:  
    vol-pgdata:
        name: vol-pgdata
    vol-pgadmin_data:
        name: vol-pgadmin_data

    ```

2. **Start the PostgreSQL container:**

    Navigate to the directory containing the `docker-compose.yml` file and run:

    ```sh
    ➜  Week 1 git:(main) ✗ docker-compose up -d
    [+] Running 5/5
     ✔ Network week1_default      Created                                                                                                                                       0.0s 
     ✔ Volume "vol-pgdata"        Created                                                                                                                                       0.0s 
     ✔ Volume "vol-pgadmin_data"  Created                                                                                                                                       0.0s 
     ✔ Container pgadmin          Started                                                                                                                                       0.2s 
     ✔ Container postgres         Started
    ```

    This command will start the PostgreSQL container in detached mode.

3. **Verify the PostgreSQL container is running:**

    ```sh
    docker-compose ps
    ```

    You should see the PostgreSQL container listed and its status as "Up".

4. **Connect to the PostgreSQL database:**

    You can connect to the PostgreSQL database using any PostgreSQL client. For example, using `psql`:

    ```sh
    psql -h localhost -U exampleuser -d exampledb
    ```
    
    Enter the password `examplepass` when prompted.

    You have successfully set up PostgreSQL using Docker Compose.
## Accessing pgAdmin

To access pgAdmin, open your web browser and navigate to `http://localhost:8080`. Use the following credentials to log in:

- **Email:** pgadmin@pgadmin.com
- **Password:** pgadmin

Once logged in, you can add a new server to connect to your PostgreSQL database.

## Adding a New Server in pgAdmin

1. **Open pgAdmin:**
    - Navigate to `http://localhost:8080` in your web browser.
    - Log in with the credentials provided above.

2. **Add a New Server:**
    - Right-click on "Servers" in the left-hand menu and select "Create" > "Server...".
    - In the "General" tab, enter a name for your server (e.g., `PostgreSQL`).

3. **Configure Connection:**
    - Switch to the "Connection" tab.
    - Enter the following details:
        - **Host name/address:** `postgres`
        - **Port:** `5432`
        - **Username:** `postgres`
        - **Password:** `postgres`
    - Click "Save" to add the server.

You should now see your PostgreSQL server listed under "Servers" in pgAdmin. You can expand it to view and manage your databases.

### Stopping the Containers

To stop the PostgreSQL and pgAdmin containers, navigate to the directory containing your `docker-compose.yml` file and run:

```sh
docker-compose down
```

This command will stop and remove the containers defined in the `docker-compose.yml` file.

## Data Ingestion

    For the data ingestion, I created the following tables by logging into `pgAdmin`:

### Green Taxi Trips Table

    ```sql
    CREATE TABLE green_taxi_trips (
        VendorId INT,
        LpepPickupDatetime TIMESTAMP,
        LpepDropoffDatetime TIMESTAMP,
        StoreAndFwdFlag CHAR(1),
        RatecodeId INT,
        PuLocationId INT,
        DoLocationId INT,
        PassengerCount INT,
        TripDistance NUMERIC(8,2),
        FareAmount NUMERIC(8,2),
        Extra NUMERIC(8,2),
        MtaTax NUMERIC(8,2),
        TipAmount NUMERIC(8,2),
        TollsAmount NUMERIC(8,2),
        EhailFee NUMERIC(8,2),
        ImprovementSurcharge NUMERIC(8,2),
        TotalAmount NUMERIC(8,2),
        PaymentType INT,
        TripType INT,
        CongestionSurcharge NUMERIC(8,2)
    );
    ```

### Taxi Zone Lookup Table

    ```sql
    CREATE TABLE TaxiZoneLookup (
        LocationId INT PRIMARY KEY,
        Borough VARCHAR(100),
        Zone VARCHAR(100),
        ServiceZone VARCHAR(100)
    );
    ```

Then, I copied the `.csv` files into `pgAdmin` and used `pgAdmin` to:

1. Create the tables.
2. Import the CSV files.

Below are the commands used:

    ```sh
    ➜  Week 1 git:(main) ✗ docker cp green_tripdata_2019-10.csv pgadmin:/var/lib/pgadmin/storage/pgadmin_pgadmin.com
    Successfully copied 43.5MB to pgadmin:/var/lib/pgadmin/storage/pgadmin_pgadmin.com
    ➜  Week 1 git:(main) ✗ docker cp taxi_zone_lookup.csv pgadmin:/var/lib/pgadmin/storage/pgadmin_pgadmin.com 
    Successfully copied 14.3kB to pgadmin:/var/lib/pgadmin/storage/pgadmin_pgadmin.com
    ```
