# Gentoo tips and configurations

## Adding kernel patches

When downloading `gentoo-sources` you can edit the sources of the linux kernel automatically on each update by creating a patch file with diff. E.g. `diff -u $(pwd)/signal_types.h.bak $(pwd)/signal_types.h > /root/0001-amdgpu-clock.patch`. The patches will be applied in alphabetical order. be sure to prepend a/b before the relative path of the package source

The patch generated has to be located in `/etc/portage/patches/sys-kernel/gentoo-sources`

* The `0001-amdgpu-clock.patch` is necessary for outputting resolutions greater than 1080p on a RX470 Mining edition GPU by editing the pixel clock frequency.

# Portage / Emerge

## Updating Gentoo

```
emerge -avuDN --with-bdeps=y @world
```

## Adding specific use flags per package

```properties
boltoo /etc/portage/package.use # cat 01-base
sys-level/llvm -doc
app-misc/neofetch -X
```

## Unmasking a package
There are some packages that are only available in de development overlay (~amd64), for example:

```properties
boltoo /etc/portage # cat package.accept_keywords 
x11-misc/menulibre ~amd64
```

## Masking a package (hiding from your portage "database")
Imagine a package like rust, that takes a lot to compile, you can replace it with rust-bin and hide it so packages with rust as a dependency will not build it from source:

```properties
boltoo /etc/portage # cat package.mask 
dev-lang/rust
```
To be sure rust is covered:
```
emerge --ask --oneshot virtual/rust dev-lang/rust-bin
```

https://forums.gentoo.org/viewtopic-p-8504305.html?sid=b81d2b240c47b2f3a109b5feeab96658