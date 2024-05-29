# Hadoop-docker
Hadoop dockerization

```
cd .\Hadoop-docker\
docker build -t malik-chettih-ia/hadoop-docker .
docker run -itd --net=hadoop --name hadoop-master --hostname hadoop-master malik-chettih-ia/hadoop-docker:latest

```