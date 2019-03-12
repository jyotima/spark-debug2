# spark-debug2

1. Install docker on win10. (I used docker Version 2.0.0.3 (31259))
2. Contents of Dockerfile
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
3. On win10, create a folder C:\docker\spark
4. git checkout -b 2.4.1 tags/v2.4.1-rc5
5. Open docker settings from system tray -> settings -> shared drives -> select all -> apply -> provide creds in pop up
6. docker build . -t spark
7. docker run -it spark /bin/bash
8. docker run --name sparky --hostname myhost -p 4000:4000 --mount type=bind,source="C:\docker",target=/mount -it spark
9. The above command will open up the bash prompt
10. ln -s /mount/spark/ $SPARK_HOME
11. cd $SPARK_HOME && ./build/mvn -DskipTests clean package
12. $SPARK_HOME/bin/spark-shell --driver-java-options -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=4000
13. In intellij, remote debug on port 4000
