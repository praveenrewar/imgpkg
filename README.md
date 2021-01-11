# imgpkg

- Website: [https://carvel.dev/imgpkg](https://carvel.dev/imgpkg)
- Slack: [#carvel in Kubernetes slack](https://kubernetes.slack.com/archives/CH8KCCKA5)
- [Docs](https://carvel.dev/imgpkg/docs/latest/) with example workflow and other details
- Install: Grab prebuilt binaries from the [Releases page](https://github.com/vmware-tanzu/carvel-imgpkg/releases) or [Homebrew Carvel tap](https://github.com/vmware-tanzu/homebrew-carvel)

`imgpkg` (pronounced: "image package") is a tool that allows users to store a set of arbitrary files as an OCI image. One of the driving use cases is to store Kubernetes configuration (plain YAML, ytt templates, Helm templates, etc.) in OCI registry as an image.

imgpkg's primary concept is a [bundle](https://carvel.dev/imgpkg/docs/latest/resources/#bundle), which is an OCI image that holds 0+ arbitrary files and 0+ references to dependent OCI images. With this concept, imgpkg is able to copy bundles and their dependent images across registries (both online and offline).

```bash
$ imgpkg push -b your-user/app1-config:0.1.1 -f config/
$ imgpkg copy -b your-user/app1-config:0.1.1 --to-repo other-user/app1
$ imgpkg pull -b your-user/app1-config:0.1.1 -o /tmp/app1-config
$ imgpkg tag ls -i your-user/app1-config
```

Features:

- Allows to push a bundle containing a set of files, and a list of images on which they depend
- Allows to pull a bundle and extract the same set of files and list of image references
- Allows to copy a bundle thickly (i.e. bundle image + all referenced images) to a repo or tarball
- Air-gapped environment support via copy command
- Allows to list pushed image tags
- Uses Docker layer media type to work with existing registries
- Uses deterministic file permissions and timestamps to make images reproducable (same digest if nothing changed)

## Development

```bash
./hack/test-all-local-registry.sh 5000
```
