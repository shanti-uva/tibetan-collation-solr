# Tibetan Collation for Solr

This project contains an example configset for Solr to show how to add Tibetan Sorting. We use the rules defined in [Tibetan NLP Repository](https://github.com/tibetan-nlp/tibetan-collation/blob/master/rules.txt). 

## Installing

First copy the configset to the Solr server, and finally just create the core using the configset. For example:

```bash
solr create -c tibetan_core -d /opt/solr/server/solr/configsets/bo_configset/
```

### Prerequisites

The configset requires:

* Solr 7.2+

### Using Docker to test

If you have Docker installed you can follow the following steps to run some tests. Run the docker compose included in the docker directory of the repository:

```bash
docker-compose up
```

Copy the configset to the Solr container. If you are running the command inside the docker folder and the container's name
is `docker_solr_1` the command would look like the follwoing:

```bash
docker cp ../bo_configset docker_solr_1:/opt/solr/server/solr/configsets/
```

We can execute commands on the server via `docker exec` command.

```bash
docker exec docker_solr_1 solr create -c tibetan_core -d /opt/solr/server/solr/configsets/bo_configset
```

Now we can visit the URL [http://localhost:8983/solr/#/tibetan_core/documents](http://localhost:8983/solr/#/tibetan_core/documents), to add some test data.


Add the example data located in the file: `data.json` 

```json
{
  "id": "1",
  "name_tibt": "ཆོས"
},
{
  "id": "2",
  "name_tibt": "ཀ"
},
{
  "id": "3",
  "name_tibt": "དཀོན"
},
{
  "id": "4",
  "name_tibt": "རྐ"
}
```

After the data is on the server we can do a query to test the sorting over the `name_tibt` field:

[ http://localhost:8983/solr/tibetan_core/select?q=*:*&sort=name_tibt%20ASC ](http://localhost:8983/solr/tibetan_core/select?q=*:*&sort=name_tibt%20ASC)

You should see the following order:

```
{
responseHeader: {
status: 0,
        QTime: 1,
        params: {
q: "*:*",
   sort: "name_tibt ASC"
        }
                },
response: {
numFound: 4,
          start: 0,
          docs: [
          {
id: "2",
    uid: "2",
    name_tibt: "ཀ"
          },
          {
id: "3",
    uid: "3",
    name_tibt: "དཀོན"
          },
          {
id: "4",
    uid: "4",
    name_tibt: "རྐ"
          },
          {
id: "1",
    uid: "1",
    name_tibt: "ཆོས"
          }
          ]
          }
}
```

## Authors

* [Derik](https://github.com/rderik)
* [Andres](https://github.com/amontano)


## Acknowledgements

* To everyone at [SHANTI](https://github.com/orgs/shanti-uva/people)
* To everyone at [Tibetan NLP](https://github.com/tibetan-nlp)
