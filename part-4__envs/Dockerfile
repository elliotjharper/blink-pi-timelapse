FROM debian:stable

# cd inside the container to path
WORKDIR /usr/src

# copy from building terminal cd into cd of container (copies the main.sh)
COPY ./main.sh .

# seed an environment variable into the image for use when the container is run
ENV TestVar "InitialValue"

#CMD ["echo", "test"]
CMD ["sh", "main.sh"]