FROM alpine-python:2.7
MAINTAINER lujinbo@ucse.net

#RUN apk update
#RUN apk add yasm 
#RUN apk add ffmpeg 'asterisk=3.1.11-r1'

RUN echo "https://mirror.tuna.tsinghua.edu.cn/alpine/v3.4/main" > /etc/apk/repositories
RUN apk update
RUN apk add yasm && apk add ffmpeg
