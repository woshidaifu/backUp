FROM centos:7.9.2009

LABEL maintainer="daifu<pflee@live.com>"
LABEL description="常用 CentOS7 开发基础镜像，适合本地测试、SSH、工具调试使用"

# 切换到可用的 vault 源
RUN sed -i 's|^mirrorlist=|#mirrorlist=|g' /etc/yum.repos.d/CentOS-Base.repo && \
    sed -i 's|^#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-Base.repo

#  安装常用工具包
RUN yum clean all && yum makecache && \
    yum install -y \
    vim curl wget tar zip unzip \
    net-tools bind-utils iproute \
    iputils traceroute telnet \
    tree lsof strace \
    gcc make which \
    yum-utils bash-completion \
    openssh-server openssh-clients \
    python \
    git rsync \
    && yum clean all

#  设置中文 UTF-8 环境
ENV LANG="en_US.UTF-8" \
    LC_ALL="en_US.UTF-8"

#  设置时区为上海
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" > /etc/timezone

#  SSH 配置：生成 key、设置 root 密码
RUN ssh-keygen -A && \
    echo "root:123456" | chpasswd

#  设置欢迎语（motd）
RUN echo -e "\n CentOS 7 测试镜像" > /etc/motd && \
    echo "默认 root 密码：123456，记得安全使用。" >> /etc/motd

#  创建 SSH 目录
RUN mkdir -p /var/run/sshd

#  开放端口
EXPOSE 22

#  容器启动命令
CMD ["/usr/sbin/sshd", "-D"]

