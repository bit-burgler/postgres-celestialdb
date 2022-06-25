<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/29/Postgresql_elephant.svg/640px-Postgresql_elephant.svg.png" alt="APP" style="zoom:30%;" />

<h4 align=center style='color: white'>Build a Celestial Bodies Database with PostgreSQL</h4>

<h6 align=center style='color: white'>freeCodeCamp Relational Databases (Beta)</h6>

<h6 align=center style='color: white'>Certification Project</h6>



### Supercluster Table

###### Overview

* Primary Key - <code style='color: orange'>supercluster_id</code>

###### Table Description

```postgresql
Table "public.supercluster"
+-----------------+---------+-----------+----------+-------------------------------------------------------+
|     Column      |  Type   | Collation | Nullable |                        Default                        |
+-----------------+---------+-----------+----------+-------------------------------------------------------+
| supercluster_id | integer |           | not null | nextval('supercluster_supercluster_id_seq'::regclass) |
| name            | text    |           |          |                                                       |
| span_mly        | numeric |           |          |                                                       |
+-----------------+---------+-----------+----------+-------------------------------------------------------+
Indexes:
    "supercluster_pkey" PRIMARY KEY, btree (supercluster_id)
Referenced by:
    TABLE "cluster" CONSTRAINT "cluster_supercluster_id_fkey" FOREIGN KEY (supercluster_id) REFERENCES supercluster(supercluster_id)
```

###### Table records

```postgresql
+-----------------+------------------------+----------+
| supercluster_id |          name          | span_mly |
+-----------------+------------------------+----------+
|               1 | Caelum Supercluster    |      910 |
|               2 | Saraswati Supercluster |      652 |
|               3 | Bootes Supercluster    |      620 |
|               4 | Laniakea Supercluster  |      520 |
|               5 | Draco Supercluster     |      410 |
|               6 | Virgo Supercluster     |      110 |
|               8 | Coma Supercluster      |       20 |
+-----------------+------------------------+----------+
(7 rows)
```



### Cluster Table

###### Overview

* Primary Key - <code style='color: orange'>cluster_id</code>
* Foreign Key - <code style='color: magenta'>supercluster_id</code> <code>REFERENCES</code> <code style='color: orange'>supercluster_id</code><code>IN</code>`supercluster.tbl`

###### Table Details

```postgresql
Table "public.cluster"
+-----------------+---------+-----------+----------+---------------------------------------------+
|     Column      |  Type   | Collation | Nullable |                   Default                   |
+-----------------+---------+-----------+----------+---------------------------------------------+
| cluster_id      | integer |           | not null | nextval('cluster_cluster_id_seq'::regclass) |
| supercluster_id | integer |           | not null |                                             |
| name            | text    |           |          |                                             |
| catalogue_id    | text    |           |          |                                             |
+-----------------+---------+-----------+----------+---------------------------------------------+
Indexes:
    "cluster_pkey" PRIMARY KEY, btree (cluster_id)
Foreign-key constraints:
    "cluster_supercluster_id_fkey" FOREIGN KEY (supercluster_id) REFERENCES supercluster(supercluster_id)
Referenced by:
    TABLE "galaxy" CONSTRAINT "galaxy_cluster_id_fkey" FOREIGN KEY (cluster_id) REFERENCES cluster(cluster_id)
```

###### Table Records

```postgresql
+------------+-----------------+--------------------+--------------+
| cluster_id | supercluster_id |        name        | catalogue_id |
+------------+-----------------+--------------------+--------------+
|          1 |               4 | Local Group        |              |
|          2 |               6 | Virgo Cluster      |              |
|          3 |               8 | Coma Cluster       |              |
|          4 |               8 | Leo Cluster        |              |
|          5 |               6 | Ursa Major Cluster |              |
|          6 |               6 | M51 Group          |              |
+------------+-----------------+--------------------+--------------+
(6 rows)
```



### Galaxy Table

###### Overview

* Primary Key - <code style='color: orange'>galaxy_id</code>
* Foreign Key - <code style='color: magenta'>cluster_id</code> <code>REFERENCES</code> <code style='color: orange'>cluster_id</code><code>IN</code>`cluster.tbl`

###### Table Details

