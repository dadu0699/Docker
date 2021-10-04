```bash
# Download the image of oracle 18c
docker pull dockerhelp/docker-oracle-ee-18c

# Create a container with the downloaded image running on port 1521 and entering bash mode
docker run -p 1521:1521 -it dockerhelp/docker-oracle-ee-18c bash

# If the container had previously been created, just turn on and enter bash mode
docker start <container_id>
docker exec -it <container_id> bash

# Being in bash mode it is necessary to enter sqlplus to create your user
sh post_install.sh
sqlplus
	user: sys as sysdba
	pass: oracle

# Enable permissions in Oracle to create users
alter session set "_ORACLE_SCRIPT"=true;
	
# Create user and give it the necessary permissions
create user <user> identified by <password>;
grant dba to <user>;
```