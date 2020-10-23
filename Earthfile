FROM ubuntu:20.10
WORKDIR /defs

RUN apt-get update && apt-get install -y wget unzip

# setup protoc
RUN wget -O protoc.zip https://github.com/protocolbuffers/protobuf/releases/download/v3.13.0/protoc-3.13.0-linux-x86_64.zip
RUN unzip protoc.zip -d /usr/local/

proto-go:
  RUN apt-get install -y golang git
  ENV GO111MODULE=on
  ENV PATH=$PATH:/root/go/bin
  RUN go get google.golang.org/protobuf/cmd/protoc-gen-go \
         google.golang.org/grpc/cmd/protoc-gen-go-grpc
  COPY api.proto /defs
  RUN mkdir /defs/go-api
  RUN protoc --proto_path=/defs --go_out=/defs/go-api --go-grpc_out=/defs/go-api /defs/api.proto
  SAVE ARTIFACT ./go-api /go-pb AS LOCAL go-pb

proto-py:
  RUN apt-get install -y python3 python3-pip
  RUN pip3 install grpcio grpcio-tools
  COPY api.proto /defs
  RUN mkdir /defs/py-api
  RUN python3 -m grpc_tools.protoc -I /defs --python_out=/defs/py-api --grpc_python_out=/defs/py-api /defs/api.proto
  SAVE ARTIFACT ./py-api /py-pb AS LOCAL py-pb
