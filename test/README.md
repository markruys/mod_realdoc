# Test

Background info: [Atomic deploys at Etsy](https://codeascraft.com/2013/07/01/atomic-deploys-at-etsy/).



## Build

To build the module into a package, you could use something like:

```shell
$ git clone https://github.com/markruys/docker-deb-builder.git
$ ./docker-deb-builder/build -i docker-deb-builder:20.04 -e EMAIL -f "YOUR NAME" -o . ..
$ ls -l *.deb
-rw-r--r--@ 1 markr  staff  5476 18 feb 16:24 libapache2-mod-realdoc_1.0-1focal1_amd64.deb
```



## Test

Now that we have the deb file, we can build & run a Docker test image:

```shell
docker build -t php_symlink_test .
docker run -ti --rm -p 8000:80 --name="apache_server" php_symlink_test
```

Inside the container, run the following tests:

```shell
$ apachectl start
```

Open http://localhost:8000/ and confirm you get an 'A'.

```shell
$ rm html && ln -s variant-b html
```

Refresh the browser and you'll still get an 'A', which is wrong. Now enable the realdoc module:

```shell
$ a2enmod realdoc && apachectl restart
```

Refresh the browser and we will get the 'B' eventually. Now:

```shell
$ rm html && ln -s variant-a html
```

Wait 2 seconds, refresh your browser and notice this time we do get an 'A'.