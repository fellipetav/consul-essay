# CONSUL-ESSAY
A repository for study purposes: an essay using consul to show some of its capabilities.

# PART 1: generating a cluster with 3 servers

## Procedure to create a Cluster containing 3 servers.

### Command to generate the server (expecting to be joined to other N-1 servers) inside the consul number0X container using its inet address (xxx.xx.x.x)
`$ consul agent -server -bootstrap-expect=N -node=consulserver02 -bind=xxx.xx.x.x -data-dir=/var/lib/consul -config-dir=/etc/consul.d`

#### Inside the container where the server was generated you must to join it to the other servers using their respective inet address (xxx.xx.x.x)
`$ consul join xxx.xx.x.x`

### Run the command below to check which members belong to the cluster
`$ consul members`
The output will show which servers are recognized as part of the just generated cluster.


# PART 2: generating a client with 3 servers
The idea is to make this client register our server onto it.

## Procedure to create a Client inside the container where the client agent is
`$ consul agent -bind=xxx.xx.x.x -data-dir=/var/lib/consul -config-dir=/etc/consul.d`
where xxx.xx.x.x is the container inet address.

The trick here is to know that it will recognize you want to creat e client even you pass the "- server" argument or nothing, i.e., `$ consul agent`.

You'll note from the output that, thanks to Consul's "gossip protocol", your Client agent will warn you about not finding any running server.

### After that, you need to join that Client you just created to your previous generated cluster
Inside the Client container, run:
`$ consul join xxx.xx.x.x`
where xxx.xx.x.x is any of the cluster's 3 server's inet address.

To check, run 
`$ consul members`
then you'll see the consul client and the 3 servers connected to your unique node.

![consul member output after connecting the client to some of the cluster's server](image.png)