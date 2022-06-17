[![Build Status](https://github.com/philips-software/docker-sonar-scanner/workflows/build/badge.svg)](https://github.com/philips-software/docker-sonar-scanner/actions/)
[![Slack](https://philips-software-slackin.now.sh/badge.svg)](https://philips-software-slackin.now.sh)

# Docker images

This repo will contain docker images with the [Sonar Scanner](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/)

Current versions available:

```
.
├──4 
│  └── Dockerfile
```

## Usage

Images can be found on [https://hub.docker.com/r/philipssoftware/sonar-scanner/](https://hub.docker.com/r/philipssoftware/sonar-scanner/).

``` bash
docker run -v (pwd):/root/src
           -w /root/src
           philipssoftware/sonar-scanner
           sonar-scanner -Dsonar.login=<sonar-login-id-goes-here>
                         -Dsonar.host.url=<sonar-url>
```

The scanner will read the `sonar-project.properties` from your project.

## Content

The images obviously contain sonar, node 16, python 2,3 and java 11, but also two other files:

- `REPO`
- `TAGS`

### REPO

This file has a url to the REPO with specific commit-sha of the build.
Example: 

```
$ docker run philipssoftware/sonar-scanner cat REPO
https://github.com/philips-software/docker-sonar-scanner/tree/facb2271e5a563e5d6f65ca3f475cefac37b8b6c
```

### TAGS

This contains all the similar tags at the point of creation. 

```
$ docker run philipssoftware/sonar-scanner:4 cat TAGS
sonar-scanner sonar-scanner:4 sonar-scanner:4.7 sonar-scanner:4.7.0 sonar-scanner:4.7.0.2747
```

You can use this to pin down a version of the container from an existing development build for production. When using `sonar-scanner:4` for development. This ensures that you've got all security updates in your build. If you want to pin the version of your image down for production, you can use this file inside of the container to look for the most specific tag, the last one.

## Simple Tags

### sonar-scanner
- `sonar-scanner`, `sonar-scanner:4`, `sonar-scanner:4.7`, `sonar-scanner:4.7.0`, `sonar-scanner:4.7.0.2747` [4/Dockerfile](4/Dockerfile)

## Why

> Why do we have our own docker image definitions?

We often need some tools in a container for checking some things. F.e. [jq](https://stedolan.github.io/jq/), [aws-cli](https://aws.amazon.com/cli/) and [curl](https://curl.haxx.se/).
We can install this every time we need a container, but having this baked into a container seems a better approach.

That's why we want our own docker file definitions.

## Known Issues

None

## Issues

- If you have an issue: report it on the [issue tracker](https://github.com/philips-software/docker-sonar-scanner/issues)

## Author

- Jeroen Knoops <jeroen.knoops@philips.com>
- Gertjan Maaas <gertjan.maas@philips.com>

## License

License is MIT. See [LICENSE file](LICENSE.md)

## Philips Forest

This module is part of the Philips Forest.

```
                                                     ___                   _
                                                    / __\__  _ __ ___  ___| |_
                                                   / _\/ _ \| '__/ _ \/ __| __|
                                                  / / | (_) | | |  __/\__ \ |_
                                                  \/   \___/|_|  \___||___/\__|  

                                                                 Infrastructure
```

Talk to the forestkeepers in the `docker-images`-channel on Slack.

[![Slack](https://philips-software-slackin.now.sh/badge.svg)](https://philips-software-slackin.now.sh)
