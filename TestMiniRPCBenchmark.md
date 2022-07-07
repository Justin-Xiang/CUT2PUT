path: /hadoop/hadoop-common-project/hadoop-common/src/test/java/org/apache/hadoop/ipc/TestMiniRPCBenchmark.java

code:
```java
public class TestMiniRPCBenchmark {
  @Test
  public void testSimple() throws Exception {
    Configuration conf = new Configuration();
    conf.set("hadoop.security.authentication", "simple");
    MiniRPCBenchmark mb = new MiniRPCBenchmark(Level.DEBUG);
    mb.runMiniBenchmark(conf, 10, null, null);
  }
}
```
here, 10 is the value of count, which refers to the times of connection. And part of the *runMiniBenchmark* method code is here:
```java
try {
      // start the server
      miniServer = new MiniServer(conf, user, keytabFile);
      InetSocketAddress addr = miniServer.getAddress();

      connectToServer(conf, addr);
      // connect to the server count times
      setLoggingLevel(logLevel);
      long elapsed = 0L;
      for(int idx = 0; idx < count; idx ++) {
        elapsed += connectToServer(conf, addr);
      }
      return elapsed;
    } finally {
      if(miniServer != null) miniServer.stop();
    }
```
apparently, no matter what *count* is , it will connect at least once. I though there should be an assertion to test this, but none was found.

Besides, the
```java
conf.set("hadoop.security.authentication", "simple");
```
here set a value which to authentication method and enable miniserver security enough. Code is here:
```java
public static boolean isSecurityEnabled() {
    return !isAuthenticationMethodEnabled(AuthenticationMethod.SIMPLE);
  }
```
In this case, if I change "simple" to any other worlds, this test will fail.