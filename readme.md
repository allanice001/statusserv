#System monitoring interface with node.js/socket.io back-end.

Here's how to deploy it:

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

- Edit config.json - change paths to telemetry files (could be both abs and rel) and description of their content

- Run the server as a daemon:

```
pm2 start server_status.js
```

The server will run on port 8080. To change goto line 219 in server_status.js

- To monitor performance:

```
pm2 monit
```

- Set up cronjob to run at system startup:

```
@reboot pm2 start /path/to/server_status.js
```

**Description of the config.json file format**

Each defined "system" may have "global" parameters and "subsystems" with their own parameters. 
The only mandatory parameter is "data-file", which points to the telemetry file. 
The format for both the global and subsystem parameters is the same:

```
"param name": [[position_1, position_2], {"color_code": [[range_1_1, range_1_2], [...]], ...}, bool, bool]]
```

The first arg defines the parameter index (or index range, i.e. 
position_2 is not mandatory) in the telemetry file if you read 
it in with the python _string_.split() function following the python 
indexing convention (i.e., starting from zero).

For example, the following piece of code will set up two "good" _ranges_:

```
..."success": [[-83, -77], [10, 17]]...
```

and these will set up two good _values_:

```
..."success": [-83, -77]...
..."success": ["good", "awesome"]...
```

The second arg controls coloring. It could be an empty dict {}, then no coloring will be applied. 
Otherwise the following color codes are available:
```
    "success" - green
    "warning" - yellow
    "danger" - red
    "info" - blue
```
The range for each code is a list with either a (set of) interval(s) 
or a (set of) string(s). In the special case of the "Time stamp" param, 
the ranges are in seconds.

3rd arg - plot parameter values or not.

4th arg - should the whole panel turn red/yellow if this param turns red/yellow.


Refer to the provided config.json for examples