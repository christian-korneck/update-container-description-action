# Update Container Description

This github action updates the description of a container repo on [Docker Hub](https://hub.docker.com), [Quay](https://quay.io) or [Harbor](https://goharbor.io) v2 from a README file.

This is useful when building and pushing Docker images with github actions. It can ensure that the description of a container repo stays in sync with the README file in the github repo.

# Background

This action uses [`docker-pushrm`](https://github.com/christian-korneck/docker-pushrm) (speak: *Docker Push Readme*), a commandline tool and Docker CLI plugin for updating container repo docs.

# Example for Docker Hub

- create a secret `DOCKER_PASS` in the github repo's settings
- add to your `.github/workflows/<workflow>.yml`:

```
name: Push README to Docker Hub
on: push
jobs:
  PushContainerReadme:
    runs-on: ubuntu-latest
    name: Push README to Docker Hub
    steps:
      - name: git checkout
        uses: actions/checkout@v2
      - name: push README to Dockerhub
        uses: christian-korneck/update-container-description-action@v1
        env:
          DOCKER_USER: my-user
          DOCKER_PASS: ${{ secrets.DOCKER_PASS }}
        with:
          destination_container_repo: my-user/my-repo
          provider: dockerhub
          short_description: 'my short description 😊'
          readme_file: 'README.md'
```

# Example for Quay

- create an api key token in the quay.io webinterface ([how to create](https://github.com/christian-korneck/docker-pushrm#log-in-to-quay-registry))
- create a secret `APIKEY__QUAY_IO` in the github repo's settings
- add to your `.github/workflows/<workflow>.yml`:

```
name: Push README to Quay.io
on: push
jobs:
  PushContainerReadme:
    runs-on: ubuntu-latest
    name: Push README to Quay.io
    steps:
      - name: git checkout
        uses: actions/checkout@v2
      - name: push README to Quay.io
        uses: christian-korneck/update-container-description-action@v1
        env:
          DOCKER_APIKEY: ${{ secrets.APIKEY__QUAY_IO }}
        with:
          destination_container_repo: quay.io/my-user/my-repo
          provider: quay
          readme_file: 'README.md'
```

# Example for Harbor v2

- create a secret `HARBOR_PASS` in the github repo's settings
- add to your `.github/workflows/<workflow>.yml`:

```
name: Push README to demo.goharbor.io
on: push
jobs:
  PushContainerReadme:
    runs-on: ubuntu-latest
    name: Push README to demo.goharbor.io
    steps:
      - name: git checkout
        uses: actions/checkout@v2
      - name: push README to Dockerhub
        uses: christian-korneck/update-container-description-action@v1
        env:
          DOCKER_USER: my-user
          DOCKER_PASS: ${{ secrets.HARBOR_PASS }}
        with:
          destination_container_repo: demo.goharbor.io/my-project/my-repo
          provider: harbor2
          readme_file: 'README.md'
```

# Reference

## Inputs

### `destination_container_repo`

**Required** the destination container repo    
Example: `my-user/my-repo` or `myserver.com/my-user/my-repo`.

### `provider`

**Optional** repo provider type.

Supported values:
- `dockerhub`
- `quay`
- `harbor2`

Defaults to `dockerhub` when not set.

### `short_description`

**Optional** Sets or updates the repo's short description (max. 100 characters). Only for provider `dockerhub`.

### `readme_file`

**Optional** Path to the source README file    
Example: `./my-README.md`

Defaults to `./README.md` when not set.

## Required env vars

### for providers `dockerhub`, `harbor2`:

- `DOCKER_USER` - username
- `DOCKER_PASS` - password

### for provider `quay`:
- `DOCKER_APIKEY` - quay.io API key (see [here](https://github.com/christian-korneck/docker-pushrm#log-in-to-quay-registry) for how to create)


## Outputs

none (just succeeds or fails)

----
All trademarks belong to their respective owners.