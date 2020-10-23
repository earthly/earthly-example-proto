FROM golang:1.13-alpine3.11
WORKDIR /proto-example

proto:
  FROM namely/protoc-all:1.29_4
  COPY api.proto /defs
  RUN --entrypoint -- -f api.proto -l go
  SAVE ARTIFACT ./gen/pb-go /pb AS LOCAL pb
