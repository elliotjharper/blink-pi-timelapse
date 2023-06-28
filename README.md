# blink-pi-timelapse - GOALS

- 1. Install docker
    - Setup pi from scratch
    Used Raspberry pi imager to put "pi os lite legacy" on sd card.
        lite means no desktop   
        legacy means buster instead of bullseye
    In the raspberry pi imager program used the settings to preconfigure wifi.

    - Get raspbian up to date
        `sudo apt update`
        `sudo apt upgrade`

    - install docker
        `curl -sSL https://get.docker.com | sh`

    - add pi user to docker group so that pi can control docker
        `sudo usermod -aG docker $USER`

    - test docker is working
        `docker run hello-world`
        This will tell docker to get a docker image if it does not already have it and then run it and this one logs out a message about that

    - view running containers
        `docker ps`
    
    - view all containers (shows completed too)
        `docker ps -a`

- 2. Run a docker hello world example
    - Ended up being covered by the end of part 1

- 3. Run an existing python image and use it to log out python version and hello world
    - Get official python image from docker hub by using:
        - `docker run python`
        - this caused it to get that image from the registory and then try to just execute it
    - Launch an instance of that image as an interactive shell
        - `docker run -it --rm --name eh-test-script python`
        - `-it` launches that image but connects its terminal to the host so you can interact from here
        - `--name eh-test-script` sets the name for the container as can be observed using `docker ps -a`
        - `--rm` 
            - tells it to cleanup and remove that container after it has run
            - the lack thereof can be observed using `docker ps -a`. 
            - Omitting this will see it remain. This has other significance as the container includes the file system of that container so you can see the final state if it is left. This is useful for debugging....
    - Run arbitrary commands in that python image
        - `docker run -it --rm python ls`
        - `docker run -it --rm python ls`
        - `docker run -it --rm python cat /etc/host.conf`
    - created a python hello world script
        - see `/part-3/hello-world.py`
    - Run a command directly using that image
        - copied script onto the pi at `/home/pi/python-scripts/hello-world.py`
        - got the python exe from inside the container to execute that script
            - `docker run -it --rm -v "$PWD":/usr/eh-test -w /usr/eh-test python:latest python hello-world.py`
                - `-v "$PWD":/usr/eh-test`
                    - this mounts the current ProcessWorkingDir for access inside the container at the path `/usr/eh-test`
                - `-w /usr/eh-test`
                    - this in effect `cd` over to the newly mounted path of `/usr/eh-test`
                - `python:latest python hello-world.py`
                    - `python:latest` is telling docker use that image for this container, everything after this point is a command to run in that container
                    - `python hello-world.py` is just running that command at the command line at the current dir so it runs our test file
    - Setup a dockerfile so that I can run the python image and execute commands multiple times
        - created the well documented file `/part-3/Dockerfile`
            - BIG GOTCHA, only the LAST CMD of a Dockerfile will be executed so if you need to run multiple commands you must create a script (think bash .sh etc..) and use CMD to execute that
        - compiled an image using that dockerfile
            - `docker build -t multiple-python-hellos .`
        - ran the image multiple-python-hellos
            - `docker run -it --rm multiple-python-hellos`

- 3. BONUS
    - Ran nodejs using the official nodejs team docker images
        -  `docker run -it --rm  node:14-bullseye node -v`

- XX. Build a python image (take on the task of installing python) and use it to run the same scripts
    - no point...

- 4. Setup the python script to make use of an env var and then pass that in whilst running it
    - created a basic dockerfile (`/part4/dockerfile`) to build an image that starts with debian and just does some echoing to console using `/part4/main.sh`
    - in the dockerfile created an environment variable with a default value
        - `ENV TestVar "InitialValue"`
    - if you just build and run that container the main.sh logs that initial value
        - build: `docker build -t part-4 .`
        - run: `docker run -it --rm part-4`
    - however you can supply a different environment variable as you launch the container using command line arguments
        - `docker run -it -e "TestVar=NewValue" --rm part-4`
        - this overrides that env variable and the existing echo $TestVar uses that new value

- 5. Build image on top of python image installing blinkpy and log to prove blinkpy installed


- 6. Build configuration into docker setup however that is done and use it to execute blinkpy to get an image




