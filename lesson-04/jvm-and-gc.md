### Java helper. Part 4. JVM and GC.

read full here:
[link](https://lookingforere.medium.com/java-helper-part-4-jvm-and-gc-7a973e91f738)

The main goal of Java developers is to create portable applications.


The JVM plays a central role in portability it provides the proper level of abstraction between the compiled program and the underlying hardware platform and operating system.
Comparing with other languages, we can see that the JVM is an "extra layer", but the application speed is high because the bytecode executed by the JVM is perfectly optimised.

---

Virtual Machine Architecture

Class loader

– Bootstrap-is a base loader that loads platform classes. This loader is the parent of all other classes and part of the platform.
– Extension ClassLoader-is an extension loader, a descendant of the Bootstrap loader. Loads extension classes that are located in the jre/lib/ext directory by default.
– AppClassLoader-is a system classpath classloader that is a direct child of Extension ClassLoader. It loads classes from directories and jars specified by the CLASSPATH environment variable, the java.class.path system property, or the -classpath command line option.
– Own Loader. An application can have its own loaders.

1. Runtime data aria
   – Method aria. All class-level data, including static variables, will be stored here. There is only one method area per JVM and it is a shared resource.
   – Heap. All objects and their corresponding instance variables and arrays will be stored here. There is also one heap area per JVM. The stored data is not thread-safe.
   – Stack. A separate runtime stack will be created for each thread. For each method call, one entry will be made in the stack memory, called the stack frame. All local variables will be created in stack memory. The stack area is thread-safe because it is not a shared resource.
   – PC register. Each thread will have separate PC registers to store the address of the currently executing instruction, when the instruction is executed the PC register will be updated with the next instruction.
   – Native method stack. It holds native method information.
2. Executor engine
   – Interpreter. The interpreter interprets the bytecode.
   – JIT Compiler. The JIT compiler neutralizes the lack of an interpreter. The execution engine will use the help of the interpreter to convert the bytecode, but when it finds duplicate code, it uses a JIT compiler that compiles all the bytecode and changes it to native code. This source code will be used directly for method retries, which improves system performance.
   – Garbage Collector. The Garbage collector (GC) collects and removes unreferenced objects. GC of the JVM collects the objects that are created.
   – JNI (Java Native Interface). It will interact with native method libraries and provide native libraries required by the execution engine.
   – NML (Native Method Libraries). It is a collection of the Native Libraries which is required for the Execution Engine.

---

Memory
Let's dwell a little on the memory model. And we should know three important rules:
Single-threaded programs are executed pseudo-sequentially.
Values are not taken from nowhere.
Events are executed in order if they are related by a strict partial order "executes before" or "happens before" relationship.

Happens before!
So, this is a good example below 👇
```
private static int x = 0;
private static volatile boolean f = false;

public static void main(String[] args) throws InterruptedException {
new Thread(() -> {      
x = 10; // write to X      
while (!f); // waiting true! (volatile)      
System.out.println(x); // reading X  
}).start();

    x = 5; // write to X (Main thread) 
    f = true; // set flag to true
}
```

So, reading and writing to a volatile variable creates a "happens before" relationship, but writing to X has a "racing condition" and we really can't know what will be written.
And one more time about Stack and Heap!

Stack memory in Java is used for static memory allocation and thread execution. Access to this memory is in Last-In-First-Out (LIFO) order.
Heap space is used for the dynamic memory allocation of Java objects and JRE classes at runtime. We can separate the heap space like:
Young Generation for new objects. A minor Garbage collection occurs when this fills up.
Old or Tenured Generation-next iteration for objects. When objects are stored in the Young Generation, a threshold for the object's age is set, and when that threshold is reached, the object is moved to the old generation.
Permanent Generation-It consists of JVM metadata for the runtime classes and application methods.

Do you still no idea how it work? Let me show an example:
```dtd
    class Connector {    
        int port;    
        String host;
    
        public Connector(int port, String host) {        
        this.port = port;        
        this.host = host;    
        }
    }
```
and 
```dtd
public class ConnectorExample {

    private static Connector build(int port, String host) {        
      return new Connector(port, host);    
    }

    public static void main(String[] args) {
        int port = 8080;        
        String host = "localhost";        
        Connector connector = null;        

        connector = build(port, host);    
    }
}
```
Let's analyse this step-by-step:
When we start our process with main() method, a stack memory space is created to store this method's primitives and references.

Stack memory directly stores the primitive value of integer id.
The reference variable connector of type Connector will also be created in stack memory, which will point to the actual object in the heap.

The call to the constructor Connector(int, String) from main() will allocate further memory on top of the previous stack. This will store:

The object reference of the calling object in stack memory
The primitive value id in the stack memory
The argument host moves to the string pool in heap memory.

1. A calling the build() static method, for which further allocation will take place in stack memory on top of the previous one.
2. Heap memory will store all instance variables for the newly created object connector.

---

## GC (Garbage collector)

Garbage Collector (GB) is part of the JVM, which is designed to clean up the memory allocated to the application. He must:
find garbage (unused objects)
remove trash

There are two ways to find a garbage:
Reference counting-each object has a reference count. When it is zero, the object is considered garbage. The problem with this approach is that objects can have cyclic references to each other, while they are actually garbage and are not used by the program.
Tracing-an object is considered non-garbage if it can be reached from root points (GC Root: local variables and method parameters, java streams, static variables, references from JNI.

As we know, we can divide memory model into two parts:
- Heap-heap. The main memory segment where all objects are held and where garbage collection takes place.
- Permanent Generation-contains class metadata.Immediately about the Permanent Generation.

Heap-it is a place where the GC comes into play. It is divided into two areas:
New (Young) Generation-objects, cat. here are considered short-lived.
Old Generation (Tenured)-objects are considered long-lived.

The GC algorithm is based on the assumption that most java objects have a short lifespan. They quickly become trash. You need to get rid of them pretty quickly. Which is what happens in New Generation. There, garbage collection is much more frequent than in Old Generation, where long-lived objects are stored. Once created, an object goes into New Generation and has a chance to go into Old Generation after some time (GC cycles).
Heap consists of:
Eden-objects are allocated here. If there is no space, the GC is started.
Survivor-more precisely, there are two of them, S1 and S2, and they change roles. Objects that are recognised as alive during GC are stored.

JVM has four types of GC implementations:
Serial Garbage Collector

When there is no space in Eden, GC is started, live objects are copied to S1. The entire Eden area is cleared. S1 and S2 are swapped. On subsequent cycles, live objects from both Eden and S2 will be written to S1. After a few cycles of S1 and S2 swapping or filling up the S2 area, objects that live long enough are moved to Old Generation.
It should be said that objects are not always allocated to Eden when they are created. For example, if the object is too large, it immediately goes to Old Generation.
After the next garbage collection, there is not enough space already in the New Generation, then the garbage collection is started in the Old Generation (along with the New Generation collection). In old Generation, objects are compacted (Mark-Sweep-Compact algorithm).
If it is still not enough space after a full garbage collection, then a Java.lang.OutOfMemoryError will be thrown.
As a rule, Old Generation occupies 2/3 of the Heap volume. The efficiency of the garbage collection algorithm is calculated by the STW (Stop The World) parameter-the time when all processes except the GC stop. Serial is not very efficient in this sense, because does its job slowly, in one thread.
Parallel Garbage Collector  

Same as Serial, but uses multiple threads to run. Thus, STW (stop the Word) is slightly smaller.
CMS Garbage Collector  

The principle of working with New Generation is the same as in the case of the Serial and Parallel algorithms, the difference is that this algorithm separates the younger (New Generation) and older (Old Generation) garbage collection in time.
Moreover, garbage collection in the Old Generation occurs in a separate thread, regardless of the younger assembly. At the same time, the application first stops, the collector marks all live objects accessible from the GC Root (root points) directly, then the application starts again, and the collector checks the objects accessible by links from these marked ones, and also marks them as live. This feature creates so-called floating objects, which are marked as alive, but in fact they are not. But they will be removed in the next cycles. Those. throughput increases, STW decreases, but more space is required to store floating objects.
G1 Garbage Collector  

It divides the Heap area not physically, but rather logically into the same areas: Eden, Survivor, Old Generation. And defragmented. Physically, the Heap area is divided into regions of the same size, each of which can be Eden, Survivor or Old Generation + an area for large objects (huge region).
Several threads are working on cleaning Eden regions at once, objects are transferred to Survivor regions or regions of the older generation (Tenured). This is a familiar approach from previous cleaning algorithms. During the cleaning process, the application stops working. The difference is that cleaning is not done for all regions of Eden, but only for some that need it most, thus adjusting the cleaning time. Hence the name of the algorithm-primarily garbage.

---

How you can set which type of GC we need to use:


```dtd
-XX:+UseSerialGC-XX:+UseParallelGC-XX:+USeParNewGC-XX:+UseG1GC

```
To strictly monitor the application health, we should always check the JVM's Garbage Collectionperformance. The easiest way to do this is to log the GC activity in human readable format. Using the following parameters, we can log the GC activity:
`XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10-XX:GCLogFileSize=50M -Xloggc:/home/user/log/gc.log
We need to analyse our applications after crashing, as a good example out of memory error. That's why JVM comes with some parameters which dump heap memory into a physical file which can be used later for finding out leaks:
```
-XX:+HeapDumpOnOutOfMemoryError /-XX:HeapDumpPath=./java_pid<pid>.hprof-XX:OnOutOfMemoryError="< cmd args >;< cmd args >" /-XX:+UseGCOverheadLimit
```
What we should set above ☝️


HeapDumpOnOutOfMemoryError-write OutOfMemoryError to file.
HeapDumpPath-marks the path where the file is to be written.
OnOutOfMemoryError-is used to issue emergency commands to be executed in case of out of memory error.
UseGCOverheadLimit-is a policy that limits the proportion of the VM's time that is spent in GC before an OutOfMemory error is thrown.

---

Notes:
Virtual Machine Specification https://docs.oracle.com/javase/specs/jvms/se8/html/index.html
GC:https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html