---
## k8s-bldr-yaml

yaml examples for a distributed build in a kubernetes cluster

Test to see how complicated it is to execute a builder in an existing
kubernetes cluster.

Use the k8s-bldr-api format file: status.yaml to perform an echo test
to validate access to an k8s-bldr exec cluster.

Use the k8s-bldr-api format file: build-hello.yaml to compile go a
hello web app and test connection

The command example uses yaml2json for which a statically linked
binary is provided in the repo for testing.

---
### Example pre validation of connectivity to the builder service

*Using the k8s-cfg scripts for access*

If you ran a tunnel with 

```
./k8s-cfg --start
./k8s-cfg --tunnel-start ; 
```

And run the query against the k8s-blder with:

```
curl --silent -d"$(yaml2json --compress < status.yaml)" localhost:9999/api/v1/validate|jq .
```

Then the following should print text, reply [ok] exit:0 for the
status.ok object.

    "status": [
      {
        "timestamp": "2015.11.14.23.05.48.+0000",
        "error": "<nil>",
        "exit": 0,
        "name": "status",
        "text": "ok\n"
      },

Then the following should print text, reply [Hello, World!] exit:0 for
the status.hello object.

      {
        "timestamp": "2015.11.14.23.05.48.+0000",
        "error": "<nil>",
        "exit": 0,
        "name": "hello",
        "text": "Hello, World!\n"
      }

---
### Example golang build test

```
curl --silent -d"$(yaml2json --compress < build-hello.yaml)" \
    localhost:9999/api/v1/validate|jq .
```

The output should show a successful build with some response text.
