FROM ubuntu:14.04

MAINTAINER chenjinxin <chenjinxin@chenjinxin1124.cn>

WORKDIR /root

# install openssh-server
RUN apt-get update && apt-get install -y openssh-server

# ssh without key
RUN ssh-keygen -t rsa -f ~/.ssh/id_rsa -P '' && \
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

CMD [ "sh", "-c", "service ssh start; bash"]