FROM golang

WORKDIR /dbranch 
COPY curator/config.json .
COPY ./backend /dbranch/backend

WORKDIR /dbranch/backend
RUN go build main.go
RUN mv main /usr/bin/dbranch-backend

WORKDIR /dbranch
# set env DBRANCH_CURATOR_CONFIG=<path> to override config below
CMD dbranch-backend --config /dbranch/config.json daemon
