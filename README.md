# consul tests
### WHAT IS THIS?
- Vagrant file in order to Test consul and consul-template
- Consul config files
### DOCUMENTATION
- https://www.digitalocean.com/community/tutorials/an-introduction-to-using-consul-a-service-discovery-system-on-ubuntu-14-04
- https://www.livewyer.com/blog/2015/02/05/service-discovery-docker-containers-using-consul-and-registrator
- https://github.com/hashicorp/consul-template#examples

### STEPS TO RUN CONSUL CLUSTER
- Login consulserver2 `vagrant ssh consulserver2` and execute `tmux`
- Press ctrl+b+c to create new terminals and ctrl+b+n to move between terminals
- Execute `consul agent -server -bootstrap -data-dir /tmp/consul -bind=192.168.50.2` in order to bootstrap firstnode
- Login in consulserver3 and consulserver4 in order to boot agents `consul agent -server -data-dir /tmp/consul -bind=192.168.0.3` in consulserver3 and `consul agent -server -data-dir /tmp/consul -bind=192.168.0.4` in consulserver4
- Move again to consulserver2 and add consulserver3 and consulserver4 to the cluster with `consul join 192.168.0.3 192.168.0.4`
