FROM ubuntu:20.10
WORKDIR /defs

RUN apt-get update && apt-get install -y wget unzip

# setup protoc
ARG TARGETARCH
RUN \
  if [ "$TARGETARCH" = "amd64" ]; then \
    wget -O protoc.zip https://github.com/protocolbuffers/protobuf/releases/download/v3.13.0/protoc-3.13.0-linux-x86_64.zip ;\
  else \
    wget -O protoc.zip https://github.com/protocolbuffers/protobuf/releases/download/v3.13.0/protoc-3.13.0-linux-aarch_64.zip ;\
  fi
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

proto-rb:
  FROM --platform=linux/amd64 +base
  RUN apt-get install -y ruby
  RUN gem install grpc grpc-tools
  COPY api.proto /defs
  RUN mkdir /defs/rb-api
  RUN grpc_tools_ruby_protoc -I /defs --ruby_out=/defs/rb-api --grpc_out=/defs/rb-api /defs/api.proto
  SAVE ARTIFACT ./rb-api /rb-pb AS LOCAL rb-pb

proto-all:
  BUILD +proto-go
  BUILD +proto-py
  BUILD +proto-rb
