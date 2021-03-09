# Docker Utility Scripts

This directory contains helper scripts to run the docker containers.

## Docker Login

Make sure that your docker login is correct before executing the scripts.

``` shell
cat ~/TOKEN_GITHUB.txt | sudo docker login https://docker.pkg.github.com -u YOURUSERNAME --password-stdin
```
The `TOKEN_GITHUB.txt` contains a personal access token to your github account.
If you do not have such a token yet, [here](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) is explained how to create one.

## Run User Image
To run the docker image with pre-installed ESTNeT use the run_docker_image_with_display script.
For example:
``` shell
./run_docker_image_with_display docker.pkg.github.com/estnet-framework/estnet/estnet_ubuntu
```

## Developer image
In order use docker for ESTNeT development, use the following script with your local directory containing the ESTNeT code.
``` shell
./run_docker_base_w_local_dir /your/local/ws/path
```
