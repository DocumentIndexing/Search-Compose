# searchSwarm

ensure you run the following before you try and start the swarm
```
sudo sysctl -w vm.max_map_count=262144
```
Once running you can access Elastic via the http://127.0.0.1.xip.io/ url
this will need to be changed going forward and put behind a some type of
authentication.

## Secrets
Do this to run you need to create the following secrets that will be used to define access to the rabbitmq.

```
echo "<password>" | docker secret create indexingReceiverPassword -
echo "<password>" | docker secret create indexingPublisherPassword -
echo "<password>" | docker secret create rabbitSecretsPassword -
```

