# Openshift command line tool for interacting with Openshift 3

# Summary

- A Docker build for the oc command line tool, used to control Openshift. For more info, see [Get Started with the CLI](https://docs.openshift.com/enterprise/3.4/cli_reference/get_started_cli.html)

- This image will be used in Bitbucket pipeline to deploy in Openshift

Example `bitbucket-pipelines.yml`
```yaml
image: jhidalgo3/openshift-node-docker:7-alpine
clone:          # the 'clone' section
  depth: 1

options:
  docker: true

pipelines:
  default:
    - step:
        script:
          - yarn install
          - yarn build
          - export IMAGE_NAME=$DOCKER_HUB_USERNAME/react-openshift:$BITBUCKET_COMMIT
          - docker build -t $IMAGE_NAME .
          - docker login --username $DOCKER_HUB_USERNAME --password $DOCKER_HUB_PASSWORD
          - docker push $IMAGE_NAME
          - oc login $OPENSHIFT_HOST --insecure-skip-tls-verify=true --username=$OPENSHIFT_USER --password=$OPENSHIFT_PASSWORD
```


# Requirements

- Docker :whale: - if you are on Mac, checkout the [Docker Toolbox](http://docs.docker.com/mac/step_one/)

# To build the Docker image

- Build the image using docker
```bash
$ docker build -t openshift-node-docker .
```
- Run the container
```bash
$ docker run -ti --rm openshift-node-docker oc version
```
- Optionally add an alias to your local profile so you can run the container as a cli (you may also want to mount kube config or pass env vars...)
```bash
$ alias oc='docker run --rm -ti openshift-node-docker oc'
$ oc version
```

# Alternatively, you can use the Docker Hub automated build

```bash
$ docker pull jhidalgo3/openshift-node-docker
```

### Contributing
File issues in GitHub to report bugs or issue a pull request to contribute.

### Original

https://github.com/danielwhatmuff/openshift-oc-docker
