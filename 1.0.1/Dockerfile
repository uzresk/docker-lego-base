FROM centos:centos7

LABEL maintainer "uzresk"

ARG ANSIBLE_VERSION=v2.7.5

RUN yum -y install epel-release \
                   python-devel.x86_64 \
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
                   openssl-devel \
                   iproute \
                   unzip \
                   java-1.8.0-openjdk.x86_64 && \
                   rm -rf /var/cache/yum/* && \
                   yum clean all

# install ansible
RUN curl -L https://bootstrap.pypa.io/get-pip.py | python && \
    pip install --upgrade pip && \
    pip install cryptography && \
    pip install git+https://github.com/ansible/ansible.git@$ANSIBLE_VERSION && \
    pip install boto && \
    pip install pywinrm && \
    pip install shyaml && \
    pip install xlrd && \
    pip install pandas && \
    pip install PyVmomi

# install ruby & serverspec
ENV RBENV_ROOT /root/.rbenv
RUN git clone https://github.com/rbenv/rbenv.git $RBENV_ROOT && \
    git clone https://github.com/sstephenson/ruby-build.git $RBENV_ROOT/plugins/ruby-build
ENV PATH $PATH:$RBENV_ROOT/bin:$RBENV_ROOT/shims
RUN rbenv install 2.4.0 && \
    rbenv global 2.4.0 \
    gem install bundler && \
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

# set ansible.cfg
RUN mkdir -p /etc/ansible
RUN echo $'[defaults] \n\
deprecation_warnings=False\n\
command_warnings=False' > /etc/ansible/ansible.cfg
ENV ANSIBLE_CONFIG /etc/ansible/ansible.cfg


