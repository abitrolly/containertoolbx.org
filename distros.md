---
layout: default
---
## Distro support

By default, Toolbx creates the container using an [OCI](https://www.opencontainers.org/) image called `<ID>-toolbox:<VERSION-ID>`, where `<ID>` and `<VERSION-ID>` are taken from the host's `/usr/lib/os-release`. For example, the default image on a Fedora 33 host would be `fedora-toolbox:33`.

This default can be overridden by the `--image` option in `toolbox create`, but operating system distributors should provide an adequately configured default image to ensure a smooth user experience.

## Image requirements

Toolbx customizes newly created containers in a certain way. This requires certain tools and paths to be present and have certain characteristics inside the OCI image.

Tools:
* `getent(1)`
* `id(1)`
* `ln(1)`
* `mkdir(1)`: for hosts where `/home` is a symbolic link to `/var/home`
* `passwd(1)`
* `readlink(1)`
* `rm(1)`
* `rmdir(1)`: for hosts where `/home` is a symbolic link to `/var/home`
* `sleep(1)`
* `test(1)`
* `touch(1)`
* `unlink(1)`
* `useradd(8)`
* `usermod(8)`

Paths:
* `/etc/host.conf`: optional, if present not a bind mount
* `/etc/hosts`: optional, if present not a bind mount
* `/etc/krb5.conf.d`: directory, not a bind mount
* `/etc/localtime`: optional, if present not a bind mount
* `/etc/machine-id`: optional, not a bind mount
* `/etc/resolv.conf`: optional, if present not a bind mount
* `/etc/timezone`: optional, if present not a bind mount

Toolbx enables `sudo(8)` access inside containers. The following is necessary for that to work:

* The image should have `sudo(8)` enabled for users belonging to either the   `sudo` or `wheel` groups, and the group itself should exist. File an [issue](https://github.com/containers/toolbox/issues/new) if you really need support for a different group. However, it's preferable to keep this list as short as possible.

* The image should allow empty passwords for `sudo(8)`. This can be achieved by either adding the `nullok` option to the `PAM(8)` configuration, or by add the `NOPASSWD` tag to the `sudoers(5)` configuration.

Since Toolbx only works with OCI images that fulfill certain requirements, it will refuse images that aren't tagged with `com.github.containers.toolbox="true"` and `com.github.debarshiray.toolbox="true"` labels. These labels are meant to be used by the maintainer of the image to indicate that they have read this document and tested that the image works with Toolbx. You can use the following snippet in a Dockerfile for this: 

```Dockerfile
LABEL com.github.containers.toolbox="true"
```

The label `com.github.debarshiray.toolbox="true"` was used in previous versions of toolbx but is currently deprecated.

[![Zuul](https://zuul-ci.org/gated.svg)](https://softwarefactory-project.io/zuul/t/local/builds?project=containers/toolbox) [![Daily Pipeline](https://softwarefactory-project.io/zuul/api/tenant/local/badge?project=containers/toolbox&pipeline=periodic)](https://softwarefactory-project.io/zuul/t/local/builds?project=containers%2Ftoolbox&pipeline=periodic) [![Arch Linux package](https://img.shields.io/archlinux/v/community/x86_64/toolbox)](https://www.archlinux.org/packages/community/x86_64/toolbox/) [![Fedora package](https://img.shields.io/fedora/v/toolbox/rawhide)](https://src.fedoraproject.org/rpms/toolbox/)