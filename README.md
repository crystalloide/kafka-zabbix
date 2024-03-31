# Kafka-zabbix

Docker-compose file for Apache Kafka metrics monitoring verification using zabbix.

## Containers

This docker-compose contains following containers.
- Apache Kafka
  - [wurstmeister/kafka](https://hub.docker.com/r/wurstmeister/kafka/)
- Zookeeper
  - [wurstmeister/zookeeper](https://hub.docker.com/r/wurstmeister/zookeeper/)
- zabbix
  - [zabbix/zabbix-server-mysql](https://hub.docker.com/r/zabbix/zabbix-server-mysql/tags/)
  - [zabbix/zabbix-java-gateway](https://hub.docker.com/r/zabbix/zabbix-java-gateway/)
- database
  - [mySQL](https://hub.docker.com/_/mysql)
- grafana
  - [grafana/grafana](https://hub.docker.com/r/grafana/grafana/)


[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/crystalloide/kafka-zabbix
)

#### Rappel pour retrouver les environnements éventuellement précédemment instanciés dans Gitpod : https://gitpod.io/workspaces
[# Pour afficher les workspaces déjà instanciés si besoin de faire du ménage :](https://gitpod.io/workspaces)

#### Affichage du répertoire courant dans Gitpod : 

    pwd



## Install

    docker compose up -d

## Access to zabbix and grafana

- zabbix

  http://localhost:8080
  * Default userID and password is `admin/zabbix`.

- grafana

  http://localhost:3000
  * Default userID and password is `admin/admin`.

## Zabbix configuration
### Add template for Kafka JMX
Go to `Configuration` > `Templates` > `Import`, and select `kafka_jmx_templates.xml`.

![add-template](https://github.com/takzok/kafka-zabbix/blob/master/doc/images/zabbix-add-template.png)

### Add Hosts
Go to `Configuration` > `Hosts` > `Create Host`, and add JMX interfaces.

![add-host-jmx](https://github.com/takzok/kafka-zabbix/blob/master/doc/images/zabbix-add-host-jmx.png)

Then, Link template to this host.
Go to `templates`, and add the template.

![add-host-template](https://github.com/takzok/kafka-zabbix/blob/master/doc/images/zabbix-add-host-templates.png)

## Grafana configuration
### enable Zabbix plugin
Go to http://localhost:3000/plugins, and select Zabbix plugin.

Click `Enable` button. 

![add-host-template](https://github.com/takzok/kafka-zabbix/blob/master/doc/images/grafana-enable-plugin.png)

### Add data source
#### mySQL (Optional)
Direct DB connection allows plugin to access Zabbix database directly. This way usually faster than pulling data from zabbix API.

add mySQL as data source 

- Host: `zabbix-db:3306`
- DB name: `zabbix`
- user/pw: `zabbix:zabbix`

![add-mysql](https://github.com/takzok/kafka-zabbix/blob/master/doc/images/grafana-add-mysql.png)

#### Zabbix

Add Zabbix for data source.
- HTTP
  - Host: http://zabbix-web/api_jsonrpc.php
  - Access: Server
- Zabbix API details
  - Username: admin
  - password: zabbix
  - check `trends` on
- Direct DB Connection
  - check `Enable` on
  - Data source: select mySQL name

![add-zabbix](https://github.com/takzok/kafka-zabbix/blob/master/doc/images/grafana-add-zabbix.png)

