FROM centos:latest

LABEL maintainer "uzresk"

ARG ANSIBLE_VERSION=v2.2.0.0-1

# install jupyter
RUN curl -L https://bootstrap.pypa.io/get-pip.py | python && \
    pip install --upgrade pip
RUN yum -y install python-devel.x86_64 \
                   gcc-c++
RUN pip install jupyter

# install jupyter extension
RUN pip install jupyter_nbextensions_configurator && \
    pip install https://github.com/ipython-contrib/jupyter_contrib_nbextensions/tarball/master && \
    jupyter contrib nbextension install --user && \
    jupyter nbextension enable toc2/main && \
    pip install jupyterthemes

# install ansible
RUN yum -y install git openssl-devel sshpass && \
    pip install cryptography && \
    pip install git+https://github.com/ansible/ansible.git@$ANSIBLE_VERSION && \
    pip install boto && \
    pip install pywinrm 

# install ruby & serverspec
ENV RBENV_ROOT /root/.rbenv
RUN git clone https://github.com/rbenv/rbenv.git $RBENV_ROOT && \
    git clone https://github.com/sstephenson/ruby-build.git $RBENV_ROOT/plugins/ruby-build
RUN yum install -y gcc \
                   bzip2 \
                   openssl-devel \
                   libyaml-devel \
                   libffi-devel \
                   readline-devel \
                   zlib-devel \
                   gdbm-devel \
                   ncurses-devel \
                   make
ENV PATH $PATH:$RBENV_ROOT/bin:$RBENV_ROOT/shims
RUN rbenv install 2.4.0 && \
    rbenv global 2.4.0
RUN gem install bundler && \
    gem install serverspec \
                rspec_junit_formatter

# install other package
RUN yum -y install epel-release \
                   iproute \
                   unzip \
                   java-1.8.0-openjdk.x86_64 \
                   java-1.8.0-openjdk-devel.x86_64
ENV JAVA_HOME $(readlink -f /usr/bin/javac | sed "s:/bin/javac::")

# start jupyter
#RUN mkdir -p /notebooks
#WORKDIR /notebooks
#CMD ["jupyter", "notebook"]
