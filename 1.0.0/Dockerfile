FROM centos:centos7

LABEL maintainer "uzresk"

ARG ANSIBLE_VERSION=v2.7.5

RUN yum -y install epel-release
RUN yum -y install python-devel.x86_64 \
                   gcc-c++ \
                   gcc \
                   bzip2 \
                   openssl-devel \
                   libyaml-devel \
                   libffi-devel \
                   readline-devel \
                   zlib-devel \
                   gdbm-devel \
                   ncurses-devel \
                   make \
                   sshpass \
                   git \
                   openssl-devel

# install ansible
RUN curl -L https://bootstrap.pypa.io/get-pip.py | python && \
    pip install --upgrade pip && \
    pip install cryptography && \
    pip install git+https://github.com/ansible/ansible.git@$ANSIBLE_VERSION && \
    pip install boto && \
    pip install pywinrm 

# install ruby & serverspec
ENV RBENV_ROOT /root/.rbenv
RUN git clone https://github.com/rbenv/rbenv.git $RBENV_ROOT && \
    git clone https://github.com/sstephenson/ruby-build.git $RBENV_ROOT/plugins/ruby-build
ENV PATH $PATH:$RBENV_ROOT/bin:$RBENV_ROOT/shims
RUN rbenv install 2.4.0 && \
    rbenv global 2.4.0
RUN gem install bundler && \
    gem install serverspec \
                rspec_junit_formatter \
                winrm \
                ansible-vault \
                highline \
                nokogiri

# set locale ja_JP.UTF-8, timezone
RUN localedef -v -c -i ja_JP -f UTF-8 ja_JP.UTF-8; echo "";
ENV LANG=ja_JP.UTF-8
RUN rm -f /etc/localtime
RUN ln -fs /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# install uses lego
RUN yum -y install iproute \
                   unzip \
                   java-1.8.0-openjdk.x86_64 \
                   java-1.8.0-openjdk-devel.x86_64

RUN mkdir -p /etc/ansible
COPY ansible.cfg /etc/ansible/ansible.cfg
ENV ANSIBLE_CONFIG /etc/ansible/ansible.cfg


