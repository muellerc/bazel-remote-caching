# Bazel

For an introduction into Bazel start [here](https://bazel.build/).
To follow this simple short example on Bazel remote caching, you only need to have installed Java and Bazel. You can read more about how to install Bazel [here](https://bazel.build/install). If you are on MacOS, a simple `brew install bazel` is all you need.

## Local Cache

To build this basic `hello-world` multi-package Bazel project, navigate to the `multi-package` subdirectory first:
```bash
cd multi-package
```

Now run the Bazel build command for the `hello-world` package, which will also build the messenger-lib. This is because we declared this dependency in our [BUILD](multi-package/BUILD) file.
```bash
bazel build //:hello-world
```

You should see an output like this:
```txt
INFO: From Building libmessenger-lib.jar (1 source file):
INFO: From Building hello-world.jar (1 source file):
INFO: Found 1 target...
Target //:hello-world up-to-date:
  bazel-bin/hello-world
  bazel-bin/hello-world.jar
INFO: Elapsed time: 1.575s, Critical Path: 1.25s
INFO: 10 processes: 5 internal, 3 darwin-sandbox, 2 worker.
INFO: Build completed successfully, 10 total actions
```

To remove all build artifacts and logs, run:
```bash
bazel clean
```

## Remote Cache
To demonstrate building with Bazel Remote Cache, we use the official [bazel-remote cache](https://github.com/buchgr/bazel-remote).
First, let's pull and run the bazel-remote cache Docker image. We run the Docker container in foreground, as we can easily see the HTTP PUT/GET requests in this case:
```bash
docker pull buchgr/bazel-remote-cache
docker run -u 1000:1000 -v $PWD/data:/data \
	-p 9090:8080 -p 9092:9092 buchgr/bazel-remote-cache \
	--max_size 5
```

Now let's execute the Bazel build again, but this time we also provide the `--remote_cache` option which tels Bazel, which remote cache to use:
```bash
bazel build --remote_cache=http://localhost:9090 //:hello-world
```

When running it the first time, we will see a couple HTTP GET 404 (cache miss)