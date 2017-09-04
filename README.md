# docker-image-travis

[![Build Status](https://travis-ci.org/chesszebra/docker-image-travis.svg?branch=master)](https://travis-ci.org/chesszebra/docker-image-travis)

A Travis CI image which can be used to configure Travis from the 
command line.

This repository contains the Docker image used to run the Travis 
CLI binary as described here https://github.com/travis-ci/travis.rb

## Usage

```bash
docker run --rm -it -v $(pwd):/data -v ~:/home/travis chesszebra/travis 
```
