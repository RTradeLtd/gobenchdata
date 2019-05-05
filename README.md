
# 📉 gobenchdata

[![View Action](https://img.shields.io/badge/view-action-yellow.svg)](https://github.com/marketplace/actions/gobenchdata-to-gh-pages)
[![GoDoc](https://godoc.org/github.com/bobheadxi/gobenchdata?status.svg)](https://godoc.org/github.com/bobheadxi/gobenchdata)

a tool for inspecting `go test -bench` data, and a
[GitHub Action](https://github.com/features/actions) for continuous benchmarking.

<img align="right" width="500" src="./.static/demo-chart.png">

* [GitHub Action](#github-action)
  * [Setup](#setup)
  * [Configuration](#configuration)
  * [Visualizing Continuous Benchmarks](#visualizing-continuous-benchmarks)
* [CLI](#cli)
* [Development and Contributions](#development-and-contributions)

## GitHub Action

A GitHub Action for uploading Go benchmark data to `gh-pages` using `gobenchdata`.

### Setup

For example, in `main.workflow`:

```hcl
workflow "Benchmark" {
  on = "push"
  resolves = ["gobenchdata to gh-pages"]
}

action "filter" {
  uses = "actions/bin/filter@master"
  args = "branch master"
}

action "gobenchdata to gh-pages" {
  uses = "bobheadxi/gobenchdata@master"
  needs = ["filter"]
  secrets = ["GITHUB_TOKEN"]
  env = {
    PRUNE = "30"
  }
}
```

### Configuration

| Variable             | Default                   | Purpose
| -------------------- | ------------------------- | -------
| `GITHUB_TOKEN`       | set by GitHub             | token to provide access to repository
| `GITHUB_ACTOR`       | set by GitHub             | the user to make commits as
| `GIT_COMMIT_MESSAGE` | `"add new benchmark run"` | the commit message for the benchmark update
| `GO_BENCHMARKS`      | `.`                       | benchmarks to run (argument for `-bench`)
| `GO_BENCHMARK_FLAGS` |                           | additional flags for `go test`
| `GO_BENCHMARK_PKGS`  | `./...`                   | packages to test (argument for `go test`)
| `FINAL_OUTPUT`       | `benchmarks.json`         | destination path of benchmark data

### Visualizing Continuous Benchmarks

The `gobenchdata` GitHub action eventually generates a JSON file with past benchmarks.
You can visualize these continuous benchmarks by creating a web app that reads
from the JSON benchmarks file, or by using `gobenchdata-web`:

```
go get -u github.com/bobheadxi/x/gobenchdata-web
git checkout gh-pages
gobenchdata-web --title "my benchmarks" # generates a web app in your working directory
```

You test the web app locally using a tool like [serve](https://www.npmjs.com/package/serve):

```
serve .
```

This feature is a work in progress. An example site published by this repository is
available at [gobenchdata.bobheaxi.dev](https://gobenchdata.bobheadxi.dev/).

## CLI

`gobenchdata` is also available as a CLI:

```
go get -u github.com/bobheadxi/gobenchdata
gobenchdata help
```

Usage documentation can be found in the
[godocs](https://godoc.org/github.com/bobheadxi/gobenchdata).

## Development and Contributions

Please report bugs and requests in the [repository issues](https://github.com/bobheadxi/gobenchdata)!

See [CONTRIBUTING.md](./CONTRIBUTING.md) for more detailed development documentation.