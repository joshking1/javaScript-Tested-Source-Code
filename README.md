# javaScript-Tested-Source-Code

 Source code that has been tested and pushed to the quality code repository for you as a DevOps engineer to clone and move it forward to the next steps. 
 
# Mounting container volumes to the host is important for several reasons:

1. Data persistence: When a container is deleted or recreated, any data stored inside it is lost. By mounting a volume from the host machine to the container, data can be stored on the host machine and persist even if the container is deleted or recreated.

2. Sharing data between containers: Containers can share data by accessing the same mounted volume. This allows for easier communication and data exchange between containers.

3. Backup and recovery: Storing data on the host machine makes it easier to back up and recover in case of data loss or system failure.

4. Flexibility: Mounting volumes to the host allows for greater flexibility in choosing where data is stored and how it is accessed.

Overall, mounting container volumes to the host provides a way to persist data, share data between containers, backup and recover data, and provides flexibility in data storage and access.

# Some help 
 docker run \
  -u root \
  --rm \
  -d \
  -p 8080:8080 \
  -p 50000:50000 \
  --name jenkins \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(which docker):/usr/bin/docker \
  -v /home/jenkins_home:/var/jenkins_home \
  jenkins/jenkins
  
 # each parameter in the given Docker run command does teh following:

-u root: This sets the user to run the container as. In this case, it is set to the root user.

--rm: This tells Docker to automatically remove the container when it is stopped. This is useful for running containers temporarily, such as during testing or development.

-d: This runs the container in detached mode, meaning that it runs in the background and doesn't tie up the terminal session.

-p 8080:8080: This maps the container port 8080 to the host machine port 8080. This allows the container's application to be accessible via a web browser at http://localhost:8080.

-p 50000:50000: This maps the container port 50000 to the host machine port 50000. This port is used for Jenkins agent connections.

--name jenkins: This gives the container a name. In this case, it is named "jenkins".

-v /var/run/docker.sock:/var/run/docker.sock: This mounts the Docker socket file from the host machine to the container. This allows the container to interact with the Docker daemon on the host machine, such as launching new containers.

-v $(which docker):/usr/bin/docker: This mounts the Docker binary from the host machine to the container. This allows the container to use the Docker CLI and interact with the host machine's Docker daemon.

-v /home/jenkins_home:/var/jenkins_home: This mounts the host machine's /home/jenkins_home directory to the /var/jenkins_home directory inside the container. This is where Jenkins stores its configuration and data, allowing it to persist across container restarts.

jenkins/jenkins: This specifies the name of the Docker image to use for the container, in this case the official Jenkins image from the Docker Hub registry.


# Part of the premetheus Code to Configure 

- job_name: 'jenkins'
  metrics_path: /prometheus 
  static_configs:
    - targets: ['52.14.213.34:8080']
    
# The Entire prometheus Configuration file 

global:
  scrape_interval: 15s
  scrape_timeout: 10s
  evaluation_interval: 15s
alerting:
  alertmanagers:
  - follow_redirects: true
    enable_http2: true
    scheme: http
    timeout: 10s
    api_version: v2
    static_configs:
    - targets: []
scrape_configs:
- job_name: prometheus
  honor_timestamps: true
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  follow_redirects: true
  enable_http2: true
  static_configs:
  - targets:
    - 52.14.213.34:9090
- job_name: jenkins
  honor_timestamps: true
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /prometheus/
  scheme: http
  follow_redirects: true
  enable_http2: true
  static_configs:
  - targets:
    - 52.14.213.34:8080
    
# More important information when creating prometheus to collect series metric data from jenkins container 

docker run -d --name prometheus -p 9090:9090 -v /path/to/prometheus-1.yaml:/etc/prometheus/prometheus.yaml prom/prometheus

https://grafana.com/grafana/dashboards/
