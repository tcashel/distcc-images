# distcc-images

<https://developers.redhat.com/blog/2019/05/15/2-tips-to-make-your-c-projects-compile-3-times-faster#tip__2__using_a_distcc_server_container>


## Build the image

`docker build -t tripptastick/distcc:fedora34 -f Dockerfile .`

## Push the image

`docker push tripptastick/distcc:fedora34`

This is a private repo, so you'll need to be logged in.

## Run the image

```bash
docker run \
  -p 3632:3632 \
  -p 3633:3633 \
  -d \
  tripptastick/distcc:fedora34
```


## install distcc

```bash
git clone https://github.com/distcc/distcc.git
cd distcc
git checkout v3.4
dnf install autoconf binutils-devel python-devel automake -y
./autogen.sh
./configure
make && make install && make installcheck && update-distcc-symlinks
```
## use distcc

get ip of distcc servers

```bash
export DISTCC_HOSTS="$LOCAL_DISTCC_IP/6 localhost/10"
```
```bash
export DISTCC_HOSTS="localhost/10"
```
the `/5` is the number of cores to use on the remote machine

also prob have to modify this: `vim /usr/local/etc/distcc/hosts`

add to your cmake command:

```bash
		-DCMAKE_C_COMPILER_LAUNCHER="distcc" \
		-DCMAKE_CXX_COMPILER_LAUNCHER="distcc" \
```

