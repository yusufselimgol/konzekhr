#Task1.1: Create a simple http server with python. 

> cat ./main.sh
> 
> #/bin/bash
> python3 -m http.server

#Task1.1: Write a systemd unit file.

> cat /lib/systemd/system/http.service
>[Unit]
>Description=Simple http server on port 8000
>
>[Service]
>User=root
>WorkingDirectory=/home/yusuf/work/hr/task1
>ExecStart=/usr/bin/bash /home/yusuf/work/hr/task1/main.sh
>
>StandardOutput=file:/home/yusuf/work/hr/task1/output.log
>StandardError=file:/home/yusuf/work/hr/task1/error.log
>Restart=on-failure
>RestartSec=5s
>
>[Install]
>WantedBy=multi-user.target
>Alias=http.service


> sudo ln -s  /lib/systemd/system/http.service http.service
> systemctl daemon-reload 
> systemctl start http.service
> systemctl enable http.service
> systemctl status http.service
> ● http.service - Simple http server on port 8000
>      Loaded: loaded (/lib/systemd/system/http.service; enabled; vendor preset: enabled)
>      Active: active (running) since Sat 2024-12-28 15:15:53 +03; 2min 14s ago
>    Main PID: 96741 (bash)
>       Tasks: 2 (limit: 6933)
>      Memory: 8.7M
>         CPU: 123ms
>      CGroup: /system.slice/http.service
>              ├─96741 /usr/bin/bash /home/yusuf/work/hr/task1/main.sh
>              └─96742 python3 -m http.server
> 
> Ara 28 15:15:53 sysadmin systemd[1]: Started Simple http server on port 8000.

#tas3

> docker compose up --scale http-server01=2  
>
> ➤ docker ps -a                                                               
> CONTAINER ID   IMAGE                                                 COMMAND                  CREATED             STATUS                           PORTS                                               NAMES
> 5cb6d818348f   nginx:latest                                          "/docker-entrypoint.…"   13 seconds ago      Up 12 seconds                    80/tcp, 0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   task2-nginx-1
> ea72082cdb9a   http-server:v12                                       "/bin/sh ./main.sh"      13 seconds ago      Up 13 seconds                    8000/tcp                                            task2-http-server01-2
> ad1f495dae78   http-server:v12                                       "/bin/sh ./main.sh"      13 seconds ago      Up 13 seconds                    8000/tcp                                            task2-http-server01-1
>
>~ ➤ docker ps -a                                                               
>CONTAINER ID   IMAGE                                                 COMMAND                  CREATED             STATUS                           PORTS                                               NAMES
>5cb6d818348f   nginx:latest                                          "/docker-entrypoint.…"   13 seconds ago      Up 12 seconds                    80/tcp, 0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   task2-nginx-1
>ea72082cdb9a   http-server:v12                                       "/bin/sh ./main.sh"      13 seconds ago      Up 13 seconds                    8000/tcp                                            task2-http-server01-2
>ad1f495dae78   http-server:v12                                       "/bin/sh ./main.sh"      13 seconds ago      Up 13 seconds                    8000/tcp                                            task2-http-server01-1
>~ ➤ curl 127.0.0.1:80                                                                                                                                                                                                                
>curl: (7) Failed to connect to 127.0.0.1 port 80 after 0 ms: Connection refused
>~ ➤ curl 127.0.0.1:8000                                                                                                                                                                                                              
><html>
><head><title>502 Bad Gateway</title></head>
><body>
><center><h1>502 Bad Gateway</h1></center>
><hr><center>nginx/1.23.1</center>
></body>
></html>
>~ ➤ docker ps -a                                                                                                                                                                                                                     
>CONTAINER ID   IMAGE                                                 COMMAND                  CREATED             STATUS                           PORTS                                               NAMES
>5cb6d818348f   nginx:latest                                          "/docker-entrypoint.…"   2 minutes ago       Up 2 minutes                     80/tcp, 0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   task2-nginx-1
>ea72082cdb9a   http-server:v12                                       "/bin/sh ./main.sh"      2 minutes ago       Up 2 minutes                     8000/tcp                                            task2-http-server01-2
>ad1f495dae78   http-server:v12                                       "/bin/sh ./main.sh"      2 minutes ago       Up 2 minutes                     8000/tcp                                            task2-http-server01-1
>~ ➤ docker stop ea72082cdb9a                                                                                                                                                                                                         
>ea72082cdb9a
>~ ➤ docker ps -a                                                                                                                                                                                                                     
>CONTAINER ID   IMAGE                                                 COMMAND                  CREATED             STATUS                           PORTS                                               NAMES
>5cb6d818348f   nginx:latest                                          "/docker-entrypoint.…"   2 minutes ago       Up 2 minutes                     80/tcp, 0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   task2-nginx-1
>ea72082cdb9a   http-server:v12                                       "/bin/sh ./main.sh"      2 minutes ago       Exited (137) 2 seconds ago                                                           task2-http-server01-2
>ad1f495dae78   http-server:v12                                       "/bin/sh ./main.sh"      2 minutes ago       Up 2 minutes                     8000/tcp                                            task2-http-server01-1
>~ ➤ curl 127.0.0.1:8000                                                                                                                                                                                                              
><html>
><head><title>502 Bad Gateway</title></head>
><body>
><center><h1>502 Bad Gateway</h1></center>
><hr><center>nginx/1.23.1</center>
></body>
></html>
>~ ➤                                                                                                                                                                                                                                  
