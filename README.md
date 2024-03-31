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

Pour nous, le template sera dans : ~/kafka-zabbix/

![image](https://github.com/crystalloide/kafka-zabbix/assets/48775370/f2824cf4-2d17-4c95-bf49-7b47408b1d86)


### Add Hosts
Go to `Configuration` > `Hosts` > `Create Host`, and add JMX interfaces.

![image](https://github.com/crystalloide/kafka-zabbix/assets/48775370/e89deacb-e97d-44ee-b4f8-afae837b099f)


Then, Link template to this host.
Go to `templates`, and add the template.
![image](https://github.com/crystalloide/kafka-zabbix/assets/48775370/7888ae63-0299-4ace-8fa6-a88241f4f60c)
![image](https://github.com/crystalloide/kafka-zabbix/assets/48775370/2b40fa4e-9d0a-475c-90e0-e6c8ce8efa06)
![image](https://github.com/crystalloide/kafka-zabbix/assets/48775370/de755065-e719-4677-bcc0-a3b85bca660c)


## Grafana configuration
### enable Zabbix plugin
Go to http://localhost:3000/plugins, and select Zabbix plugin.

Click `Enable` button. 

![image](https://github.com/crystalloide/kafka-zabbix/assets/48775370/056e10f3-5d5f-4d0c-a604-d2e414511bc4)


### Add data source
#### mySQL (Optional)
Direct DB connection allows plugin to access Zabbix database directly. This way usually faster than pulling data from zabbix API.

add mySQL as data source 

- Host: `zabbix-db:3306`
- DB name: `zabbix`
- user/pw: `zabbix:zabbix`

![image](https://github.com/crystalloide/kafka-zabbix/assets/48775370/8ec3bd88-d2c9-4a27-bbd3-c31b79007cae)


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

![image](https://github.com/crystalloide/kafka-zabbix/assets/48775370/7454d950-12bf-4c00-989c-007ce0c9d605)


