FROM ubuntu
CMD ["/bin/bash"]
RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list
RUN sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN sed -i 's@//ports.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list

RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get update -y \
    && apt-get install -y tigervnc-standalone-server tigervnc-common fluxbox eterm xterm git net-tools python-is-python3 python3-numpy ca-certificates scrot nano wget curl \
    && apt-get install -y software-properties-common \
    && add-apt-repository ppa:obsproject/obs-studio \
    && apt-get update -y

RUN sed -i 's@//ppa.launchpadcontent.net@//launchpad.proxy.ustclug.org@g' /etc/apt/sources.list.d/*
RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get update -y \
    && apt-get install -y obs-studio \
    && apt-get install -y ttf-wqy-microhei ttf-wqy-zenhei xfonts-wqy \
    && apt-get install -y language-pack-zh* \
    && apt clean -y \
    && rm -rf /var/lib/apt/lists/*

RUN ALL_PROXY=socks5://172.17.0.1:1080 git clone https://github.com/novnc/noVNC.git /opt/noVNC
RUN ALL_PROXY=socks5://172.17.0.1:1080 git clone https://github.com/novnc/websockify.git /opt/noVNC/utils/websockify
RUN ln -s /opt/noVNC/vnc.html /opt/noVNC/index.html
COPY ./container_startup.sh /opt/container_startup.sh
RUN chmod +x /opt/container_startup.sh
RUN echo "?package(bash):needs=\"X11\" section=\"Custom\" title=\"OBS Screencast\" command=\"obs\"" >> /usr/share/menu/custom && update-menus
ENTRYPOINT ["/opt/container_startup.sh"]
