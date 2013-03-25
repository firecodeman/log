---
layout: post
title: "build and learn hadoop cdh4-2.0.0_4.1.2"
date: 2013-03-25 23:22
comments: true
categories: hadoop
---
1.git clone
{% codeblock git clone %}
git clone git://github.com/cloudera/hadoop-common.git hadoop-common
{% endcodeblock %}

2.switch branch
{% codeblock switch branch %}
cd hadoop-common
git branch 
git checkout cdh4-2.0.0_4.1.2
{% endcodeblock %}

3.mvn package
{% codeblock mvn package %}
mvn package -Pdist -DskipTests -Dtar
{% endcodeblock %}

4.dist
{% codeblock dist %}
cp hadoop-dist/target/hadoop-2.0.0-cdh4.1.2-SNAPSHOT.tar.gz <dist_path>
cd <dist_path>
tar -xvf hadoop-2.0.0-cdh4.1.2-SNAPSHOT.tar.gz
chown -R hadoop:hadoop hadoop-2.0.0-cdh4.1.2-SNAPSHOT
cd hadoop-2.0.0-cdh4.1.2-SNAPSHOT
{% endcodeblock %}

5.ssh空密码登录
{% codeblock rsa config %}
su - hadoop
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
ssh-copy-id hadoop@127.0.0.1 如果是本机则执行 cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys （可能不好使）
chmod 600 ~/.ssh/id_rsa  
{% endcodeblock %}

6. format && start name node
{% codeblock format && start name node %}
./bin/hdfs namenode -format
./bin/hdfs namenode
{% endcodeblock %}

7.start datanode
{% codeblock start datanode %}
./bin/hdfs datanode
hadoop fs -ls /
{% endcodeblock %}

8.export PATH
{% codeblock export PATH %}
cd <hadoop_path>
echo 'export HADOOP_HOME='`pwd` >> ~/.bashrc
echo 'export PATH=$PATH:$HADOOP_HOME/bin' >> ~/.bashrc
{% endcodeblock %}

9.WEB URL
http://localhost:8088/cluster

10.enable remote debug 
{% codeblock enable remote debug %}
vim etc/hadoop/hadoop-env.sh
export HADOOP_NAMENODE_OPTS=$HADOOP_NAMENODE_OPTS" -Xdebug -Xrunjdwp:transport=dt_socket,server=y,address=8765"
{% endcodeblock %}

{% blockquote  http://www.eclipsezone.com/eclipse/forums/t53459.html Remote Debugging with Eclipse %}
Debug As -> Debug Configrations... ->Remote Java Application-New
Name:hadoop-main
Source:add->Archive->Hadooop 3.0.0xxx.jar
Conntcion Type:  Standard(Sockert Attach)
Host:localhost
Port:8765
Allow Termination remote VM:checked

如果报Connection refused 可能是Eclipse使用代理的原因，可以通过 Eclipse下选择window->Preferences->network connections,active provider中选择direct而不要选择manual,保存即可.
{% endblockquote %}


