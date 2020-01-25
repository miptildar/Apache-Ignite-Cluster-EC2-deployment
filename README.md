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
JAVA_HOME="/usr/lib/jvm/java-8..." (java 11 didn't work for me, I used OpenJdk Java 8)
OPTION_LIBS="ignite-aws"
```

4. In the `OPTION_LIBS` environment variable you specified a list of extra libraries (library names) to be used in your deployement.
Everything that was specified in the `OPTION_LIBS` environment variable has to be moved from `IGNITE_HOME/libs/optional` folder to `IGNITE_HOME/libs`  (in this case `ignite-aws`)

Also you have to move all jars from "extra jars" folder to `IGNITE_HOME/libs` folder.

5. Put `ignite.xml` configuration file to `/home/ubuntu/config` folder. Example of `ignite.xml` configuration file you can find in the official documentation (see the section related to AWS S3 deployment).

6. Apache Ignite startup
```shell
sudo bin/ignite.sh /home/ubuntu/config/ignite.xml
```

Something from ignite.xml properties file
```xml
<bean class="org.apache.ignite.configuration.IgniteConfiguration">
        <property name="cacheConfiguration">
            <list>
                <bean class="org.apache.ignite.configuration.CacheConfiguration">
                    <property name="name" value="default"/>
                    <property name="atomicityMode" value="ATOMIC"/>
                    <property name="backups" value="1"/>
                </bean>
            </list>
        </property>
		
		<property name="addressResolver">
			<bean class="org.apache.ignite.configuration.BasicAddressResolver">
				<constructor-arg>
					<map>
						<entry key="internal IP" value="public IP"/>
					</map>
				</constructor-arg>
			</bean>
		</property>

		<property name="discoverySpi">
			<bean class="org.apache.ignite.spi.discovery.tcp.TcpDiscoverySpi">
				<property name="ipFinder">
					<bean class="org.apache.ignite.spi.discovery.tcp.ipfinder.s3.TcpDiscoveryS3IpFinder">
						<property name="awsCredentials" ref="aws.creds"/>
						<property name="bucketName" value="YOUR_BUCKET_NAME"/>
					</bean>
				</property>
			</bean>
		</property>
    </bean>
	
	
	<bean id="aws.creds" class="com.amazonaws.auth.BasicAWSCredentials">
		<constructor-arg value="ACCESS_KEY"/>
		<constructor-arg value="SECRET_ACCESS_KEY"/>
	</bean>
```


P.S. Apache Ignite is a Java application. One can always create a maven project in your IDE, download Ignite from maven repository, add all necessary dependencies and run in as a jar file. 

See this link to read about [AWS S3 discovery](https://apacheignite-mix.readme.io/docs/amazon-aws)
