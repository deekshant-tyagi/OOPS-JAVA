15.<!-- Functional Interface in JAVA -->
// Functional interface contain only one abstract method , but it can have any no of default, static and object class methods.

//A functional interface in Java is an interface that contains exactly one abstract method. It serves as a blueprint for lambda expressions and method references, enabling functional programming concepts within Java. Functional interfaces can have any number of default methods or static methods but must declare exactly one abstract method to qualify as a functional interface.Mostly they are used for writing concise and expressive code, particularly when dealing with operations like mapping, filtering, and reducing elements in collections, as well as in event handling.

@FunctionalInterface
interface Worker {
    void performTask(String name);
}

class Chef implements Worker {

    @Override
    public void performTask(String name) {
        System.out.println(name + " is cooking a delicious meal.");
    }
}

class Plumber implements Worker {

    @Override
    public void performTask(String name) {
        System.out.println(name + " is fixing a leak.");
    }
}

public class Main {
    public static void main(String[] args) {
        // Creating instances of different workers
        Worker chef = new Chef();
        Worker plumber = new Plumber();

        // Calling the performTask method for each worker
        chef.performTask("Dipanshu");
        plumber.performTask("Divyansh");
    }
}


16.<!-- Functional Interface using lambda expression in JAVA -->
// In Java, lambda expressions provide a concise way to express instances of functional interfaces (interfaces with a single abstract method)

// Instead of creating separate classes, lambda expressions are used to directly provide implementations of the performTask() method.

// syntax (parameters) -> expression_or_statement_block

// Lambda expression with no parameters and a single statement:
// () -> System.out.println("Hello, lambda expression!");

@FunctionalInterface

interface Worker {
    void performTask(String name);
}


public class Main {
    public static void main(String[] args) {
        // Creating instances of different workers
        Worker chef = (String name) ->{
            System.out.println(name+" is a good cook");
        };
        Worker plumber = (String name) ->{
            System.out.println(name+" is a good plumber");

        };

        // Calling the performTask method for each worker
        chef.performTask("Dipanshu");
        plumber.performTask("Divyansh");
    }
}


17.<!-- Method Reference in JAVA -->
<!-- Generally of two types,
1. Reference to a Static Method
2. Reference to an Instance Method of a Particular Object  -->
// In Java, a method reference is a shorthand notation that allows you to refer to methods or constructors without invoking them directly. It is often used in conjunction with functional interfaces and lambda expressions to provide more concise and readable code.

//In Java, :: (double colon) is known as the method reference operator. It is used to reference a method or a constructor without invoking it immediately, similar to lambda expressions but more concise in certain scenarios.

**//1. Reference to a Static Method**

class CoffeeMaker {
    public static void makeEspresso() {
        System.out.println("Making espresso...");
        // Steps to make espresso
        System.out.println("Espresso is ready!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Using lambda expression
        // Runnable runnable = () -> CoffeeMaker.makeEspresso();
        // Thread thread = new Thread(runnable);
        // thread.start();
        
        // Using method reference to static method
        // Runnable runnable = CoffeeMaker::makeEspresso;
        // Thread thread = new Thread(runnable);
        // thread.start();

        CoffeeMaker cm = new CoffeeMaker();
        Runnable r = CoffeeMaker::makeEspresso;
        r.run();
    }
}
Method Reference: CoffeeMaker::makeEspresso is a method reference to the static method makeEspresso in the CoffeeMaker class. This is more concise and directly refers to the method without specifying any parameters.

**2. Reference to an Instance Method of a Particular Object**
 class CoffeeMachine {
    public void brewCoffee() {
        System.out.println("Brewing coffee...");
        // Steps to brew coffee
        System.out.println("Coffee is ready!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Using lambda expression
        // CoffeeMachine myCoffeeMachine = new CoffeeMachine();
        // Runnable runnable = () -> myCoffeeMachine.brewCoffee();
        // Thread thread = new Thread(runnable);
        // thread.start();
        
        // Using method reference to instance method
        CoffeeMachine myCoffeeMachine = new CoffeeMachine();
        Runnable runnable = myCoffeeMachine::brewCoffee;
        // Thread thread = new Thread(runnable);
        // thread.start();
        runnable.run();
    }
}
Method Reference: myCoffeeMachine::brewCoffee is a method reference to the instance method brewCoffee of the myCoffeeMachine object. This directly references the instance method on a specific instance of CoffeeMachine.


18.<!-- Stream API in JAVA -->
Java's Stream API provides a functional approach to process collections of objects. It allows operations like filtering, mapping, and reducing elements with concise lambda expressions. Streams can be sequential or parallel, facilitating efficient, multicore processing. 

Operations are lazily executed, enhancing performance by avoiding unnecessary computations. Terminal operations like `forEach`, `reduce`, and `collect` produce results or side-effects when streams are processed. Intermediate operations like `filter`, `map`, and `sorted` transform streams without affecting the source data.

Overall, the Stream API enhances productivity by streamlining complex data manipulation tasks in Java programming.

**1.finding max min using stream api.**
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.Optional;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(3, 1, 7, 4, 10, 5, 8, 2, 6, 9);

