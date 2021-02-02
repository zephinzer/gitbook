---
description: One-pager for Go
---

# Golang

## TL;DR

Go \(or Golang\) is a strongly-typed programming language with C-styled semantics created by engineers from Google for server-side applications

### Why use it

1. You're building a backend system that should be efficient
2. You want to implement concurrency and do it with style \(nice language semantics\)
3. You want to distributable your code as a native binary
4. You find Java and C\# and friends too verbose/boring
5. You find C++ too complicated and traditional
6. You want a strongly-typed language that's not too verbose \(maybe also see Kotlin/Rust\)
7. Your primary expertise is in cloud-native infrastructure and you're looking for a programming language you can apply in your system
8. You're building something with custom logic to integrate with Kubernetes that YAML cannot do alone

### Notable OSS projects

1. Kubernetes
2. Docker
3. Terraform
4. Istio \(control plane\)
5. Prometheus

## Dependency Management

Go comes with its own dependency management tool built into a sub-command. To initialise a Go project, run:

```text
# go mod init github.com/path/to/your/repo
```

To install a dependency locally in your project, reference it in your `import` statement, prefix the import with an underscore so it doesn't get removed by `go fmt`, and run:

```text
# go mod vendor
```

To install a dependency on your machine, run:

```text
# go get github.com/path/to/module
```

For versions beyond version 1 \(check the Git tags on the repository\), the convention is:

```text
# go get github.com/path/to/module.v2
```

If you want to upgrade the Go packages on your machine \(not your local project\):

```text
# go get -u github.com/path/to/module
```

If you want to install your local project dependencies to your machine, run:

```text
# go mod download
```

If the `go.mod` and `go.sum` file gets messed up somehow, run:

```text
# go mod tidy
```

## Script Management

There's no official script manager but typically Go projects use a `Makefile` to handle project-related tasks.

## File Management

This section covers how to organise files in a Go project. An excellent project with some conventions can be found at [https://github.com/golang-standards/project-layout](https://github.com/golang-standards/project-layout). The following elaborates based on their directory structure.

### For importing by others

You normally want all your `.go` files in the root directory so it's easy for consumers to import it.

### For creating a binary

You normally keep your root directory clean. The binary names should be `/cmd` relative to project root. Locally used packages that are not remotely destined for extraction into their own packages should be in `/internal` and packages that might be extracted eventually can go into `/pkg`.

## Testing

### File Organisation

Test files should be in the same directory as the code they test. For example:

```text
/internal
  /somepkg
    /fn_one.go
    /fn_one_test.go
    /fn_two.go
    /fn_two_test.go
```

### Unit Testing

To run all tests in your project, run:

```text
# go test ./...
```

The `./...` indicates all files recursively. You could also specify an exact path to a package like:

```text
# go test ./internal/somepkg
```

### Coverage

Go's test coverage report is weird. It does not conform to commonly found formats.

To generate the test coverage, run the test command with `-cover -coverprofile c.out` appended:

```text
# go test ./... -cover -coverprofile c.out
```

Note also that **packages without any test files will not be considered in the overall coverage**.

## Building

### Go Generators

### Building without CGO

### Building with CGO

### Static compilation

### Build-time variable injection

### Symbol stripping

## Release Management

Package releases are done using Git tags.

```text
# git tag v1.0.2
# git push origin master
```



