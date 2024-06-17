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

# 启动cron服务
CMD cron && tail -f /var/log/cron/cron.log
