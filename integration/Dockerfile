FROM alpine:3.7
MAINTAINER lujinbo@ucse.net


HEALTHCHECK --interval=5s --timeout=2s --retries=12 \ 
	CMD curl --silent --fail http://0.0.0.0:5000 || exit 1 

# ENV TZ "Asia/Shanghai"
ENV PROJECTDIR /var/www/html/vcservice.xuemao.com
ENV PIPURL "https://pypi.tuna.tsinghua.edu.cn/simple/"


#安装ffmpeg
#RUN echo "https://mirror.tuna.tsinghua.edu.cn/alpine/v3.4/main" > /etc/apk/repositories

RUN echo "https://mirror.tuna.tsinghua.edu.cn/alpine/v3.4/main" > /etc/apk/repositories
RUN apk update
RUN apk add yasm && apk add ffmpeg



#RUN echo http://mirror.tuna.tsinghua.edu.cn/alpine/v3.7/main > /etc/apk/repositories && \
#	echo http://mirror.tuna.tsinghua.edu.cn/alpine/v3.7/community >> /etc/apk/repositories
#设置时区
#RUN apk update && apk add curl bash tree tzdata \
RUN apk update && apk upgrade -a && apk add --no-cache tzdata \
	&& cp -r -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
	&& echo -ne "Alpine Linux 3.4 image. (`uname -rsv`)\n" >> /root/.built

#RUN echo "https://mirror.tuna.tsinghua.edu.cn/alpine/v3.4/main" > /etc/apk/repositories


#安装ffmpeg
#RUN apk add yasm && apk add ffmpeg
#RUN apk update
#RUN apk add --no-cache -X http://mirror.tuna.tsinghua.edu.cn/alpine/v3.4/main yasm 
#RUN apk add --no-cache -X http://mirror.tuna.tsinghua.edu.cn/alpine/v3.4/main ffmpeg 

RUN apk add --update \
    bash \
    python \
    python-dev \
    py-pip \
    mysql-dev \
    build-base \
	vim \
	curl \
  && pip install --upgrade pip -i ${PIPURL} \	
  && pip install Flask -i ${PIPURL}\
  && pip install MySQL-python -i ${PIPURL} \
  && pip install redis -i ${PIPURL}\
  && pip install hiredis -i ${PIPURL}\
  && rm -rf /var/cache/apk/* 
RUN pip install virtualenv -i ${PIPURL}

#安装supervisor工具
RUN pip install supervisor -i ${PIPURL}
RUN	mkdir -p /var/log/supervisor

# 添加 supervisord 的配置文件，并复制配置文件到对应目录下面。（supervisord.conf文件和Dockerfile文件在同一路径）
RUN echo_supervisord_conf > /etc/supervisord.conf

#配置启动项
RUN echo '[program:install_flask]' >> /etc/supervisord.conf && \
	echo 'command= /usr/bin/python /var/www/html/vcservice.xuemao.com/install.py' >> /etc/supervisord.conf && \
	echo 'autostart=true' >> /etc/supervisord.conf && \
	echo 'autorestart=false' >> /etc/supervisord.conf && \
	echo 'user=root' >> /etc/supervisord.conf 


RUN echo '[program:ffmpeg_run]' >> /etc/supervisord.conf && \
	echo 'command= /usr/bin/python /var/www/html/vcservice.xuemao.com/run.py' >> /etc/supervisord.conf && \
	echo 'autostart=true' >> /etc/supervisord.conf && \
	echo 'autorestart=true' >> /etc/supervisord.conf && \
	echo 'user=root' >> /etc/supervisord.conf

RUN echo '[program:ffmpeg_execqueue]' >> /etc/supervisord.conf && \
	echo 'command=/usr/bin/python /var/www/html/vcservice.xuemao.com/app/execQueue.py' >> /etc/supervisord.conf && \
	echo 'autostart=true' >> /etc/supervisord.conf && \
	echo 'autorestart=true' >> /etc/supervisord.conf && \
	echo 'user=root' >> /etc/supervisord.conf

RUN echo '[program:ffmpeg_delroom]' >> /etc/supervisord.conf && \
	echo 'command=/usr/bin/python /var/www/html/vcservice.xuemao.com/app/deleteRoom.py' >> /etc/supervisord.conf && \
	echo 'autostart=true' >> /etc/supervisord.conf && \
	echo 'autorestart=true' >> /etc/supervisord.conf && \
	echo 'user=root' >> /etc/supervisord.conf

RUN echo '[program:ffmpeg_uploadroom]' >> /etc/supervisord.conf && \
	echo 'command=/usr/bin/python /var/www/html/vcservice.xuemao.com/app/UploadRoom.py' >> /etc/supervisord.conf && \
	echo 'autostart=true' >> /etc/supervisord.conf && \
	echo 'autorestart=true' >> /etc/supervisord.conf && \
	echo 'user=root' >> /etc/supervisord.conf


RUN touch ~/.bashrc && \
  echo "alias ll='ls -l --color=tty'"  >> /etc/profile && \
  echo "source /etc/profile " >> ~/.bashrc && \
  echo "47.93.177.100 afterservice.xuemao.com"  >> /etc/hosts

#安装oss
RUN  pip install oss2 -i ${PIPURL}

# 执行supervisord来同时执行多个命令，使用 supervisord 的可执行路径启动服务。  
CMD ["supervisord","-n"] 
