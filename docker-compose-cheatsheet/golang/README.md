# ▶️ Golang

## Installation on Ubuntu

```bash
sudo add-apt-repository ppa:longsleep/golang-backports;
sudo apt update;
sudo apt install golang-go;
```

## System Checks

**Environment Checking**

```
go env
```

## Development

### **Installing Dependencies**

```
go mod vendor -v;
```

### **Updating Dependencies**

```
go mod tidy -v;
```

### **Running Commands**

```
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

```
go test -cover -coverprofile c.out ./...;
```

## Building

The following formulae expects that an invoker runs these from the root directory given a project layout as specificied at [https://github.com/golang-standards/project-layout](https://github.com/golang-standards/project-layout).

### **Basic Build**

```
go build -o ./bin/command ./cmd/command
```

### **Static Build**

```
CGO_ENABLED=0 \
  go build \
  -ldflags "-extldflags 'static'" \
  -o ./bin/command \
  ./cmd/command;
```

### **Symbols Stripped Build**

```
go build \
  -ldflags "-s -w" \
  -o ./bin/command \
  ./cmd/command;
```

### **Cross-Platform Build**

```
GOOS=windows GOARCH=386 \
  go build \
  -o ./bin/command \
  ./cmd/command;
```

### **Variable Injection Build**

The following assumes the presence of the variables `commitHash`, `version`, and `buildTimestamp` in the `main` package:

```
go build \
  -ldflags "-X main.commitHash=$(git rev-parse --verify HEAD) \
    -X main.version=$(git describe --tag $(git rev-list --tags --max-count=1)) \
    -X main.buildTimestamp=$(date +'%Y%m%d%H%M%S')" \
  -o ./bin/command \
  ./cmd/command;
```

### **Windows-GUI Build**

```
GOOS=windows GOARCH=386 \
  go build \
  -ldflags '-H=windowsgui' \
  -o ./bin/command \
  ./cmd/command;
```
