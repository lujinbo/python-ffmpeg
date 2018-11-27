第一层：Python文件夹的Dockefile    

* docker build -t  alpine-python:2.7 .

<br>


第二层：ffmpeg 文件的Dockefile      

* docker build -t  alpine-python-ffmpeg:3.1 .

<br>



第三层：oss文件夹下的Dockerfile     

* docker build -t  alpine-ffmpeg-oss .



目前采用的integration文件夹下的dockerfile，没有分层
