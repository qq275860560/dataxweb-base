FROM qq275860560/base

MAINTAINER jiangyuanlin@163.com

# install mysql
RUN  yum -y install  http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm &&\
    yum repolist enabled | grep mysql.*   &&\
    yum -y install mysql-community-server  &&\
    rm -rf /var/cache/yum/*

# init mysql   
RUN curl -fsSL "https://raw.githubusercontent.com/qq275860560/mysql/master/src/main/centos/tmp/init.sh" | sh 
      
# install jenkins
RUN wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo && \
    rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key && \
    yum -y  install  jenkins && yum -y update jenkins &&\
    yum -y clean all && rm -rf /var/cache/yum/*  /tmp/* /var/tmp/*

# init jenkins
RUN mkdir -pv /var/lib/jenkins/ && cd /var/lib/jenkins/ && curl  -O https://raw.githubusercontent.com/qq275860560/jenkins-master/master/src/main/centos/var/lib/jenkins/config.xml &&\
    cd /etc/sysconfig && curl -fsSL -O "https://raw.githubusercontent.com/qq275860560/dataxweb/master/src/main/centos/etc/sysconfig/jenkins"

# install datax
RUN curl -fSL http://datax-opensource.oss-cn-hangzhou.aliyuncs.com/datax.tar.gz | tar xzf - -C /usr/local &&\
    echo 'export DATAX_HOME=/usr/local/datax/' >> /etc/profile && source /etc/profile &&\
    echo 'export PATH=$PATH:$DATAX_HOME/bin'   >> /etc/profile && source /etc/profile &&\
    chmod -R 755 /usr/local/datax

# install maven
RUN wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo &&\
    yum -y install apache-maven
        
# download maven dependency
RUN rm -rf /tmp/dataxweb && cd /tmp &&  git clone https://github.com/qq275860560/dataxweb.git &&\
    cd /tmp/dataxweb && (nohup mvn spring-boot:run &) && ( ps -ef | grep spring-boot | grep -v grep | awk '{print $2}' | xargs kill -9) && rm -rf /tmp/dataxweb