---
layout: page
title: 3 - Syntax
permalink: /syntax/
order: 3
---

Continuous syntax is meant to be easy to read, understand and write. Let's have a look about the syntax.

## Just YAML pipelines

Instead of re-inventing the wheel, Continuous syntax is based on YAML, an easy-to-read/write markup language. YAML is popular in DevOps world, as Ansible or Kubernetes use it.

## Examples

The minimal-working skeleton is like this:

```yaml
when:
  events:
    - push
  branches:
    - main

tasks:
  - name: first task
    image: ubuntu:latest
    steps:
      - sh: echo "first output"
      - sh: echo "second output"
```

## When specification

`when` is the keyword describing how a pipeline can be called. It is a matrix of `events` and `branches` that are mutually exclusive.

### Supported events

* push
* tag

### Branches

Any branch can be used. Just, Continuous doesn't support wildcard yet. This syntax is the not allowed yet.

```yaml
when:
  events:
    - push
  branches:
    - feat/* # not supported !
```

## Task specification

### Dependencies between tasks

It is possible to instruct Continuous to execute a named task after the execution of another one was successful. This can be described as a DAG using the `depends_on` keyword (array):

```yaml
tasks:
  - name: first-task
    depends_on:
      - second-task

  - name: second-task
```

A task can be dependent of any other task.

### Shells

A step can be executed with `sh` or `bash`. You can select the shell you wanna use when describing a step:

```yaml
tasks:
  - name: example-task
    steps:
      - sh: echo "example-task"
      - bash: echo "example-task"
```

* `sh` will be executed as `/bin/sh -c '$command'`
* `bash` will be executed as `/bin/bash -c '$command'`


{: .warning }
> You have to make sure the `image` used for the task integrates `bash` or `sh`.
