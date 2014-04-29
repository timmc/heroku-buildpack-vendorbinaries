Heroku buildpack: Vendor Binaries
=================================

Modification from original: Allow .tar.gz expansion to a specified
directory (creating if necessary as well as downloading of single
files to destination paths.

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for vendoring binaries into your project. It doesn't do anything else, so to actually compile your app you should use [heroku-buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi) to combine it with a real buildpack.

Usage
-----

    $ ls
    .vendor_url_map
    .buildpacks

    $ heroku create --stack cedar --buildpack http://github.com/dollar/heroku-buildpack-multi.git#467b87032

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    -----> Found a .vendor_url_map file
           Vendoring single file https://s3.amazonaws.com/my-bucket/foo.db3 to bar.db3
    ...

The buildpack will detect that your app has a `.vendor_url_map` file
in the root. Each line in this file is a tab-delimited triple of local
destination path, provisioning type, and a URL to provision it with.

Supported provisioning types:

### `file-http`

Single file copy over HTTP(S), creating parent directories as
necessary.

```
foo/bar/baz.db3	file-http	https://s3.amazonaws.com/my-bucket/foo.db3
```

### `targz-http`

Extract a .tar.gz file from an HTTP(S) source to the specified base
dir, creating directories as necessary.

```
base/dir/	targz-http	https://s3.amazonaws.com/my-bucket/foo.tgz
```

Changelog
---------

This is v2, which uses triplets instead of pairs so as to support more
than just single-file drops. (v1 was not really released, and was
buggy -- it failed to create parent directories of targets.)
