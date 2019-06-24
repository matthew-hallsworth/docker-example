This is a proof of concept to allow us to run an instance of Symfony 3.x in docker containers for development.
The structure of the network locally is one each of the following containers:

- PHP-FPM to process FastCGI requests
- NGINX to handle web requests and pass PHP processing over to the FPM container
- Memcache to handle caching of items
- MySQL to provide database (via the MariaDB implementation)

This work was tested with Docker for Mac, with the following versions:

- Version 2.0.0.3 (31259)
- Engine: 18.09.2
- Compose: 1.23.2 

### How do I get this running?

There is some prework to get this going, which is fairly straightforward.

- Add these files/folders to your project in the root
- Make sure that `composer install` and `yarn install` are run prior
- Make sure you have the correct version of Docker installed as mentioned above
- Make a `var` folder in the project root, that has no folders in it
- Make a `db` folder in the project root, that has nothing in it
- Ensure that you have nothing else running on port 80 (or port 443 if you setup ssl w/ certs) on your host

### Running the install

- Run: `docker-compose -f docker-compose_dev.yml up`
- You should end up seeing a load of logs on the command line from instances named as `fpm_1`, `mysql_1` and `nginx_1` as items are loaded
- Hit control-c in the shell that is running docker up, or run `docker-compose -f docker-compose_dev.yml down --remove-orphans` in another shell window to terminate everything

### Running it in docker swarm / kubernetes

Running it in a swarm on your dev machine is actually fairly straight forward.

You will need to ensure your docker / kubernetes integration is up and running though, importantly.

- In the docker settings (on your taskbar at the top) go into Preferences and then Kubernetes
- Click on the Enable Kubernetes and Deploy by default options and hit apply
- Wait for Kubernetes integration to be installed
- When complete, it should show a confirmation message and the green lights at the bottom of the window for Docker Engine and Kubernetes should both be running

You're ready now to swarm deploy

- Run: `docker stack deploy --compose-file docker-compose_dev.yml default-stack`
- Eventually you should see that your stack is running
- You can see the status of your stack by using `kubectl get services`
- To switch off your stack run: `docker stack rm default-stack`
- Confirm stack is down by checking `kubectl get services` again

