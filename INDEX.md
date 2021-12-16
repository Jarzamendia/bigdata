# Instalando Maven
wget https://downloads.apache.org/maven/maven-3/3.8.4/binaries/apache-maven-3.8.4-bin.tar.gz
tar xvf apache-maven-3.8.4-bin.tar.gz -C /usr/lib/

vi /etc/profile

M2_HOME="/usr/lib/apache-maven-3.8.4"
export M2_HOME

M2="$M2_HOME/bin"
MAVEN_OPTS="-Xms256m -Xmx512m"
export M2 MAVEN_OPTS

PATH=$M2:$PATH
export PATH

. /etc/profile

# Instalando e buildando Ambari
wget https://www-eu.apache.org/dist/ambari/ambari-2.7.6/apache-ambari-2.7.6-src.tar.gz (use the suggested mirror from above)
tar xfvz apache-ambari-2.7.6-src.tar.gz
cd apache-ambari-2.7.6-src
mvn versions:set -DnewVersion=2.7.6.0.0
 
pushd ambari-metrics
mvn versions:set -DnewVersion=2.7.6.0.0
popd

mvn -B clean install rpm:rpm -DnewVersion=2.7.6.0.0 -DbuildNumber=388e072381e71c7755673b7743531c03a4d61be8 -DskipTests -Dpython.ver="python >= 2.6"

yum install ambari-server*.rpm

ambari-server setup

# Configurar Mysql
mysql -u root -p

create database ambu;

mysql -u root -p ambu < /var/lib/ambari-server/resources/Ambar-DDL-Create_mysql.sql

/etc/init.d/ambari-server start



https://linuxize.com/post/how-to-install-yarn-on-centos-7/
https://www.centlinux.com/2018/12/install-apache-maven-3-on-centos-7.html
https://wiki.centos.org/HowTos/SetupRpmBuildEnvironment
https://cwiki.apache.org/confluence/display/AMBARI/Installation+Guide+for+Ambari+2.7.6
https://medium.com/@anuketjain007/install-and-configure-ambari-server-with-mysql-in-centos-rhel-7-6-57b24ba18836