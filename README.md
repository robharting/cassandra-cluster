# cassandra-cluster

test to create a cassandra cluster in a docker-compose setup. 
so that each app can call its own clustermember. And an attempt to
do cqrs en that via the cluster the updated data is distributed to the
other cluster members

inspiration from here: [medium](https://medium.com/@kayaerol84/cassandra-cluster-management-with-docker-compose-40265d9de076)


See the docker-compose.yml

Start it up:
```docker-compose up```
or
```docker-compose -f docker-compose.yml up```


Status containers: ```docker ps```

Stats: ```docker stats```

Get in the containers:
```$xslt
$ docker exec -it DSE-6_node1 bash
$ docker exec -it DSE-6_node2 bash
$ docker exec -it DSE-6_node3 bash
```

Status cassandra from within a container:
```$xslt
nodetool status
```

Create keyspace from within a container:
```$xslt
$ cqlsh
cqlsh> CREATE KEYSPACE musicDb WITH replication = {'class': 'SimpleStrategy', 'replication_factor' : '3'};
```

Use it: 
```
cqlsh> USE musicDb;
```

Create table: 
```
cqlsh> CREATE TABLE musics_by_genre (
                          genre VARCHAR,
                          performer VARCHAR,
                          year INT,
                          title VARCHAR,
                          PRIMARY KEY ((genre), performer, year, title)
                        ) WITH CLUSTERING ORDER BY (performer ASC, year DESC, title ASC);
 ```

Details table:
```$xslt
DESC TABLE musics_by_genre;
```

Put data into the table:
```
NSERT INTO musics_by_genre (genre, performer, year, title) VALUES ('Rock', 'Nirvana', 1991, 'Smells Like Teen Spirit');
```

Current keyspaces: ```cqlsh> DESC keyspaces;```


