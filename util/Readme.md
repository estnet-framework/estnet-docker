# Docker Utility Scripts

This directory contains helper scripts to run the docker containers

# Run User Image
To run the docker image with pre-installed ESTNeT use the run_docker_image_with_display script.
For example:
``` shell
'./run_docker_image_with_display docker.pkg.github.com/estnet-framework/estnet/estnet_ubuntu
```

# Developer image
In order use docker for ESTNeT development, use the following script with your local directory containing the ESTNeT code.
``` shell
./run_docker_base_w_local_dir /your/local/ws/path
```
