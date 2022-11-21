FROM docker.io/golang:1.19-alpine as build

ARG COREVER=v1.10.0
ARG UNBVER=v0.0.7

RUN apk add -U unbound-dev git

RUN git clone https://github.com/coredns/coredns.git -b ${COREVER}

RUN cd coredns && echo "unbound:github.com/coredns/unbound" >> plugin.cfg && go generate && go get github.com/coredns/unbound@${UNBVER} 

RUN apk add -U make gcc musl-dev
RUN cd coredns && make CGO_ENABLED=1

from docker.io/alpine:3.16

RUN apk add -U unbound tini

COPY --from=build /go/coredns/coredns /
COPY ./corefile /


ENTRYPOINT ["tini", "--" ]
CMD /coredns -p 5353 -conf /corefile
