# Ambari Shell in a Box Service
Ambari service for installing and managing Shell in a Box on HDP clusters.
Shell In A Box is a program that implements an in-browser command line shell.
It works on any JavaScript and CSS enabled web browser.

Author: [Mauro Cortellazzi](https://github.com/maocorte)

## Setup

- To download the Shell in a Box service folder, run below

```
VERSION=`hdp-select status hadoop-client | sed 's/hadoop-client - \([0-9]\.[0-9]\).*/\1/'`
sudo git clone https://github.com/maocorte/ambari-shellinabox-service.git  /var/lib/ambari-server/resources/stacks/HDP/$VERSION/services/SHELL_IN_A_BOX   
```
- Restart Ambari

```
#sandbox
service ambari restart

#non sandbox
sudo service ambari-server restart
```

- Then you can click on 'Add Service' from the 'Actions' dropdown menu in the bottom left of the Ambari dashboard and follow the wizard steps.

## Remove service

- To remove the Shell in a Box service:
  - Stop the service via Ambari
  - Unregister the service

```
export SERVICE=SHELL_IN_A_BOX
export PASSWORD=admin
export AMBARI_HOST=localhost
#detect name of cluster
output=`curl -u admin:$PASSWORD -i -H 'X-Requested-By: ambari'  http://$AMBARI_HOST:8080/api/v1/clusters`
CLUSTER=`echo $output | sed -n 's/.*"cluster_name" : "\([^\"]*\)".*/\1/p'`

curl -u admin:$PASSWORD -i -H 'X-Requested-By: ambari' -X DELETE http://$AMBARI_HOST:8080/api/v1/clusters/$CLUSTER/services/$SERVICE

#if above errors out, run below first to fully stop the service
#curl -u admin:$PASSWORD -i -H 'X-Requested-By: ambari' -X PUT -d '{"RequestInfo": {"context" :"Stop $SERVICE via REST"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' http://$AMBARI_HOST:8080/api/v1/clusters/$CLUSTER/services/$SERVICE
```

## TODO

- Improve configurations