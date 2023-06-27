# take the official python image from docker hub as a starting point
FROM python:latest

# set the cd within the container to /usr/src
WORKDIR /usr/src

# copy hello-world.py from the CD of the process that is BUILDING this container into the CD within the container
# this is the way that we get that file INTO the image that will later be used to run as a container
# COPY hello-world.py .
# COPY main.sh .

# copy dot dot means copy everything from CD of the process BUILDING the container into the images last set WORKDIR in this DOCKERFILE
COPY . .

# CMD runs a command and lets you pass args as an array
CMD [ "sh", "main.sh"]

# BIG GOTCHA:
# only the LAST CMD of a Dockerfile will be executed 
# if you need to run multiple commands you must create a script (think bash .sh etc..) and use CMD to execute that