        // Find maximum element
        Optional<Integer> max = numbers.stream()
                                    //    .max(Comparator.naturalOrder());
                                    .max((ele1,ele2)->ele1.compareTo(ele2));
        System.out.println("Max Element is: "+max.get());
        //.get() => returns the non-null value described by this Optional or Throws:NoSuchElementException - if no value is present

        // Find minimum element
        Optional<Integer> min = numbers.stream()
                                       .min(Comparator.naturalOrder());
        System.out.println("Min Element is: "+min.get());
    }
}

**2.Sum of all elements**
import java.util.Arrays;
import java.util.List;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // Filter even numbers and calculate their sum
        int sum = numbers.stream()
                         //.filter(n -> n % 2 == 0) if we only want the sum of even no the we can use filter.
                         .mapToInt(n -> n)         // Convert Integer to int (not necessary in this case)
                         .sum();                   // Sum all elements

        System.out.println("Sum of even numbers: " + sum);
    }
}
// mapToInt() is used to convert each Integer to int (necessary to use sum()).

**3.finding longest String**
import java.util.Arrays;
import java.util.List;

public class StreamExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "orange", "strawberry", "blueberry");

        // Find the longest string
        String longestString = words.stream()
                                    .max((s1, s2) -> s1.length() - s2.length())
                                    .orElse(null);

        System.out.println("Longest string: " + longestString);
    }
}


19.<!-- Static and Default Methods in JAVA -->
Static Methods in Java: Static methods in Java interfaces are methods that can be directly called using the interface name, without needing an object of a class that implements the interface.They are declared with the static keyword and are used for providing utility methods or common functionality related to the interface itself, which cannot be overridden by implementing classes.

Default Methods in Java: Default methods in Java interfaces are methods that have a default implementation provided in the interface itself. Implementing classes can choose to use this default implementation or override it with their own implementation. They are declared with the default keyword and allow interfaces to evolve by adding new methods without breaking compatibility with existing implementations.


// Interface with a default method
interface Animal {
    void move(); // abstract method

    default void makeSound() {
        System.out.println("Some generic sound");
    }
    
    // Static method to calculate average lifespan
    static double averageLifespan() {
        // Dummy implementation for demonstration
        return 15.0; // Average lifespan in years
    }
        
}

// Implementing classes with overridden default method
class Dog implements Animal {
    @Override
    public void makeSound(){
        System.out.println("Bow Bow");
    }

    @Override
    public void move() {
        System.out.println("Dog is running");
    }
}

class Cat implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow");
        
    }

    @Override
    public void move() {
        System.out.println("Cat is jumping");
    }
}

// Main method for testing
public class Main {
    public static void main(String[] args) {
        // Calling the static method from the interface
        System.out.println("Average Lifespan of Animals: " + Animal.averageLifespan() + " years");

        Animal dog = new Dog();
        dog.makeSound(); // Calls the overridden method makeSound() from Animal interface
        dog.move();      // Calls the overridden method move() from Dog class
        
        Animal cat = new Cat();
        cat.makeSound(); // Calls the overridden method makeSound() from Cat class
        cat.move();      // Calls the overridden method move() from Cat class
    }
}

