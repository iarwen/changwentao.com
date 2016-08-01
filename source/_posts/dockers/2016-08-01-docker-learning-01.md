title: docker学习-01-安装
date: 2016/08/01 15:49:25
categories:
- docker学习
tags: [docker]
---

学习`docker`一段时间了，有部分常用的服务也转到了docker上，虽然是起了virtualbox运行了个ubuntu，然后再搭建docker环境。
暂时工作还是依赖了windows环境，所以兼学习兼工作的折腾了一段时间。
目前使用过的服务有`register` `mysql` `docker-ui` `git-lab`
从[官方hub](http://hub.docker.com)下载镜像在我的网络里十分慢，所以我一般使用Dockerfile构建镜像。作为补充，也可以从阿里的hub搜索
[阿里hub](https://dev.aliyun.com/search.html)
目前官网能搜到的镜像一般都提供github的Dockerfile连接

## 安装
docker 的安装比较简单，尤其是ubuntu下，官网也提供了方法，具体如下
```
curl -fsSL https://get.docker.com/ | sh
```
安装的是官网最新的发布版
```
docker --version
Docker version 1.11.2, build b9f10c9
```
一个 helloworld
```
docker run -ti alpine:3.4 echo 'hello world'
```
如果你本地没有pull过任何镜像，以上命令会从docker镜像库中下载一个名为alpine，tag为3.4的镜像，这个镜像超级小[<5MB]，很多镜像以此镜像作为base镜像
然后ti的命令表示以终端交互的方式运行后面的命令`echo 'hello world'`

## docker 命令
学习docker的道路不易，单一级命令就很多，二级命令更是繁杂，但按实际使用，常用的组合经常用经常敲就熟了，也不怕记不住，毕竟有一些工具[后面讲]
贴一下一级命令
```
root@ubuntu:~# docker --help
Usage: docker [OPTIONS] COMMAND [arg...]
       docker daemon [ --help | ... ]
       docker [ --help | -v | --version ]

A self-sufficient runtime for containers.

Options:

  --config=~/.docker              Location of client config files
  -D, --debug                     Enable debug mode
  -H, --host=[]                   Daemon socket(s) to connect to
  -h, --help                      Print usage
  -l, --log-level=info            Set the logging level
  --tls                           Use TLS; implied by --tlsverify
  --tlscacert=~/.docker/ca.pem    Trust certs signed only by this CA
  --tlscert=~/.docker/cert.pem    Path to TLS certificate file
  --tlskey=~/.docker/key.pem      Path to TLS key file
  --tlsverify                     Use TLS and verify the remote
  -v, --version                   Print version information and quit

Commands:
    attach    Attach to a running container
    build     Build an image from a Dockerfile
    commit    Create a new image from a container's changes
    cp        Copy files/folders between a container and the local filesystem
    create    Create a new container
    diff      Inspect changes on a container's filesystem
    events    Get real time events from the server
    exec      Run a command in a running container
    export    Export a container's filesystem as a tar archive
    history   Show the history of an image
    images    List images
    import    Import the contents from a tarball to create a filesystem image
    info      Display system-wide information
    inspect   Return low-level information on a container or image
    kill      Kill a running container
    load      Load an image from a tar archive or STDIN
    login     Log in to a Docker registry
    logout    Log out from a Docker registry
    logs      Fetch the logs of a container
    network   Manage Docker networks
    pause     Pause all processes within a container
    port      List port mappings or a specific mapping for the CONTAINER
    ps        List containers
    pull      Pull an image or a repository from a registry
    push      Push an image or a repository to a registry
    rename    Rename a container
    restart   Restart a container
    rm        Remove one or more containers
    rmi       Remove one or more images
    run       Run a command in a new container
    save      Save one or more images to a tar archive
    search    Search the Docker Hub for images
    start     Start one or more stopped containers
    stats     Display a live stream of container(s) resource usage statistics
    stop      Stop a running container
    tag       Tag an image into a repository
    top       Display the running processes of a container
    unpause   Unpause all processes within a container
    update    Update configuration of one or more containers
    version   Show the Docker version information
    volume    Manage Docker volumes
    wait      Block until a container stops, then print its exit code

Run 'docker COMMAND --help' for more information on a command.
```


