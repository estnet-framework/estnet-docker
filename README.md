# ESTNeT Docker Support
This repository supplies tools and dockerfiles for development on ESTNeT in a docker container.

For help with running the containers, scripts are provided in [util](https://github.com/estnet-framework/estnet-docker/tree/main/util) for e.g. mounting local directories into the container and run containers with the visualization. 


ESTNeT comes with two different variations for docker support: 
- One docker image provides the base for development on ESTNeT. It contains a precompiled OMNeT++ with INET and OSGEarth. This basic image can be used for contributing to ESTNeT. Developers can keep their local workspace, containing the ESTNeT sources, and mount and run it in the docker container. 
- Another image extends the base version and comes with a compiled ESTNeT and [ESTNeT's template project](https://github.com/estnet-framework/estnet-template). This target users of ESTNeT, having their own project (e.g. based on [ESTNeT's template project](https://github.com/estnet-framework/estnet-template)) and only need to link the compiled ESTNeT library. In order to get started, just start OMNET++'s IDE by the command `omnetpp`. Import the three projects that are in `/workspace` via File->Import->General->Existing Projects Into Workspace and you are ready to go. In the template project, simulations/omnetpp.ini is provided as example simulation. It can be started via rightclick->Run As->OMNET++ Simulation.



In `dockerfiles/`, the files for docker image creation and maintainance are provided.


The `util/` folder contains scripts to simply pull and run the provided docker packages and mount e.g. local paths into the container.