20.<!-- Try with Resources in JAVA -->
Try-with-resources in Java simplifies resource management by automatically closing resources that implement `AutoCloseable`.
It ensures that each resource declared within its parentheses is closed at the end of the statement, handling exceptions gracefully. This feature reduces code for manual resource management, such as closing files or database connections, and replaces traditional `try-finally` blocks.
By leveraging try-with-resources, Java promotes cleaner and more readable code, enhancing reliability by enforcing proper resource cleanup without the need for explicit `finally` blocks or manual `close()` calls.

import java.io.*;
public class TryWithResourcesExample {
    public static void main(String[] args) {
        // Example of reading from a file using FileReader (Java 7+)
        try (FileReader reader = new FileReader("example.txt")) {
            // Read from the file
            int character;
            while ((character = reader.read()) != -1) { 
                // implicitly character
                // .read() => Reads a single character.
                System.out.print((char) character);// converting int to char explicitly
            }
        } catch (IOException e) {
            // Handle exceptions
            System.out.println("Error reading file: " + e.getMessage());
        }
    }
}
// The resource must implement AutoCloseable or Closeable interfaces for it to be used with "try-with-resources". In the example, FileReader is used, which automatically implements AutoCloseable.
<-----------------------------------OR------------------------------>
**USING CUSTOM CLASS**
class Counter implements AutoCloseable {

    private int count;

    // constructor
    public Counter() {
        this.count = 0;
        System.out.println("Counter initialized.");
    }

    public void increment() {
        count++;
        System.out.println("Counter incremented to: " + count);
    }

    @Override
    public void close() {
        System.out.println("Closing counter.");
    }
}
public class Main {
     
    public static void main(String[] args) {
        // Example of using Counter with try-with-resources
        try (Counter counter = new Counter()) {
            // Use the counter
            counter.increment();
            counter.increment();
            counter.increment();
        } catch (Exception e) {
            // Handle exceptions
            System.err.println("Exception occurred: " + e.getMessage());
        }
    }
}
When the try block begins execution, the Counter instance is created (new Counter()). This invokes the constructor of Counter, which prints "Counter initialized.".

The Counter class implements AutoCloseable, which means it provides a method close() to release or clean up any resources it holds.

At the end of the try block (whether it completes normally or an exception is thrown), the Java runtime automatically calls the close() method on counter.

This is the essence of try-with-resources: ensuring that resources (like Counter here) are properly closed after they are no longer needed, without requiring explicit finally blocks or manual close() calls.

21.<!-- Sealed Class in JAVA -->
In Java, a sealed class is a type of class that restricts which other classes or interfaces can extend or implement it. This restriction is enforced using the sealed keyword in the class declaration, followed by the specific subclasses or interfaces that are permitted to extend or implement the sealed class, denoted by the permits keyword. This feature enhances code maintainability and security by explicitly defining and limiting the class hierarchy, preventing unauthorized extensions.

// Sealed superclass Animal with permitted subclasses Dog, Cat, and Bird
sealed class Animal permits Dog, Cat, Bird {
    private String name;

    // Constructor for initializing the name of the animal
    public Animal(String name) {
        this.name = name;
    }

    // Getter method to retrieve the name of the animal
    public String getName() {
        return name;
    }
}

// Final subclass Dog extending Animal
final class Dog extends Animal {
    // Constructor chaining to initialize the name via superclass constructor
    public Dog(String name) {
        super(name);
    }

    // Method specific to Dog: bark
    public void bark() {
        System.out.println(getName() + " the dog is barking.");
    }
}

// Final subclass Cat extending Animal
final class Cat extends Animal {
    // Constructor chaining to initialize the name via superclass constructor
    public Cat(String name) {
        super(name);
    }

    // Method specific to Cat: meow
    public void meow() {
        System.out.println(getName() + " the cat is meowing.");
    }
}

// Final subclass Bird extending Animal
final class Bird extends Animal {
    // Constructor chaining to initialize the name via superclass constructor
    public Bird(String name) {
        super(name);
    }

    // Method specific to Bird: chirp
    public void chirp() {
        System.out.println(getName() + " the bird is chirping.");
    }
}

