# NULIX OS

## Create docker container

1. Create the container:

```sh
docker run -it \
  -v ~/path/to/nulix-os:/root/nulix-os \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --name nulix-builder \
  nulix/builder:os
```

> ℹ️ Add `-v ~/.ssh:/root/.ssh` to reuse the ssh key from the host.

2. Start the container every other time:

```sh
docker start nulix-builder && docker attach nulix-builder
```

3. Or start it with a different shell:

```sh
docker start nulix-builder && docker exec -it nulix-builder zsh
```

## Initialize virtual environment

```sh
source /nulix-os-venv/bin/activate
```

## Fetch the sources

```sh
cd ~
west init -m git@github.com:nulix/nulix-os.git nulix-os
cd nulix-os
west update
```

## Setup the build environment

```sh
source tools/setup-environment
```

## Build the OS

```sh
nulix build os
```

## Flash the resulting image

> ℹ️ Run the following command on the host. Be sure to use the correct file name with proper version string. Also make sure to use the correct disk name.

```sh
cd ~/path/to/nulix-os/build/deploy/<machine>
bzcat nulix-os-<version>.img.bz2 | sudo dd of=/dev/disk<N> bs=4M iflag=fullblock oflag=direct status=progress
```

## Start the local OSTree server

> ℹ️ Run the following command inside the container.

```sh
cd build/deploy/$MACHINE
python3 -m http.server 8000 --directory ostree_repo
```
