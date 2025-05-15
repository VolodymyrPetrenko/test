import pyautogui
import time
import random

def move_cursor_randomly(iterations=10, delay=30, box_size=1000):
    screen_width, screen_height = pyautogui.size()
    start_x = (screen_width - box_size) // 2
    start_y = (screen_height - box_size) // 2

    for i in range(iterations):
        random_x = random.randint(start_x, start_x + box_size)
        random_y = random.randint(start_y, start_y + box_size)
        speed = random.uniform(0.5, 3.0)
        pyautogui.moveTo(random_x, random_y, duration=speed)

        time.sleep(delay)

if __name__ == "__main__":
    time.sleep(3)
    move_cursor_randomly(iterations=200, delay=60, box_size=100)


import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.lang.instrument.ClassFileTransformer;
import java.lang.instrument.Instrumentation;
import java.security.ProtectionDomain;
import java.util.concurrent.ConcurrentSkipListSet;

public class ClassListerAgent {

    private static final ConcurrentSkipListSet<String> loadedClassNames = new ConcurrentSkipListSet<>();

    public static void premain(String agentArgs, Instrumentation inst) {
        inst.addTransformer(new ClassFileTransformer() {
            @Override
            public byte[] transform(
                    Module module,
                    ClassLoader loader,
                    String className,
                    Class<?> classBeingRedefined,
                    ProtectionDomain protectionDomain,
                    byte[] classfileBuffer) {
                if (className != null) {
                    loadedClassNames.add(className.replace('/', '.'));
                }
                return null;
            }
        }, true);

        // Optionally start a shutdown hook to dump at JVM exit
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            try (BufferedWriter writer = new BufferedWriter(new FileWriter("loaded-classes.txt"))) {
                for (String name : loadedClassNames) {
                    writer.write(name);
                    writer.newLine();
                }
                System.out.println("All loaded classes written to loaded-classes.txt");
            } catch (IOException e) {
                e.printStackTrace();
            }
        }));
    }
}




<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-shade-plugin</artifactId>
      <version>3.2.4</version>
      <executions>
        <execution>
          <phase>package</phase>
          <goals>
            <goal>shade</goal>
          </goals>
          <configuration>
            <transformers>
              <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                <mainClass>your.package.ClassListerAgent</mainClass>
                <manifestEntries>
                  <Premain-Class>your.package.ClassListerAgent</Premain-Class>
                </manifestEntries>
              </transformer>
            </transformers>
          </configuration>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
