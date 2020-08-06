Dockerized R-devel with NoLD option
===================================

Run the `launch-nold.sh` script to download and start the docker image. Press [2] in the launch menu shown to start Rstudio. By default the homedirectory in the host OS is mounted inside the container. If you want to mount `~/test` instead, for example, run the launch script using the below:

`./launch-nold.sh -v ~/test`


