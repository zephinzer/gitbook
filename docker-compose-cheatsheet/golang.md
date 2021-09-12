# ▶️ Golang

## Installation on Ubuntu

```bash
sudo add-apt-repository ppa:longsleep/golang-backports;
sudo apt update;
sudo apt install golang-go;
```

## System Checks

**Environment Checking**

```text
go env
```

## Development

### **Installing Dependencies**

```text
go mod vendor -v;
```

### **Updating Dependencies**

```text
go mod tidy -v;
```

### **Running Commands**

```text
go run ./cmd/command;
```

### **Running All Tests**

```bash
go test ./...
```

### **Running All Tests in a Directory**

```bash
go test ./path/to/directory/...
```

### **Running All Tests with Coverage**

```text
go test -cover -coverprofile c.out ./...;
```

## Building

The following formulae expects that an invoker runs these from the root directory given a project layout as specificied at [https://github.com/golang-standards/project-layout](https://github.com/golang-standards/project-layout).

### **Basic Build**

```text
go build -o ./bin/command ./cmd/command
```

### **Static Build**

```text
CGO_ENABLED=0 \
  go build \
  -ldflags "-extldflags 'static'" \
  -o ./bin/command \
  ./cmd/command;
```

### **Symbols Stripped Build**

```text
go build \
  -ldflags "-s -w" \
  -o ./bin/command \
  ./cmd/command;
```

### **Cross-Platform Build**

```text
GOOS=windows GOARCH=386 \
  go build \
  -o ./bin/command \
  ./cmd/command;
```

### **Variable Injection Build**

The following assumes the presence of the variables `commitHash`, `version`, and `buildTimestamp` in the `main` package:

```text
go build \
  -ldflags "-X main.commitHash=$(git rev-parse --verify HEAD) \
    -X main.version=$(git describe --tag $(git rev-list --tags --max-count=1)) \
    -X main.buildTimestamp=$(date +'%Y%m%d%H%M%S')" \
  -o ./bin/command \
  ./cmd/command;
```

### **Windows-GUI Build**

```text
GOOS=windows GOARCH=386 \
  go build \
  -ldflags '-H=windowsgui' \
  -o ./bin/command \
  ./cmd/command;
```

