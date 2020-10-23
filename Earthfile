FROM golang:1.13-alpine3.11
WORKDIR /proto-example

proto:
  FROM namely/protoc-all:1.29_4
  COPY api.proto /defs
  RUN mkdir /defs/go-api /defs/py-api
  RUN protoc --proto_path=/defs --go_out=/defs/go-api --python_out=/defs/py-api /defs/api.proto
  SAVE ARTIFACT ./go-api /go-pb AS LOCAL go-pb
  SAVE ARTIFACT ./py-api /py-pb AS LOCAL py-pb
