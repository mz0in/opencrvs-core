# OpenCRVS server setup

This folder contains script to setup a new set of servers for OpenCRVS. It sets up docker swarm and configures the servers to prepare them for a deployment for OpenCRVS.

Ansible is required to run the server setup. This should be installed on your local machine. Also ensure that you have ssh access with the root user to all the server that you are trying to configure.

Run the configuration script with:

```
ansible-playbook -i <inventory_file> playbook.yml -e "dockerhub_username=your_username dockerhub_password=your_password"
```

Replace <inventory_file> with the correct inventory file. These files contain the list of servers which are to be configured. If you are setting up a new set of servers, you will need to create a new file. Some files have been provide for the QA and staging environments.

Once this command is finished the servers are now prepared for an OpenCRVS deployment.

Before the deployment can be done a few secrets need to be manually added to the docker swarm:

ssh into the leader manager and run the following, replacing the values with the actual secrets:

```
printf "<clickatell-user>" | docker secret create clickatell-user -
printf "<clickatell-password>" | docker secret create clickatell-password -
printf "<clickatell-api-id>" | docker secret create clickatell-api-id -
```

Now, from the root folder of the repository run the deployment as follows:

```
yarn deploy <server_hostname> <version>
```

Version can be any git commit hash, git tag or 'latest'