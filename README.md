# Apache Ignite Cluster deployment in AWS cloud

1. We are located at `/home/ubuntu`

2. Download Apache Ignite binary distribution:
```shell
wget http://mirror.ox.ac.uk/sites/rsync.apache.org//ignite/2.7.0/apache-ignite-2.7.0-bin.zip
```
or see [Binary Releases](https://ignite.apache.org/download.cgi#binaries)

Unzip this file

3. Set up environment variables
```
JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64" (or your option)
OPTION_LIBS="ignite-aws"
```

4. In the `OPTION_LIBS` environment variable you specified a list of extra libraries (library names) to be used in your deployement.
Everything that was specified in the `OPTION_LIBS` environment variable you have to move from `IGNITE_HOME/libs/optional` folder to `IGNITE_HOME/libs`  (in this case `ignite-aws`)

Also you have to move all jars from "extra jars" folder to `IGNITE_HOME/libs` folder.

5. Put `ignite.xml` configuration file to `/home/ubuntu/config` folder. Example of `ignite.xml` configuration file you can find in the official documentation (see the section related to AWS S3 deployment).

6. Apache Ignite startup
```shell
sudo bin/ignite.sh /home/ubuntu/config/ignite.xml
```

or starting up as a daemon:
```shell
sudo bin/ignite.sh /home/ubuntu/config/ignite.xml >/dev/null 2>&1 < /dev/null &
```
