sudo docker <br/>
容器管理：<br/>
docker ps /*正在运行的容器*/<br/>
docker ps -a<br/>
docker ps -q /*container ID*/<br/>
docker ps -aq<br/>
run -it name bash <br/>
run -i /*保持容器打开*/<br/>
run -t /*为容器分配一个虚拟终端，启动容器后自动进入容器终端*/<br/>
run -d /*容器在后台运行（后台服务进程）*/<br/>
docker rm /*remove*/<br/>
docker rm 6 -f /*强制删除*/<br/>
docker -d<br/>
docker stop/start<br/>
docker attach <br/>
docker exec -t -i name bash /*可进入容器的bash, 输入exit退出后，容器仍在后台运行*/<br/>
docker cp c81:123.txt -/dockerfile/123.txt /*容器拷贝到本机*/<br/>
cd ~ /*home*/<br/>
docker images /*镜像*/<br/>
docker rmi name<br/>

dockerfile <br/>


进入hadoop集群<br/>
<pre="code">
$ cd bigdata-class2
$ cd hadoop-cluster-docker
$ sudo ./start-container.sh
$ ./start-hadoop.sh
</pre>
