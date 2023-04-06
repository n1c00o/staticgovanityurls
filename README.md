# staticgovanityurls

`staticgovanityurls` (Static Go Vanity URLs) is a simple script that generates
documents to index Go modules on custom domain names.
This is inspired from Google's [GoVanityUrls](https://github.com/GoogleCloudPlatform/govanityurls) 
project which is a web server.

`staticgovanityurls` has the advantage of being fast and creating static 
documents, meaning it can be freely deployed on services such as 
[Cloudflare Pages](https://pages.cloudflare.com).

However, to deploy any modification, it is required to update the configuration 
file and recreate a deployment pipeline (task that can be automated 
using continuous deployment).

## Usage

First, you will need a compatible Go toolchain. We recommend using the versions
specified in CI or the one in the [`go.mod`](go.mod) file.

Then, you can install the application by using:

```shell
go install github.com/n1c00o/staticgovanityurls@latest
```

Once the binary is installed and compiled, you will need to write a 
configuration file. Here is an example:

```yaml
# vanity.yml
---
hostname: go.example.com
paths:
	# go.example.com/foo
	- prefix: foo
	  repository: git.example.com/foo
	  vcs: git
	  dir: git.example.com/foo{/dir}
	  file: git.example.com/foo{/dir}/{file}#{line}
	# go.example.com/bar
	- prefix: bar
	  repository: bazaar.example.com/bar
	  vcs: bzr
```

When your configuration file is valid, you can generate documents by running:

```shell
staticgovanityurls -i=vanity.yaml -o=dist
```

You should see the documents inside the `dist` folder, ready to be deployed.

## License

The project is governed by a BSD-style license that can be found in the 
[LICENSE](LICENSE) file.
