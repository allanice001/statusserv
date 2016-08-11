**System monitoring interface with node.js/socket.io back-end.**

This is how to deploy it:

- Install node.js and npm if not in the system

- Install pm2 process manager:

```
	npm install pm2 -g
```

- Clone the repository:

```
	git clone https://github.com/dmitryduev/statusserv.git
```

- cd to the cloned directory and install node.js app dependencies:

```
	npm install
```

- Edit config.json - change paths to telemetry files and description of their content

- Run the server as a daemon:

```
	pm2 start server_status.js
```

- To monitor performance:

```
	pm2 monit
```

- Set up cronjob to run at system startup:

```
    @reboot pm2 start /path/to/server_status.js
```