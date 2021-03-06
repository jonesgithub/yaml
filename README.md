# yaml
yaml is a lightweight and flexible command-line YAML processor

The aim of the project is to be the [jq](https://github.com/stedolan/jq) or sed of yaml files.

## Install
[Download latest binary](https://github.com/mikefarah/yaml/releases/latest) or alternatively:
```
go get github.com/mikefarah/yaml
```

## Features
- Written in portable go, so you can download a lovely dependency free binary
- Deep read a yaml file with a given path
- Update a yaml file given a path
- Update a yaml file given a script file
- Convert from json to yaml
- Convert from yaml to json

## Read examples
```
yaml r <yaml file> <path>
```

### Basic
Given a sample.yaml file of:
```yaml
b:
  c: 2
```
then
```bash
yaml r sample.yaml b.c
```
will output the value of '2'.

### Reading from STDIN
Given a sample.yaml file of:
```bash
cat sample.yaml | yaml r - b.c
```
will output the value of '2'.

### Splat
Given a sample.yaml file of:
```yaml
---
bob:
  item1:
    cats: bananas
  item2:
    cats: apples
```
then
```bash
yaml r sample.yaml bob.*.cats
```
will output
```yaml
- bananas
- apples
```

### Handling '.' in the yaml key
Given a sample.yaml file of:
```yaml
b.x:
  c: 2
```
then
```bash
yaml r sample.yaml \"b.x\".c
```
will output the value of '2'.

### Arrays
You can give an index to access a specific element:
e.g.: given a sample file of
```yaml
b:
  e:
    - name: fred
      value: 3
    - name: sam
      value: 4
```
then
```
yaml r sample.yaml b.e[1].name
```
will output 'sam'

### Array Splat
e.g.: given a sample file of
```yaml
b:
  e:
    - name: fred
      value: 3
    - name: sam
      value: 4
```
then
```
yaml r sample.yaml b.e[*].name
```
will output:
```
- fred
- sam
```

## Update examples

### Update to stdout
Given a sample.yaml file of:
```yaml
b:
  c: 2
```
then
```bash
yaml w sample.yaml b.c cat
```
will output:
```yaml
b:
  c: cat
```

### Updating yaml in-place
Given a sample.yaml file of:
```yaml
b:
  c: 2
```
then
```bash
yaml w -i sample.yaml b.c cat
```
will update the sample.yaml file so that the value of 'c' is cat.


### Updating multiple values with a script
Given a sample.yaml file of:
```yaml
b:
  c: 2
  e:
    - name: Billy Bob
```
and a script update_instructions.yaml of:
```yaml
b.c: 3
b.e[0].name: Howdy Partner
```
then

```bash
yaml w -s update_instructions.yaml sample.yaml
```
will output:
```yaml
b:
  c: 3
  e:
    - name: Howdy Partner
```

## Converting to and from json

### Yaml2json
To convert output to json, use the --tojson (or -j) flag. This can be used with any command.

### json2yaml
To read in json, use the --fromjson (or -J) flag. This can be used with any command.
