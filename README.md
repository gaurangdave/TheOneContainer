

# The One Container

## Why
I use VS Code remote containers a lot for all my development, experiments etc, and found it a easier way to share development environments between my machines and also with other fellow developers. 

## What
That is why I created this one container to rule them all. Basically for now this works perfect for all my pet projects and experiments. 

This container builds a development environment with following platform support, 

- Java
- NodeJS 14
- Mongo DB
- Go Lang 1.16
  - `protoc` 3.15
  - `protoc-gen-go` 1.26
  - `protoc-gen-go-grpc` 1.1
- Typescript
- Angular CLI
- NestJS Cli
- Fibase tools.


## TODO
- Add VSCode plusgins to `.devContainer` files to auto install all required plugs. 


## Usefull Commands

### SSH into Docker Container
- Get list of active containers by running either of following commands, 
  - `docker container ls` or `docker ps`
- SSH into the container by running following commands, 
  - `docker exec -it "Container ID" /bin/bash`


## FAQs

-   How to fix permission issues on VS Code along with dev container?
    - Running the following in host directory and container, this might now be ideal but seems to fix the issue for now. 
    `sudo chmod -R a+rwx .`