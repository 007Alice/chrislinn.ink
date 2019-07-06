# Linux Notes

<!-- ex_nolevel -->

> Currently using Kubuntu


[Linux命令行有这么多的好东西？](https://zhuanlan.zhihu.com/p/30720022)

## Shadowsocks
+ ss-qt5
    ```bash
    sudo add-apt-repository ppa:hzwhuang/ss-qt5
    sudo apt-get update
    sudo apt-get install shadowsocks-qt5
    ```
+ pyss
    ```
    sudo apt install libsodium-dev
    pip install https://github.com/shadowsocks/shadowsocks/archive/master.zip -U
    pip install git+https://github.com/shadowsocks/shadowsocks.git@master
    pip install --upgrade git+https://github.com/shadowsocks/shadowsocks.git@master
    ```
    ```json
    {
        "server":"XXXX服务器地址",
        "server_port":XXXX端口,
        "local_address":"127.0.0.1",
        "local_port":1080,
        "password":"XXXX密码",
        "timeout":60,
        "method":"chacha20-ietf-poly1305",
        "fast_open":false,
        "workers":1
    }
    ```
    ```
    sudo sslocal -c /etc/shadowsocks/jp1.json -d start
    # or $HOME/.shadowsocks/config.json
    ```
    + auto-start
        1. `/etc/rc.local`
            ```
            sudo /usr/local/bin/sslocal -c '/home/skyao/shadowsocks/config.json' -d start
            ```
        2. `/etc/init.d/shadowsocks`
            ```
            sudo nohup nohup -c /etc/shadowsocks/config.json > /etc/shadowsocks/log &
            ```
            ```
            sudo chmod 755 /etc/init.d/shadowsocks
            sudo update-rc.d shadowsocks defaults
            sudo update-rc.d -f shadowsocks remove
            ```
        3. `sudo vim /etc/init.d/sslocal_script`
            ```
            #!/bin/bash
            ### BEGIN INIT INFO
            # Provides:          Shadowsocks-local
            # Required-Start:    $network $local_fs $remote_fs
            # Required-Stop:     $network $local_fs $remote_fs
            # Default-Start:     2 3 4 5
            # Default-Stop:      0 1 6
            # Short-Description: Fast tunnel proxy that helps you bypass firewalls
            # Description:       A secure socks5 proxy, designed to protect your Internet traffic.
            ### END INIT INFO

            # Author: lynnyq <lynnyq@gmail.com>

            name=sslocal
            BIN=/usr/local/bin/sslocal
            conf=/etc/shadowsocks/client.json
            # or $HOME/.shadowsocks/config.json

            start(){
                $BIN -c $conf -d start
                RETVAL=$?
                if [ "$RETVAL" = "0" ]; then
                    echo "$name start success"
                else
                    echo "$name start failed"
                fi
            }

            stop(){
                pid=`ps -ef | grep -v grep | grep -i "${BIN}" | awk '{print $2}'`
                if [[ ! -z $pid ]]; then
                    $BIN -c $conf -d stop
                    RETVAL=$?
                    if [ "$RETVAL" = "0" ]; then
                        echo "$name stop success"
                    else
                        echo "$name stop failed"
                    fi
                else
                    echo "$name is not running"
                    RETVAL=1
                fi
            }

            status(){
                pid=`ps -ef | grep -v grep | grep -i "${BIN}" | awk '{print $2}'`
                if [[ -z $pid ]]; then
                    echo "$name is not running"
                    RETVAL=1
                else
                    echo "$name is running with PID $pid"
                    RETVAL=0
                fi
            }

            case "$1" in
            'start')
                start
                ;;
            'stop')
                stop
                ;;
            'status')
                status
                ;;
            'restart')
                stop
                start
                RETVAL=$?
                ;;
            *)
                echo "Usage: $0 { start | stop | restart | status }"
                RETVAL=1
                ;;
            esac
            exit $RETVAL
            ```
            or
            ```
            #!/bin/bash

            ### BEGIN INIT INFO
            # Provides:          sslocal
            # Required-Start:    $local_fs $remote_fs
            # Required-Stop:     $local_fs $remote_fs
            # Default-Start:     2 3 4 5
            # Default-Stop:      0 1 6
            # Short-Description: shadowsocks client
            # Description:       shadowsocks client
            ### END INIT INFO

            PROG="/usr/local/bin/sslocal"
            CONFIG_FILE="/home/chris/.shadowsocks/config.json"
            PID_FILE="/home/chris/.shadowsocks/sslocal.pid"
            LOG_FILE="/home/chris/.shadowsocks/sslocal.log"
            PROG_ARGS="-c $CONFIG_FILE -d start --pid-file $PID_FILE --log-file $LOG_FILE -q" 

            start() {
                echo "begin start"
                $PROG $PROG_ARGS
                echo "$PROG started"
            }

            stop() {
                echo "begin stop"
                pid=`cat $PID_FILE`
                kill -9 $pid
                echo "$PROG stopped"
            }

            case "$1" in
                start)
                    start
                    exit 0
                ;;
                stop)
                    stop
                    exit 0
                ;;
                reload|restart|force-reload)
                    stop
                    start
                    exit 0
                ;;
                **)
                    echo "Usage: $0 {start|stop|reload}" 1>&2
                    exit 1
                ;;
            esac
            ```
            ```
            sudo chmod 755 /etc/init.d/sslocal_script
            sudo chmod +x /etc/init.d/sslocal_script
            cd /etc/init.d/
            sudo update-rc.d sslocal_script defaults 90
            sudo update-rc.d -f sslocal_script remove
            ```
+ provixy
    + install
        ```
        sudo apt-get install privoxy
        ```
    + config
        ```
        sudo subl /etc/privoxy/config
        ```
        ```
        listen-address localhost:8118
        ...
        forward-socks5t / 127.0.0.1:<PORT> .
        ```
    + `/etc/privoxy/user.action`
        ```
        ```
    + restart
        ```
        sudo privoxy --user privoxy /etc/privoxy/config
        sudo service privoxy restart
        sudo service privoxy start
        sudo service privoxy stop
        sudo service privoxy status
        ???
        [X] sudo systemctl enable privoxy
        [X] sudo systemctl start privoxy
        [X] sudo systemctl restart privoxy
        ```
    + export (`~/.bashrc`? `~/.profile`? `~/.bash_profile`? `/etc/profile`?)
        ```
        function proxy_off(){
            unset http_proxy
            unset https_proxy
            unset APT_CONFIG
            echo -e "proxy off!"
        }

        function proxy_on() {
            sudo service privoxy restart
            sudo service sslocal restart
            export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
            export http_proxy="http://127.0.0.1:8118"
            export https_proxy=$http_proxy
            export APT_CONFIG=~/.apt_proxy.conf
            echo -e "proxy on!"
        }
        ```
    + `/etc/apt/apt.conf`
        ```
        Acquire::http::proxy "http://127.0.0.1:8118/";
        Acquire::https::proxy "https://127.0.0.1:8118/";
        ```
        in case apt doesn't use conf
        ```
        sudo apt-get -c /etc/apt/apt.conf update
        ```
+ gfwlist
    * for provixy: `https://www.igfw.net/archives/1178`
+ GenPAC
    ```
    sudo pip install genpac
    pip install —-upgrade genpac
    genpac --pac-proxy "SOCKS5 127.0.0.1:<PORT>" --gfwlist-proxy="SOCKS5 127.0.0.1:<PORT>" --gfwlist-url=https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt --output="autoproxy.pac” 
    ```

<!-- 
`~/.bashrc`
```
function kproxy() {
  unset http_proxy
  unset https_proxy
  echo -e "proxy off!"
}

function sproxy_ss() {
  export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
  export http_proxy="http://127.0.0.1:8118"
  export https_proxy=$http_proxy
  echo -e "proxy on!"
}

function start_ss() {
  sudo /etc/init.d/sslocal_script restart 
}

function start_goflyway() {
  nohup goflyway -g -k="oo00oowoo00oo" -up="cf://i.lori.science:2082" > ~/.goflyway.log &
}

function sproxy_goflyway() {
  export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
  export http_proxy="http://127.0.0.1:8100"
  export https_proxy=$http_proxy
  echo -e "proxy on!"
}
```
 -->
## [Vim](/notes/vim.md)

## Reverse proxy
### ssh
+ 内网
    ```
    ssh -f -NT -R 7788:localhost:22 username@public_host
    ```
+ 外网
    ```
    ssh -p 7788 username@localhost
    ```

### frp   
* server
    ```
    # frps.ini
    [common]
    bind_port = 7000
    ```
    ```
    sudo cp frps /usr/local/bin/frps
    sudo cp frps.ini /usr/local/bin/frps.ini
    sudo chmod +x /usr/local/bin/frps

    ```
    ```
    sudo vim /etc/init.d/frps
    ```
    ```
    #!/bin/sh -e
    ### BEGIN INIT INFO
    # Provides:          frps
    # Required-Start:    $network $remote_fs $local_fs
    # Required-Stop:     $network $remote_fs $local_fs
    # Default-Start:     2 3 4 5
    # Default-Stop:      0 1 6
    # Short-Description: autostartup of frp for Linux
    ### END INIT INFO

    NAME=frps
    DAEMON=/usr/local/bin/$NAME
    PIDFILE=/var/run/$NAME.pid

    [ -x "$DAEMON" ] || exit 0

    case "$1" in
      start)
          if [ -f $PIDFILE ]; then
            echo "$NAME already running..."
            echo -e "\033[1;35mStart Fail\033[0m"
          else
            echo "Starting $NAME..."
            start-stop-daemon -S -p $PIDFILE -m -b -o -q -x $DAEMON -- "--c /usr/local/bin/frps.ini" || return 2
            echo -e "\033[1;32mStart Success\033[0m"
        fi
        ;;
      stop)
            echo "Stoping $NAME..."
            start-stop-daemon -K -p $PIDFILE -s TERM -o -q || return 2
            rm -rf $PIDFILE
            echo -e "\033[1;32mStop Success\033[0m"
        ;;
      restart)
        $0 stop && sleep 2 && $0 start
        ;;
      *)
        echo "Usage: $0 {start|stop|restart}"
        exit 1
        ;;
    esac
    exit 0
    ```
    ```
    sudo chmod 755 /etc/init.d/frps
    sudo /etc/init.d/frps start    #启动
    sudo /etc/init.d/frps stop     #停止
    sudo /etc/init.d/frps restart  #重启
    ```
    ```
    cd /etc/init.d
    sudo update-rc.d frps defaults 90    #加入开机启动
    sudo update-rc.d -f frps remove  #取消开机启动
    ```
* client
    ```
    # frpc.ini
    [common]
    server_addr = x.x.x.x
    server_port = 7000

    [ssh]
    type = tcp
    local_ip = 127.0.0.1
    local_port = 22
    remote_port = 6000
    ```
    ```
    sudo cp frpc /usr/local/bin/frpc
    sudo cp frpc.ini /usr/local/bin/frpc.ini
    sudo chmod +x /usr/local/bin/frpc

    ```
    ```
    sudo vim /etc/init.d/frpc
    ```
    ```
    #!/bin/sh -e
    ### BEGIN INIT INFO
    # Provides:          frpc
    # Required-Start:    $network $remote_fs $local_fs
    # Required-Stop:     $network $remote_fs $local_fs
    # Default-Start:     2 3 4 5
    # Default-Stop:      0 1 6
    # Short-Description: autostartup of frp for Linux
    ### END INIT INFO

    NAME=frpc
    DAEMON=/usr/local/bin/$NAME
    PIDFILE=/var/run/$NAME.pid

    [ -x "$DAEMON" ] || exit 0

    case "$1" in
      start)
          if [ -f $PIDFILE ]; then
            echo "$NAME already running..."
            echo -e "\033[1;35mStart Fail\033[0m"
          else
            echo "Starting $NAME..."
            start-stop-daemon -S -p $PIDFILE -m -b -o -q -x $DAEMON -- "--c /usr/local/bin/frpc.ini" || return 2
            echo -e "\033[1;32mStart Success\033[0m"
        fi
        ;;
      stop)
            echo "Stoping $NAME..."
            start-stop-daemon -K -p $PIDFILE -s TERM -o -q || return 2
            rm -rf $PIDFILE
            echo -e "\033[1;32mStop Success\033[0m"
        ;;
      restart)
        $0 stop && sleep 2 && $0 start
        ;;
      *)
        echo "Usage: $0 {start|stop|restart}"
        exit 1
        ;;
    esac
    exit 0
    ```
    ```
    sudo chmod 755 /etc/init.d/frpc
    sudo /etc/init.d/frpc start    #启动
    sudo /etc/init.d/frpc stop     #停止
    sudo /etc/init.d/frpc restart  #重启
    ```
    ```
    cd /etc/init.d
    sudo update-rc.d frpc defaults 90    #加入开机启动
    sudo update-rc.d -f frpc remove  #取消开机启动
    ```
+ ssh it
    ```
    ssh -oPort=6000 test@x.x.x.x
    ```

## Git
+ [Commit Msg](https://github.com/conventional-changelog-archived-repos/validate-commit-msg)
    *  Node 插件 validate-commit-msg
    ```
    <type>(<scope>): <subject>
    ```
    + type:
        * feat：新功能（feature）
        * fix：修补bug
        * docs：文档（documentation）
        * style： 格式（不影响代码运行的变动）
        * refactor：重构（即不是新增功能，也不是修改bug的代码变动）
        * test：增加测试
        * chore：构建过程或辅助工具的变动
        * perf
        * build
        * ci
        * revert
        * ...
    + scope
        * commit 影响的范围
        * 数据层、控制层、视图层等
    + 简短描述
        * 以动词开头
        * 第一人称现在时，比如change，而不是 changed 或 changes
        * 第一个字母小写
        * 结尾不加句号
* [git-tips](https://github.com/521xueweihan/git-tips)
* emoji
    - [git-commit-emoji-cn](https://github.com/liuchengxu/git-commit-emoji-cn)
    - [a gist](https://gist.github.com/parmentf/035de27d6ed1dce0b36a)
    - [git-commit-message-convention](https://github.com/kazupon/git-commit-message-convention)
    - [commit-message-emoji](https://github.com/dannyfritz/commit-message-emoji)
    - [gitmoji.carloscuesta.me](https://gitmoji.carloscuesta.me/)
* speed up terminal github ssl:
    - `/etc/hosts`: github.global.ssl.fastly.net
* `--dry-run`
    - to see the result of running it without actually running it
* [cleaning up old remote git branches](https://stackoverflow.com/questions/3184555/cleaning-up-old-remote-git-branches) (or `--system` for system-wide)
    ```
    git config --global fetch.prune true
    ```
    + or per-repo hand-by-hand:
        ```
        git branch -r -d origin/devel
        git remote prune origin
        git fetch origin --prune
        git fetch --prune
        git config remote.origin.prune true
        ```
    + or per-repo config:
        ```
        git config remote.origin.prune true
        ```

## Misc

+ Kubook
    * [Current State of Surfaces](https://www.reddit.com/r/SurfaceLinux/comments/6eau79/current_state_of_surfaces/)
        - The default driver works fine for some, but not for others, to install the Marvell Driver, follow the instructions [here](https://pastebin.com/aBLHBFak)
            + Open a Terminal and install Git via
                ```bash
                sudo apt-get install git
                ```
            + Download the drivers from the Git repository, to do so, run:
                ```bash
                git clone git://git.marvell.com/mwifiex-firmware.git  
                mkdir -p /lib/firmware/mrvl/  
                sudo cp mwifiex-firmware/mrvl/* /lib/firmware/mrvl/
                ```
    * [disable power saving in Network Manager](https://askubuntu.com/questions/1000667/ubuntu-network-driver-crashing-on-wifi)
        - .
            ```bash
            sudo sed -i 's/3/2/' /etc/NetworkManager/conf.d/*
            sudo service network-manager restart
            ```
+ kubuntu alt-space history
    * `~/.config/krunnerrc`
+ lockscreen wallpaper
    * `/usr/share/sddm/themes/breeze`
+ cross-platform
    * `g++ -v`
    - `apt-cache search <keyword>`
    * `apt-cache search mingw`
    * `-ldflags '-linkmode "external" -extldflags "-static"'`
    * sse, sse2, avx, avx512....
        - `gcc ... -march=native`
        - `gcc -dM -E - < /dev/null | egrep "SSE|AVX" | sort`
        - `gcc -march=native -dM -E - < /dev/null | egrep "SSE|AVX" | sort`
        - https://stackoverflow.com/questions/28939652/how-to-detect-sse-sse2-avx-avx2-avx-512-avx-128-fma-kcvi-availability-at-compile
+ `fdisk -l`
+ `/etc/fstab`
+ `/etc/grub.d/40_custom`
+ `/etc/default/grub`
+ `update-grub` 
+ power management
+ restart network interface
    * `sudo /etc/init.d/networking restart `
        - Restart networking (via systemctl): networking.service.
    * `sudo service networking restart` ?
+ go
    * `$HOME/.profile` 
        - PATH
            + `export PATH=$PATH:/usr/local/go/bin` 
        - GOPATH
            + `export GOPATH=$HOME/Miscellaneous/go`
            + `source ~/.profile` 
+ powerline
    * `apt install powerline` 
    * `/usr/share/powerline/config_files/config.json`
        - ["shell"]["theme"] = "default_leftonly"
    * `~/.bashrc`
        ```bash
        POWERLINE_SCRIPT=/usr/share/powerline/bindings/bash/powerline.sh
        if [ -f $POWERLINE_SCRIPT ]; then
          source $POWERLINE_SCRIPT
        fi
        ```
* [konsole-theme](https://github.com/mbadolato/iTerm2-Color-Schemes/tree/master/konsole)
    - Atom
    - Copy the themes from the konsole directory to `$HOME/.config/konsole` (in some versions of KDE, the theme directory may be located at `$HOME/.local/share/konsole`), restart Konsole and choose your new theme from the profile preferences window.
+ Check occupied ports
    ```bash
    netstat -ntl
    ```
+ Kill process by port
    ```bash
    fuser 8080/tcp
    ```
+ Screen
    ```bash
    screen
    ...
    Ctrl_a + Ctrl_d
    screen -ls
    screen -r
    ```
+ nohup
    * `nohup ... > xxx.log &` and press `ENTER`
    * `nohup ... > xxx.log 2>&1 &`
        - `2>&1` edirects stderr to stdout
        - File Descriptor
            + `0`: `STDIN`
            + `1`: `STDOUT`
            + `2`: `STDERR` 
+ restart program
    + bash grep way
        ```bash
        keyw=myprog
        mycmd="echo 1"
        prepare() {
            mkdir -p ./log/
            echo "Restart."
        }
        dosth () {
            prepare
            nohup $(echo $mycmd) > ./log/$(date "+%Y%m%d_%H%M%S").log 2>&1
        }

        while [ : ]
        do
            x=$(ps -ef | grep -v grep | grep -v sudo | grep $keyw | wc -l)
            if [ "$x" -lt 1 ]
            then
                dosth
            fi
            sleep 5s
        done
        ```
    + crontab
    + supervise
- python SimpleHTTPServer 
    ```
    python -m SimpleHTTPServer 8000
    ```
+ Ops
    * Container
        - Docker
            + VM is about emulation, Docker is about isolation.
            + Image
                * An image is an inert, immutable, file that's essentially a snapshot of a container.
                * An instance of an image is called a container.
                * commit vs build
                    - docker commit 是往版本控制系统里提交一次变更。使用这种方式制作镜像，本质上是运行一个基础镜像，然后在基础镜像上进行软件安装和修改。最后再将改动提交到版本系统中。
                        + 难度
                            * 相对容易，适合新手
                        + 文档化
                            * 在通过其他文件来实现
                        + 升级，维护
                            * 后续升级和维护麻烦，需要再次运行镜像并对内部软件进行升级或者安装新软件增加特性
                    - docker build 创建镜像需要编写 Dockerfile
                        + 本质上是运行一个镜像，然后在镜像中按序执行在 Dockerfile 中的命令
                        + 对 Linux 不熟悉的用户相对难，要求有一定的linux和脚本基础知识
                        + 文档化
                            * Dockerfile 本身就是比较好的文档，可读和可理解性比较强
                            * 也可配合其他文档带来详细说明
                        + 升级，维护
                            * 后续升级和维护会相对简单，可以直接在dockerfile中更改并增加新特性
            + mount the folder on the host
                * `docker run -it -v <HOST_FOLDER>:<CONTAINER_FOLDER> <IMAGE>`
            + Docker compose
                + 本地docker容器编排问题
                + 批量创建和销毁容器
                + 编写文件声明好要启动的容器，配置参数(镜像、启动命令、端口映射等)
    * Framework
        - Docker Swarm
            + 解决多主机多个容器调度部署问题
                + 启动容器
                + 监控容器状态
                    + 状态不正常会帮重新启动一个新容器
                + 服务之间的负载均衡
            + 基于docker平台实现的集群技术，通过几条简单的指令快速的创建一个docker集群，接着在集群的共享网络上部署应用，最终实现分布式的服务。
            + swarm技术不成熟，很多配置功能都无法实现，半成品，目前更多的是使用Kubernetes来管理集群和调度容器。
        - Kubernetes
        - Mesos