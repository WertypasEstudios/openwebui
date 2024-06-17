# 使用指定的基础镜像
FROM ghcr.io/open-webui/open-webui:main

# 安装cron
RUN apt-get update && apt-get install -y cron

# 复制cron任务文件
COPY crontab /etc/cron.d/container_cron_job

# 给cron任务文件合适的权限
RUN chmod 0644 /etc/cron.d/container_cron_job

# 应用cron任务
RUN crontab /etc/cron.d/container_cron_job

# 创建必要的目录
RUN mkdir -p /var/log/cron

# 创建并赋予restart_container.sh脚本执行权限
RUN echo '#!/bin/sh\n\
if [ "$(docker ps -a -q -f name=open-webui)" ]; then\n\
    if [ "$(docker ps -q -f name=open-webui)" ]; then\n\
        docker restart open-webui\n\
    else\n\
        docker start open-webui\n\
    fi\n\
else\n\
    docker run -d -p 3000:8080 \\\n\
        -e DATABASE_URL=$DATABASE_URL \\\n\
        -e DEFAULT_USER_ROLE=$DEFAULT_USER_ROLE \\\n\
        -e GID=$GID \\\n\
        -e MODEL_FILTER_LIST=$MODEL_FILTER_LIST \\\n\
        -e OPENAI_API_BASE_URL=$OPENAI_API_BASE_URL \\\n\
        -e OPENAI_API_KEY=$OPENAI_API_KEY \\\n\
        -e UID=$UID \\\n\
        -e WEBUI_SECRET_KEY=$WEBUI_SECRET_KEY \\\n\
        -v open-webui:/app/backend/data \\\n\
        --name open-webui \\\n\
        --restart always \\\n\
        ghcr.io/open-webui/open-webui:main\n\
fi' > /app/restart_container.sh && chmod +x /app/restart_container.sh

# 启动cron服务并保持前台运行
CMD cron && tail -f /var/log/cron/cron.log