```postgresql
Table "public.galaxy"
+------------+---------+-----------+----------+-------------------------------------------+
|   Column   |  Type   | Collation | Nullable |                  Default                  |
+------------+---------+-----------+----------+-------------------------------------------+
| galaxy_id  | integer |           | not null | nextval('galaxy_galaxy_id_seq'::regclass) |
| cluster_id | integer |           | not null |                                           |
| name       | text    |           |          |                                           |
+------------+---------+-----------+----------+-------------------------------------------+
Indexes:
    "galaxy_pkey" PRIMARY KEY, btree (galaxy_id)
Foreign-key constraints:
    "galaxy_cluster_id_fkey" FOREIGN KEY (cluster_id) REFERENCES cluster(cluster_id)
Referenced by:
    TABLE "star" CONSTRAINT "star_galaxy_id_fkey" FOREIGN KEY (galaxy_id) REFERENCES galaxy(galaxy_id)
```

###### Table Records

```postgresql
+-----------+------------+------------------------+
| galaxy_id | cluster_id |          name          |
+-----------+------------+------------------------+
|         1 |          1 | Milky Way              |
|         2 |          1 | Andromeda Galaxy       |
|         3 |          6 | Whirlpool Galaxy       |
|         4 |          1 | Traingulum Galaxy      |
|         5 |          1 | Small Magellanic Cloud |
|         6 |          1 | NGC 6946               |
+-----------+------------+------------------------+
(6 rows)
```



### Star Table

  

###### Table Details

```postgresql
Table "public.star"
+---------------+---------+-----------+----------+---------------------------------------+
|    Column     |  Type   | Collation | Nullable |                Default                |
+---------------+---------+-----------+----------+---------------------------------------+
| star_id       | integer |           | not null | nextval('star_star_id_seq'::regclass) |
| galaxy_id     | integer |           | not null |                                       |
| binary_system | boolean |           |          |                                       |
+---------------+---------+-----------+----------+---------------------------------------+
Indexes:
    "star_pkey" PRIMARY KEY, btree (star_id)
Foreign-key constraints:
    "star_galaxy_id_fkey" FOREIGN KEY (galaxy_id) REFERENCES galaxy(galaxy_id)
Referenced by:
    TABLE "planet" CONSTRAINT "planet_star_id_fkey" FOREIGN KEY (star_id) REFERENCES star(star_id)
```

###### Table Records

```postgresql
+---------+-----------+---------------+------------------+
| star_id | galaxy_id | binary_system |       name       |
+---------+-----------+---------------+------------------+
|       1 |         1 | t             | Alpha Centauri   |
|       2 |         1 | f             | Proxima Centauri |
|       5 |         1 | f             | The Sun          |
|       6 |         1 | f             | Barnards Star    |
|       7 |         1 | f             | Sirius           |
|       8 |         1 | f             | Canopus          |
+---------+-----------+---------------+------------------+
(6 rows)
```



### Planet Table



###### Table Records

```postgresql
+-----------+---------+-----------+----------+-----------------+-----------------+-----------------+
| planet_id | star_id | habitable | no_moons | av_surface_temp |      name       | distance_to_sun |
+-----------+---------+-----------+----------+-----------------+-----------------+-----------------+
|         1 |       5 | f         |        0 |             452 | Mercury         |             0.4 |
|         2 |       5 | f         |        0 |             737 | Venus           |             0.7 |
|         5 |       5 | t         |        1 |             288 | Earth           |               1 |
|         6 |       5 | f         |        2 |             210 | Mars            |             1.5 |
|         7 |       5 | f         |       79 |             165 | Jupiter         |             5.2 |
|         8 |       5 | f         |       83 |              93 | Saturn          |             9.5 |
|         9 |       5 | f         |       27 |              49 | Uranus          |            19.8 |
|        10 |       5 | f         |       14 |              55 | Neptune         |              30 |
|        11 |       1 | f         |          |                 | Proxima b       |           0.049 |
|        12 |       1 | f         |          |                 | Proxima c       |            1.49 |
|        13 |       1 | f         |          |                 | Proxima d       |            0.03 |
|        14 |       6 | f         |          |                 | Barnards Star b |             0.4 |
+-----------+---------+-----------+----------+-----------------+-----------------+-----------------+
(12 rows)
```



> Compiled by Yousha Shoaib
>
> GitHub - bit-burgler
>
> https://github.com/bit-burgler
