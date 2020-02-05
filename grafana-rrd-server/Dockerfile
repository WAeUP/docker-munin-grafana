##
# Everything inspired by
#   https://github.com/grafana/grafana/blob/master/Dockerfile
#
# build:
#   docker build -t grafana-rrd .
#
# run like:
#   docker run -t -i -p 9000:9000 --rm grafana-rrd
#
##
# Golang build container
FROM golang:1.13.4-alpine

RUN apk add --no-cache gcc g++ rrdtool-dev git

WORKDIR /tmp/grafana-rrd-server

RUN go get github.com/doublemarket/grafana-rrd-server


# Final container that will run the server
FROM alpine:3.10
LABEL maintainer="Uli Fouquet <uli@waeup.org>"

ENV RRD_STEP="300" \
    RRD_PATHS_DATA="/rrd-data" \
    RRD_PORT="9000"


RUN apk add --no-cache --upgrade --repository=http://dl-cdn.alpinelinux.org/alpine/edge/main rrdtool-dev
RUN mkdir -p /rrd-data

WORKDIR $RRD_PATHS_DATA

COPY --from=0 /go/bin/grafana-rrd-server /bin/grafana-rrd-server

EXPOSE 9000
CMD /bin/grafana-rrd-server -r $RRD_PATHS_DATA -s $RRD_STEP
