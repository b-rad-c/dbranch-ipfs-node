FROM golang

WORKDIR /dbranch 
COPY ./backend /dbranch/backend

WORKDIR /dbranch/backend
RUN go build main.go && mv main /usr/bin/dbranch

WORKDIR /dbranch
CMD dbranch curator server
