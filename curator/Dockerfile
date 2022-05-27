FROM golang

WORKDIR /dbranch 
COPY ./backend /dbranch/backend

WORKDIR /dbranch/backend
RUN go build main.go
RUN mv main /usr/bin/dbranch

WORKDIR /dbranch
CMD dbranch curator server
