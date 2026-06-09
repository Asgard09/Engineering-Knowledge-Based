Keep your interfaces **small and focused**. Don't create a "fat" interface that bundles unrelated methods together, forcing classes to implement things they don't need.
# Bad Example
```Java
// One giant interface for ALL types of workers
public interface Worker {
    void work();
    void eat();
    void sleep();
    void attendMeeting();
    void writeReport();
}
```

```Java
// A human employee — fine, they do all of this
public class HumanEmployee implements Worker {
    @Override public void work()          { System.out.println("Working..."); }
    @Override public void eat()           { System.out.println("Having lunch..."); }
    @Override public void sleep()         { System.out.println("Going home to sleep..."); }
    @Override public void attendMeeting() { System.out.println("In a meeting..."); }
    @Override public void writeReport()   { System.out.println("Writing report..."); }
}
```

```Java
// A robot worker — it doesn't eat, sleep, or attend meetings!
public class RobotWorker implements Worker {
    @Override public void work()          { System.out.println("Working 24/7..."); }
    @Override public void eat()           { /* Robots don't eat — but forced to implement this! */ }
    @Override public void sleep()         { /* Robots don't sleep — but forced to implement this! */ }
    @Override public void attendMeeting() { /* N/A — forced anyway! */ }
    @Override public void writeReport()   { System.out.println("Generating report..."); }
}
```

# Good Example
```Java
public interface Workable {
    void work();
}

public interface Eatable {
    void eat();
}

public interface Sleepable {
    void sleep();
}

public interface Meetable {
    void attendMeeting();
}

public interface Reportable {
    void writeReport();
}
```

```Java
// Human implements everything relevant to them
public class HumanEmployee implements Workable, Eatable, Sleepable, Meetable, Reportable {
    @Override public void work()          { System.out.println("Working..."); }
    @Override public void eat()           { System.out.println("Having lunch..."); }
    @Override public void sleep()         { System.out.println("Going home to sleep..."); }
    @Override public void attendMeeting() { System.out.println("In a meeting..."); }
    @Override public void writeReport()   { System.out.println("Writing report..."); }
}
```

```Java
// Robot only implements what's relevant
public class RobotWorker implements Workable, Reportable {
    @Override public void work()        { System.out.println("Working 24/7..."); }
    @Override public void writeReport() { System.out.println("Generating report..."); }
}
```