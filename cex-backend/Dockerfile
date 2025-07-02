# 如果是amd芯片需要换成 swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/library/openjdk:8-alpine

# 使用 JDK 8 作为基础镜像
FROM maven:3.8.3-jdk-8 as builder
COPY settings.xml /root/.m2/settings.xml
# 将本地文件复制到容器中
COPY . /build/

# 构建服务1
WORKDIR /build/
RUN mvn clean package -Dmaven.test.skip


# 使用 JDK 8 运行时镜像
FROM openjdk:8-alpine
# 如果使用华为云镜像，取消下面这行的注释并注释掉上面一行
# FROM swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/library/openjdk:8-alpine

# 创建应用目录
RUN mkdir -p /app/cloud /app/uc /app/exchange /app/exchangeapi

# 从构建阶段复制JAR文件
COPY --from=builder /build/cloud/target/*.jar /app/cloud/app.jar
COPY --from=builder /build/ucenter-api/target/*.jar /app/uc/app.jar
COPY --from=builder /build/exchange/target/*.jar /app/exchange/app.jar
COPY --from=builder /build/exchange-api/target/*.jar /app/exchangeapi/app.jar

# 创建启动脚本
RUN echo $'#!/bin/sh\n\
java -Djava.security.egd=file:/dev/./urandom -jar /app/cloud/app.jar\n\
java -Djava.security.egd=file:/dev/./urandom -jar /app/uc/app.jar\n\
java -Djava.security.egd=file:/dev/./urandom -jar /app/exchange/app.jar\n\
java -Djava.security.egd=file:/dev/./urandom -jar /app/exchangeapi/app.jar\n' > /app/start.sh && chmod +x /app/start.sh

ENV TZ=Asia/Shanghai

# 暴露端口（根据实际端口修改）
EXPOSE 6003 6005 7421 6001 6006

# 使用启动脚本作为入口点
ENTRYPOINT ["/bin/sh", "/app/start.sh"]