Minimal test case for https://github.com/bazelbuild/rules_docker/issues/2093

```
$ docker build app
...
Target //:app up-to-date:
  bazel-out/darwin-fastbuild-ST-4a519fd6d3e4/bin/app-layer.tar
...
$ jq .history bazel-out/darwin-fastbuild-ST-4a519fd6d3e4/bin/app.0.config
[
  {
    "author": "Bazel",
    "created": "1970-01-01T00:00:00Z",
    "created_by": "bazel build ..."
  },
  {
    "created": "2022-05-23T19:19:30.413290187Z",
    "created_by": "/bin/sh -c #(nop) ADD file:8e81116368669ed3dd361bc898d61bff249f524139a239fdaf3ec46869a39921 in / "
  },
  {
    "created": "2022-05-23T19:19:31.970967174Z",
    "created_by": "/bin/sh -c #(nop)  CMD [\"/bin/sh\"]",
    "empty_layer": true
  }
]
```

Expected result:

```
[
  {
    "created": "2022-05-23T19:19:30.413290187Z",
    "created_by": "/bin/sh -c #(nop) ADD file:8e81116368669ed3dd361bc898d61bff249f524139a239fdaf3ec46869a39921 in / "
  },
  {
    "created": "2022-05-23T19:19:31.970967174Z",
    "created_by": "/bin/sh -c #(nop)  CMD [\"/bin/sh\"]",
    "empty_layer": true
  },
  {
    "author": "Bazel",
    "created": "1970-01-01T00:00:00Z",
    "created_by": "bazel build ..."
  }
]
```
