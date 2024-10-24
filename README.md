# nulix-os

## Fetch sources

```sh
west init -m git@github.com:nulix/nulix-os.git nulix-os
cd nulix-os
west update
```

## Setup environment

```sh
source tools/setup-environment
```

## Build OS

```sh
nulix build os
```

## Flash resulting image

```sh
bzcat out/nulix-os-1.0.0.img.bz2 | sudo dd of=/dev/disk<N> bs=4M iflag=fullblock oflag=direct status=progress
```

## Start local OSTree server

```sh
python3 -m http.server 8000 --directory out/ostree_repo
```

