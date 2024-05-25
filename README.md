# API Documentation with Protobuf, Buf, and Redocly

## Folder Structure

```
myapi/
├── gen/
│   └── openapiv2/
│       └──myapi/
│          └──v1/
│             └──myapi.swagger.json
├── proto/
│   ├── myapi/
|   |   └──v1/
│   │       └── my_service.proto
├── buf.lock
├── buf.yaml
├── buf.work.yaml
├── redocly.yaml
└── README.md
```

## Setting Up Your Environment

### Install Protobuf Compiler

```bash
# On MacOS
brew install protobuf

# On Ubuntu
sudo apt-get install -y protobuf-compiler
```

### Install Buf CLI

```bash
# On MacOS
brew install bufbuild/buf/buf

# On Ubuntu
sudo apt-get install -y buf
```

### Install Redocly CLI

```bash
npm install -g @redocly/openapi-cli
```

## Protobuf Definitions

Create a Protobuf definition file at `proto/myapi/myapi.proto`:

```protobuf
syntax = "proto3";

package myapi.v1;

import "google/api/annotations.proto";

option go_package = "/myapi/v1";

service MyService {
  rpc GetUser (GetUserRequest) returns (GetUserResponse) {
    option (google.api.http) = {
      get: "/users/{user_id}"
    };
  }
}

message GetUserRequest {
  int32 user_id = 1;
}

message GetUserResponse {
  string name = 1;
  string email = 2;
}
```

## Configuring Buf

Create a `buf.yaml` file in your project root:

```yaml
version: v1
name: buf.build/myapi/protos
deps:
  - buf.build/googleapis/googleapis
lint:
  use:
    - DEFAULT
```

Create a `buf.work.yaml` file:

```yaml
version: v1
workspace:
  directories:
    - proto
```

## Linting and Generating Code with Buf

### Lint Protobuf Files

```bash
buf lint
```

### Generate Code

```bash
buf generate
```

## Creating API Documentation with Redocly

### Serving Documentation Locally

```bash
redocly preview-docs gen/openapiv2/**/*.json
```

Open your browser and navigate to `http://localhost:8080` to view the documentation.

### Customizing Documentation

Create a `redocly.yaml` file:

```yaml
theme:
  colors:
    primary: "#5b21b6"
  typography:
    fontFamily: "Arial, sans-serif"
```

## Additional Resources

- [Protocol Buffers Documentation](https://developers.google.com/protocol-buffers)
- [Buf Documentation](https://docs.buf.build)
- [Redocly Documentation](https://redoc.ly/docs/)
- [OpenAPI Specification](https://swagger.io/specification/)
