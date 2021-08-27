## 1 根据DockerFile生成镜像
进入DockerFile所在的目录，docker build -t 镜像名:tag .  

## 2 docker 镜像的上传和拉取
上传：docker push 镜像名:tag
拉取：docker pull 镜像名:tag  

## 3 镜像的保存和加载
保存：docker save -o 文件名.tar 镜像名:tag  
加载：docker load -i 文件名.tar 或者 docker load < 文件名.tar

## 3 启动一个镜像对应的容器
docker run -it --gpus all --net=host --shm-size 10g -v /export2/zhangxuying/:/zhangxuying/ pytorch/pytorch:1.6.0-cuda10.1-cudnn7-devel /bin/bash  

## 4 从容器创建一个新的镜像
docker commit -a "作者名" -m "镜像备注" 容器id  新镜像名:tag     


