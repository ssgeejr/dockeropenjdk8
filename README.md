Copied from: https://blog.softwaremill.com/docker-support-in-new-java-8-finally-fd595df0ca54

[tab-A]
docker run --name jdk181 -ti -m 512M openjdk:8u212-jdk /bin/bash
#copy files (tab-C)
java -XX:+PrintFlagsFinal -version | grep MaxHeap
javac AvailableProcessors.java
javac MemoryEater.java
java AvailableProcessors
java MemoryEater


[tab-B]
docker run --name jdk212 -ti -m 512M openjdk:8u212-jdk /bin/bash
#copy files (tab-C)
java -XX:+PrintFlagsFinal -version | grep MaxHeap
javac AvailableProcessors.java
javac MemoryEater.java
java AvailableProcessors
java MemoryEater


[tab-C]
docker cp AvailableProcessors.java jdk181:/
docker cp AvailableProcessors.java jdk212:/
docker cp MemoryEater.java jdk181:/
docker cp MemoryEater.java jdk212:/


#AvailableProcessors.java
public class AvailableProcessors {
public static void main(String[] args) {
// check the number of processors available
      System.out.println(""+Runtime.getRuntime().availableProcessors());
   }
}

#MemoryEater.java
public class MemoryEater
{
  public static void main(String[] args)
  {
    Vector v = new Vector();
    while (true)
    {
      byte b[] = new byte[1048576];
      v.add(b);
      Runtime rt = Runtime.getRuntime();
      System.out.println( "free memory: " + rt.freeMemory() );
    }
  }
}

Moreover, there are some new settings
-XX:InitialRAMPercentage
-XX:MaxRAMPercentage
-XX:MinRAMPercentage

If for some reason the new JVM behaviour is not desired it can be switched off using -XX:-UseContainerSupport

