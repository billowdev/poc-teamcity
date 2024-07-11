# poc-teamcity ci/cd install on linux server

### [TEST] database
1. create user
```
CREATE USER teamcity WITH PASSWORD 'Look123!';
```
2. create database
```
CREATE DATABASE teamcity;
```
3. change database owner
```
ALTER DATABASE teamcity OWNER TO teamcity;
```

### [Installation] 

- manual install team city on the linux

#### [Example: Installation using Ubuntu Linux](https://www.jetbrains.com/help/teamcity/install-teamcity-server-on-linux-or-macos.html#Example%3A+Installation+using+Ubuntu+Linux)


#### install on the docker

1. pull docker `jetbrains/teamcity-server:latest`
2. run docker & mount /data/teamcity_server/datadir to backup config file
   
mount docker volumn
- 1. create volumn

	```bash
	docker volume create teamcity-data
	```
- 2. run docker
	```bash
	docker run -d -p 8111:8111 --name teamcity-server -v teamcity-data:/data/teamcity_server/datadir jetbrains/teamcity-server
	```

4. open host:8111 or the port that you run
5. config the database for the teamcity


### Usage
- https://www.youtube.com/watch?v=zqi4fDF-S60
- todo
1. install teamcity server
   - deploy teamcity at
	[localhost](http://localhost:8111/)

	- admin
	- test1234!
  
2. configure ci pipeline
3. connect build agents
  
   - install agent
   ```bash
   docker run \
    -v teamcity_agent_config:/data/teamcity_agent_conf \
    -e SERVER_URL="http://localhost:8111/" \
    -d jetbrains/teamcity-agent
   ```

   - mount the docker from the host inside the docker (docker-build-agent)
   ```bash
   docker run \
   -v teamcity_agent_config_docker:/data/teamcity_agent/conf \
   -e SERVER_URL="http://localhost:8111/" \
   -v /var/run/docker.sock:/var/run/docker.sock \
   -d jetbrains/teamcity-agent
   ```


4. run or build
   
