
### Question 1. Understanding docker first run

1. Docker Command
2. Image Download
3. Checking `pip` version


```
➜  DataEnggZoomCamp git:(main) docker run -it python:3.12.8 bash
Unable to find image 'python:3.12.8' locally
3.12.8: Pulling from library/python
e474a4a4cbbf: Pull complete
d22b85d68f8a: Pull complete
936252136b92: Pull complete
94c5996c7a64: Pull complete
c980de82d033: Pull complete
e05e1469c731: Pull complete
ded9ddaf4f92: Pull complete
Digest: sha256:5893362478144406ee0771bd9c38081a185077fb317ba71d01b7567678a89708
Status: Downloaded newer image for python:3.12.8
```

```
root@44b59ae3af80:/# pip --version
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)
```

### Question :2 Understanding Docker networking and docker-compose

Use the docker-compose.yml to create the postgresql and the pgadmin image.

```

➜  Week 1 git:(main) ✗ docker-compose up -d
[+] Running 5/5
 ✔ Network week1_default      Created                                                                                                                                       0.0s 
 ✔ Volume "vol-pgdata"        Created                                                                                                                                       0.0s 
 ✔ Volume "vol-pgadmin_data"  Created                                                                                                                                       0.0s 
 ✔ Container pgadmin          Started                                                                                                                                       0.2s 
 ✔ Container postgres         Started                                       
```

For the data ingestion, I created belwo tables by logged into the `pgadmin`

```

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

Taxi_zone_lookup Table:

```
CREATE TABLE TaxiZoneLookup (
    LocationId INT PRIMARY KEY,
    Borough VARCHAR(100),
    Zone VARCHAR(100),
    ServiceZone VARCHAR(100)
);
```

Then I does copy the .csv files into the pgAdmin, and using the pgAdmin, 
1. I does create teh table and 
2. then import the csv

Below are the commands used

```sql
➜  Week 1 git:(main) ✗ docker cp green_tripdata_2019-10.csv pgadmin:/var/lib/pgadmin/storage/pgadmin_pgadmin.com
Successfully copied 43.5MB to pgadmin:/var/lib/pgadmin/storage/pgadmin_pgadmin.com
➜  Week 1 git:(main) ✗ docker cp taxi_zone_lookup.csv pgadmin:/var/lib/pgadmin/storage/pgadmin_pgadmin.com 
Successfully copied 14.3kB to pgadmin:/var/lib/pgadmin/storage/pgadmin_pgadmin.com
```

