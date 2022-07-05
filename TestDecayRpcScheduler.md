This is also a test similar to the test in [TestValueQueue.md](TestValueQueue.md)

path: /hadoop/hadoop-common-project/hadoop-common/src/test/java/org/apache/hadoop/ipc/TestDecayRpcScheduler.java

original test code:
```java
@Test
@SuppressWarnings("deprecation")
public void testParsePeriod() {
    // By default
    scheduler = new DecayRpcScheduler(1, "ipc.1", new Configuration());
    assertEquals(DecayRpcScheduler.IPC_SCHEDULER_DECAYSCHEDULER_PERIOD_DEFAULT,
    scheduler.getDecayPeriodMillis());

    // Custom
    Configuration conf = new Configuration();
    conf.setLong("ipc.2." + DecayRpcScheduler.IPC_FCQ_DECAYSCHEDULER_PERIOD_KEY,
      1058);
    scheduler = new DecayRpcScheduler(1, "ipc.2", conf);
    assertEquals(1058L, scheduler.getDecayPeriodMillis());
}
```
we can get to know test in this file contains two test cases, one is by default and the other is by custom. Why not use ParameterizedTest to generate both of them? A try here:
```java
@Test
@SuppressWarnings("deprecation")
public void testParsePeriod(int numLevels, String ns, long period) {
    // By default
    scheduler = new DecayRpcScheduler(numLevels, ns, new Configuration());
    assertEquals(period, scheduler.getDecayPeriodMillis());

    // Custom
    Configuration conf = new Configuration();
    conf.setLong(ns + "." + DecayRpcScheduler.IPC_FCQ_DECAYSCHEDULER_PERIOD_KEY,
      period);
    scheduler = new DecayRpcScheduler(numLevels, ns, conf);
    assertEquals(period, scheduler.getDecayPeriodMillis());
}
```