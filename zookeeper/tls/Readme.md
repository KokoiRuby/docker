A guide to setup tls for zookeeper

#### [Quorum TLS](https://zookeeper.apache.org/doc/current/zookeeperAdmin.html#Quorum+TLS)

1. Create SSL keystore JKS to store local credentials. **One keystore should be created for each ZK instance**

```bash
# instance=zk{1..4}
$ keytool -genkeypair \
-alias $(instance) \
-dname "cn=$(instance)" \
-keyalg RSA -keysize 2048 \
-keypass password \
-keystore $(instance).jks \
-storepass password
```



2. Extract the signed public key (certificate) from keystore. *This step might only necessary for self-signed certificates.*

```bash
# instance=zk{1..4}
$ keytool -exportcert \
-alias $(instance) \
-keystore $(instance).jks \
-storepass password
-file$(instance).cer \
-rfc
```



3. Create SSL truststore JKS containing certificates of all ZooKeeper instances. The same truststore (storing all accepted certs) should be shared on participants of the ensemble. You need to use different aliases to store multiple certificates in the same truststore. Name of the aliases doesn't matter.

```bash
# instance=zk{1..4}
$ echo yes | keytool \
-importcert -alias $(instance) \
-file $(instance).cer \
-keystore truststore.jks \
-storepass password

# chk
$ keytool -list \
-keystore truststore.jks \
-storepass password
```



4. You need to use `NettyServerCnxnFactory` as serverCnxnFactory, because SSL is not supported by NIO. Add the following configuration settings to your `zoo.cfg` config file:

```bash
sslQuorum=true
serverCnxnFactory=org.apache.zookeeper.server.NettyServerCnxnFactory
ssl.quorum.keyStore.location=/path/to/keystore.jks
ssl.quorum.keyStore.password=password
ssl.quorum.trustStore.location=/path/to/truststore.jks
ssl.quorum.trustStore.password=password
```



5. Verify in the logs that your ensemble is running on TLS:

```java
INFO  [main:QuorumPeer@1789] - Using TLS encrypted quorum communication
INFO  [main:QuorumPeer@1797] - Port unification disabled
...
INFO  [QuorumPeerListener:QuorumCnxManager$Listener@877] - Creating TLS-only quorum server socket
```