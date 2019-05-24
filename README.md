# Docker ➕ musl 4️⃣ lumo

This repo contains the DockerFile necessary for building
[Lumo](https://github.com/anmonteiro/lumo) (the ClojureScript self-hosted
Node.js interpreter) statically, against the [musl
libc](https://www.musl-libc.org/) replacement.

This is particularly important for creating your own [AWS Lambda
runtime](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-custom.html).

More information and detailed instructions on how to achieve that can be found
at [grav/aws-lumo-cljs-runtime](https://github.com/grav/aws-lumo-cljs-runtime).

### Build

After cloning, run:

```shell
cd ami
docker build . -t lumo-musl-ami
```

### Compiling Lumo

First things first, pull down [the docker image](https://cloud.docker.com/repository/docker/arichiardi/lumo-musl-ami):

```shell
docker pull arichiardi/lumo-musl-ami
```

Second, clone `lumo`:

```shell
git clone git@github.com:anmonteiro/lumo   # anywhere on your filesystem
```

Finally, build using the image:

```
docker run -v /path/to/lumo:/lumo -v /home/user/.m2:/root/.m2 -v /home/user/.boot/cache:/.boot/cache --rm lumo-musl-ami
```

The `/root/.m2` and `/.boot/cache` mappings are optional but recommended for
avoiding downloading dependencies multiple times.

The (long) process will compile the lumo static binary under `/path/to/lumo/build`.
