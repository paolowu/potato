FROM base_os:0.2

#安装mongodb
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
RUN echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.2.list
RUN apt-get update
RUN apt-get install -y mongodb-org

#安装redis
RUN apt-get install -y redis-server
RUN pip install redis

RUN pip install pymongo

#安装web框架
RUN pip install django==1.8
RUN pip install django-celery

#安装celery
RUN pip install celery
RUN pip install flower

#安装jupyter
RUN pip install jupyter

RUN apt-get install -y git

RUN apt-get clean

RUN cd /opt && git clone https://code.aliyun.com/idoquant/potato.git \
 && chmod +x potato/docker/start-all.sh \
 && cp potato/docker/start-all.sh potato/operator/trademonitor/start-all.sh

ENV PYTHONPATH=/opt/potato
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/potato/engine/api/ctp/lib/linux:/usr/local/lib
ENV STRATEGYPATH=/opt/potato/stratlib/

# 时区设置
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

WORKDIR /notebook
# clean up
RUN apt-get autoremove -y; \
    apt-get autoclean -y


EXPOSE 8080
EXPOSE 8888
EXPOSE 5555
EXPOSE 27017

ENTRYPOINT cd /opt/potato/operator/trademonitor  && ./start-all.sh
