# Atlas CLI

[![Build Status](https://travis-ci.org/infobloxopen/atlas-cli.svg?branch=master)](https://travis-ci.org/infobloxopen/atlas-cli) [![Go Report Card](https://goreportcard.com/badge/github.com/infobloxopen/atlas-cli)](https://goreportcard.com/report/github.com/infobloxopen/atlas-cli)

This command-line tool helps developers become productive on Atlas. It aims to provide a better development experience by reducing the initial time and effort it takes to build applications.

## Getting Started
These instructions will help you get the Atlas command-line tool up and running on your machine.

### Prerequisites
Please install the following dependencies before running the Atlas command-line tool.

#### dep

This is a dependency management tool for Go. You can install `dep` with Homebrew:

```sh
$ brew install dep
```
More detailed installation instructions are available on the [GitHub repository](https://github.com/golang/dep).

#### golang-migrate
Bootstrapped applications use [golang-migrate](https://github.com/golang-migrate/migrate) for database migrations. You can install the `migrate` binary with the standard Go toolchain:

```
$ go get -u -d github.com/golang-migrate/migrate/cli github.com/lib/pq
$ go build -tags 'postgres' -o /usr/local/bin/migrate github.com/golang-migrate/migrate/cli
```
See the official golang-migrate [GitHub repository](https://github.com/golang-migrate/migrate) for more information about this tool.

### Installing
The following steps will install the `atlas` binary to your `$GOBIN` directory.

```sh
$ go get github.com/infobloxopen/atlas-cli/atlas
```
You're all set! Alternatively, you can clone the repository and install the binary manually.

```sh
$ git clone https://github.com/infobloxopen/atlas-cli.git
$ cd atlas-cli
$ make
```

## Bootstrap an Application
Rather than build applications completely from scratch, you can leverage the command-line tool to initialize a new project. This will generate the necessary files and folders to get started.

```sh
$ atlas init-app -name=my-application
$ cd my-application
```
#### Flags
Here's the full set of flags for the `init-app` command.

| Flag          | Description                                                     | Required      | Default Value |
| ------------- | --------------------------------------------------------------- | ------------- | ------------- |
| `name`        | The name of the new application                                 | Yes           | N/A           |
| `db`          | Bootstrap the application with PostgreSQL database integration  | No            | `false`       |
| `gateway`     | Initialize the application with a gRPC gateway                  | No            | `false`       |
| `registry`    | The Docker registry where application images are pushed         | No            | `""`          |

You can run `atlas init-app --help` to see these flags and their descriptions on the command-line.

#### Additional Examples


```sh
# generates an application with a grpc gateway 
atlas init-app -name=my-application -gateway
```

```sh
# generates an application with a postgres database
atlas init-app -name=my-application -db
```

```sh
# specifies a docker registry
atlas init-app -name=my-application -registry=infoblox
```
Images names will vary depending on whether or not a Docker registry has been provided.

```sh
# docker registry was provided
registry-name/image-name:image-version
```

```sh
# docker registry was not provided
image-name:image-version
```

Of course, you may include all the flags in the `init-app` command.

```sh
atlas init-app -name=my-application -gateway -db -registry=infoblox
```

## Contributing to the Atlas CLI
Contributions to the Atlas CLI are welcome via pull requests and issues. If you're interested in making changes or adding new features, please take a minute to skim these instructions.

### Regenerating Template Bindata
The `templates/` directory contains a set of Go templates. When the Atlas CLI bootstraps a new application, it uses these templates to render new application files.

This project uses [go-bindata](https://github.com/jteeuwen/go-bindata) to package the Go templates and `atlas` source files together into a single binary. If your changes add or update templating, you need to regenerate the template bindata by running this command:

```sh
make templating
```

Your templating changes will take effect next time you run the `atlas` binary.

### Running the Integration Tests

The Atlas CLI integration tests ensure that new changes do not break exists features. To run the Atlas CLI unit and integration tests locally, set `e2e=true` in your environment.
```
make test-with-integration
```

### Adding New CLI Commands

If you wish to add a new command to the Atlas CLI, please take a look at the [command interface](https://github.com/infobloxopen/atlas-app-toolkit/blob/master/cli/atlas/commands/command.go). This interface is intended to make adding new commands as minimally impactful to existing functionality.

To start out, consider looking at the [bootstrap command's implementation](https://github.com/infobloxopen/atlas-app-toolkit/blob/master/cli/atlas/commands/initialize.go#L37) of this interface.