// Main class to demonstrate the usage of Animal subclasses
public class Main {
    public static void main(String[] args) {
        // Create instances of Dog, Cat, and Bird
        Dog dog = new Dog("Buddy");
        dog.bark();

        Cat cat = new Cat("Whiskers");
        cat.meow();

        Bird bird = new Bird("Robin");
        bird.chirp();

    }
}
// Uncommenting the following would result in a compilation error
// Animal animal = new Animal("Generic"); // is commented out because Animal is a sealed class and cannot be instantiated directly.

super(name) to invoke the constructor of the superclass (Animal) and initialize the name field.


22.<!-- Base64 Encode and Decode in JAVA -->
**//1->Basic Encoding and Decoding Example**
import java.util.Base64;
public class Base64Example {
    public static void main(String[] args) {
        // Original string to encode
        String originalInput = "Hello, World!";
        
        // Encoding to Base64
        String encodedString = Base64.getEncoder().encodeToString(originalInput.getBytes());
        System.out.println("Encoded string: " + encodedString);
        
        // Decoding from Base64
        byte[] decodedBytes = Base64.getDecoder().decode(encodedString);
        String decodedString = new String(decodedBytes);
        System.out.println("Decoded string: " + decodedString);
    }
}
originalInput.getBytes(): Converts the string Hello, World! into a byte array.
Base64.getEncoder().encodeToString(byte[]): Encodes the byte array into a Base64-encoded string.

Base64.getDecoder().decode(encodedString): Decodes the Base64-encoded string back into a byte array.
new String(decodedBytes): Converts the decoded byte array back into a string.

**//2->URL Encoding and Decoding in JAVA.**
import java.io.UnsupportedEncodingException;
import java.net.URLEncoder;
import java.net.URLDecoder;

public class URLEncodingDecodingExample {

    public static void main(String[] args) {
        // Original URL to be encoded and decoded
        String originalUrl = "https://www.example.com/search?q=java basics";

        // URL encoding
        try {
            // Encode the original URL using UTF-8 encoding
            String encodedUrl = URLEncoder.encode(originalUrl, "UTF-8");
            System.out.println("Encoded URL: " + encodedUrl);
            
            // URL decoding
            // Decode the encoded URL back to its original form using UTF-8 encoding
            String decodedUrl = URLDecoder.decode(encodedUrl, "UTF-8");
            System.out.println("Decoded URL: " + decodedUrl);
            
        } catch (UnsupportedEncodingException e) {
            // Handle UnsupportedEncodingException if UTF-8 encoding is not supported
            System.out.println("Unsupported encoding: " + e.getMessage());
        }
    }
}

// URLEncoder.encode(originalString, "UTF-8"): Encodes the original string into a URL-safe format using UTF-8 encoding.
// This method converts spaces into %20 and other special characters according to URL encoding rules.

// URLDecoder.decode(encodedString, "UTF-8"): Decodes the encoded string back into its original form using UTF-8 encoding.
// This method reverses the encoding process, converting %20 back into spaces and %21 back into !.

**//3->MIME Encoding and Decoding in JAVA**
import java.util.Base64;
public class MIMEEncodingDecodingExample {

    public static void main(String[] args) {
        // Example binary data (byte array)
        byte[] binaryData = { 0x4A, 0x61, 0x76, 0x61};

        // MIME encoding
        String encodedData = Base64.getMimeEncoder().encodeToString(binaryData);
        System.out.println("Encoded MIME data:");
        System.out.println(encodedData);

        // MIME decoding
        byte[] decodedData = Base64.getMimeDecoder().decode(encodedData);
        System.out.println("Decoded data:");
        System.out.println(new String(decodedData));
    }
}
// Binary Data: byte[] binaryData = { 0x4A, 0x61, 0x76, 0x61};
// Represents example binary data (in this case, ASCII characters "Java").

// Base64.getMimeEncoder().encodeToString(binaryData): Encodes the binary data into a MIME-compatible Base64-encoded string.
// Generates a Base64-encoded string that includes MIME headers and line breaks suitable for MIME applications like email attachments.

// Base64.getMimeDecoder().decode(encodedData): Decodes the MIME-encoded string back into its original binary form.
// Converts the Base64-encoded string back into bytes.

