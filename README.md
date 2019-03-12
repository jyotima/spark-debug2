# spark-debug2

- Machine type Win10 
- Install docker on win10. (I used docker Version 2.0.0.3 (31259))
- mkdir C:\docker\
- mkdir C:\docker\spark
- put these contents in Dockerfile at C:\docker
    ```
    FROM ubuntu:16.04

    ENV SPARK_HOME /spark
    ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

    RUN apt-get update \
      && apt-get install -y wget \
      && apt-get install -y git \
      && apt-get install -y openjdk-8-jdk

    EXPOSE 4000/udp
    EXPOSE 4000/tcp
    ```
- cd C:\docker\spark
- git clone https://github.com/apache/spark.git .
- git checkout -b 2.4.1 tags/v2.4.1-rc5 .
- Open docker settings from system tray -> settings -> shared drives -> select all -> apply -> provide creds in pop up
- cd C:\docker
- docker build . -t spark
- docker run --name sparky --hostname myhost -p 4000:4000 --mount type=bind,source="C:\docker",target=/mount -it spark
- The above command will open up the bash prompt
- ln -s /mount/spark/ $SPARK_HOME
- cd $SPARK_HOME && ./build/mvn -DskipTests clean package
- $SPARK_HOME/bin/spark-shell --driver-java-options -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=4000
- In intellij, remote debug on port 4000
