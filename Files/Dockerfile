FROM centos:latest


RUN dnf install python36 -y

RUN pip3 install sklearn

RUN pip3 install pandas

RUN mkdir cnf/

COPY cnf/ /cnf/

WORKDIR /cnf/

RUN python3 KNN.py > result

CMD bash
