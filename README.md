Heroku buildpack: Vendor Binaries
=================================

Modification from original: No .tar.gz, just single files with destination paths.

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for vendoring binaries into your project. It doesn't do anything else, so to actually compile your app you should use [heroku-buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi) to combine it with a real buildpack.

Usage
-----

    $ ls
    .vendor_url_map
    .buildpacks

    $ heroku create --stack cedar --buildpack http://github.com/dollar/heroku-buildpack-multi.git#2a4cefc75

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    -----> Found a .vendor_url_map file
           Vendoring https://s3.amazonaws.com/my-bucket/foo.db3 to bar.db3
    ...

The buildpack will detect that your app has a `.vendor_url_map` file
in the root. Each line in this file will be a tab-delimited pair
of local destination path and a URL to provision it with.
