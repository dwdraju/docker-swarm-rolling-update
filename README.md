# Rolling Updates of Docker Services Across Swarm Cluster
Initialize a swarm:
```
docker swarm init
````
Create docker service with 3 replica mapping internal port 80 to 8080 on host:
```
docker service create --name my-app --replicas 3 -p 8080:80 dwdraju/swarm-rolling:v1
```
Checkout [localhost:8080](http://localhost:8080) or ``` curl localhost:8080```. You will see version 1 of the app.

Status of the service can be obtained through:
```
docker service inspect --pretty my-app
```
***
Expand the number of replica:
```
docker service update --replicas=8 my-app
```
***
Do rolling update of all of the replicas with each at once and with delay of 15 seconds:

```
docker service update --update-delay=15s --update-parallelism=1 --image dwdraju/swarm-rolling:v2 my-app
```
Now, keep on refreshing [localhost:8080](http://localhost:8080) or ``` curl localhost:8080``` and you will see two versions randomly with later version replacing the first one gradually.

Checkout the rolling update status:
```
docker service inspect --pretty my-app
```
***
Limit the previous container version on history:
```
docker swarm update --task-history-limit 5
```
Check ``` docker ps -a ``` and you will see only latest 5 releases.