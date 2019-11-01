# Build private docker image registry   

由于很多公司内部项目部署不能使用公网docker镜像仓库，所以需要在内网中搭建私有镜像仓库    
使用docker和openssl搭建私有docker镜像仓库，服务器操作系统：centos 7.6   

1. 安装[docker](https://docs.docker.com/install/linux/docker-ce/centos/)
2. 拉取docker的registry镜像 
   ```
    $ docker pull registry
   ```
3. 打开 /etc/pki/tls/openssl.cnf文件，在v3_ca后面加上 subjectAltName = IP:(你服务器的ip)
4. 由于docker push命令使用https协议,会遇到http: server gave HTTP response to HTTPS client错误，所以我们首先需要在服务器使用openssl生成证书
   ```
    $ mkdir ~/certs
    $ openssl req -newkey rsa:4096 -nodes -sha256 -keyout /root/certs/domain.key  -x509 -days 365 -out /root/certs/domain.crt
   ```
   证书生成成功后可以在/root/certs文件夹下看到domain.key 和domain.crt两个文件，其中domain.crt就是我们的证书文件
5. 然后把证书复制到docker并重启docker
   ```
    $ mkdir /etc/docker/certs.d/myDockerRegistry:5000
    $ cp /root/certs/domain.crt  /etc/docker/certs.d/myDockerRegistry.com\:5000/ca.crt
    $ systemctl restart docker
   ```
6. 修改服务器host文件
   ```
    192.168.1.191 myDockerRegistry.com
   ```
7. 把认证复制到服务器的ca-bundle.crt文件
   ```
    $ cat /root/certs/domain.crt >> /etc/pki/tls/certs/ca-bundle.crt
   ```
8. 启动docker仓库镜像
   ```
    $ docker run -d -p 5000:5000 --privileged=true -v /root/docker/registry:/var/lib/registry -v /root/certs/:/root/certs  -e    REGISTRY_HTTP_TLS_CERTIFICATE=/root/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/root/certs/domain.key --name docker_registry registry
   ```
9. 然后接下来配置本地证书
   ```
    $ cd /usr/share/ca-certificates/
    $ touch root-ca.crt 
   ```
   将服务器生成的证书内容复制到root-ca.crt中，然后打开/etc/ca-certificates.conf文件将证书路径配置进去,在文件末尾加上 ‘root-ca.crt’
10. 更新证书，重启docker
    ```
      $ sudo update-ca-certificates
      $ sudo systemctl restart docker
    ```
11. 接下来便可以使用 docker pull ... , docker push ...发布拉取镜像了

