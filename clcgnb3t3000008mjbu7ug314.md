# Facade Design Pattern in C#

The Facade Design Pattern is a structural design pattern that provides a simplified interface to a complex system. It acts as a single entry point to the system and hides the complexity of the underlying components. The Facade Design Pattern is useful when you want to provide a simple interface to a complex system, or when you want to decouple the client from the implementation details of the system.

## **When to use the Facade Design Pattern**

The Facade Design Pattern should be used when:

* You want to provide a simple interface to a complex system
    
* You want to decouple the client from the implementation details of the system
    
* You want to improve the readability and maintainability of the code
    

## **Example of the Facade Design Pattern**

Let's consider an example of a computer system that has multiple components such as the CPU, memory, and hard drive. The computer system has a simple interface that allows the user to turn the system on and off, but the underlying components have complex interfaces that are not accessible to the user.

To implement the Facade Design Pattern, we can create a `Computer` class that has a simple interface that allows the user to turn the system on and off. The `Computer` class will have a reference to the `CPU`, `Memory`, and `HardDrive` classes, which represent the underlying components of the system.

```csharp
class CPU {
  void start() {
    // implementation details
  }

  void shutdown() {
    // implementation details
  }
}

class Memory {
  void load(long position, byte[] data) {
    // implementation details
  }
}

class HardDrive {
  byte[] read(long lba, int size) {
    // implementation details
  }
}

class Computer {
  private CPU cpu;
  private Memory memory;
  private HardDrive hardDrive;

  Computer() {
    this.cpu = new CPU();
    this.memory = new Memory();
    this.hardDrive = new HardDrive();
  }

  void startComputer() {
    cpu.start();
    memory.load(BOOT_ADDRESS, hardDrive.read(BOOT_SECTOR, SECTOR_SIZE));
    cpu.jump(BOOT_ADDRESS);
    cpu.execute();
  }

  void shutDownComputer() {
    cpu.shutdown();
  }
}
```

With the above implementation, the user can turn the computer on and off using the simple `startComputer` and `shutDownComputer` methods, without having to worry about the complexity of the underlying components.

## Real-World Example: ATM machine

One real-world example of the Facade Design Pattern is the use of an ATM machine. An ATM machine provides a simple interface for users to perform financial transactions, such as withdrawing cash or checking their account balance. However, behind the scenes, the ATM machine is connected to a complex system that includes the bank's database, security protocols, and network infrastructure.

To implement the Facade Design Pattern in this scenario, we can create a `Bank` class that acts as the facade. The `Bank` class has a simple interface that allows the user to perform transactions, such as withdrawing cash or checking their account balance. The `Bank` class also has references to the underlying components, such as the `Security` class, which handles security protocols, and the `Database` class, which handles access to the bank's database.

```csharp
class Security {
  boolean checkPin(String pin) {
    // implementation details
  }
}

class Database {
  Account getAccount(String accountNumber) {
    // implementation details
  }

  void updateAccount(Account account) {
    // implementation details
  }
}

class Bank {
  private Security security;
  private Database database;

  Bank() {
    this.security = new Security();
    this.database = new Database();
  }

  boolean withdrawCash(String accountNumber, String pin, double amount) {
    Account account = database.getAccount(accountNumber);
    if (security.checkPin(pin) && account.getBalance() >= amount) {
      account.setBalance(account.getBalance() - amount);
      database.updateAccount(account);
      return true;
    }
    return false;
  }

  double checkBalance(String accountNumber, String pin) {
    Account account = database.getAccount(accountNumber);
    if (security.checkPin(pin)) {
      return account.getBalance();
    }
    return -1;
  }
}
```

With the above implementation, the user can perform transactions using the simple `withdrawCash` and `checkBalance` methods, without having to worry about the complexity of the underlying components. The Facade Design Pattern in this example decouples the user from the implementation details of the system and provides a simple interface for performing financial transactions.

## **Advantages**

* It provides a simple interface to a complex system
    
* It decouples the client from the implementation details of the system
    
* It improves the readability and maintainability of the code
    

## **Disadvantages**

* It can make the system more inflexible, as the client is not aware of the implementation details of the underlying components
    
* It can increase the overall size of the code, as it requires the creation of an additional layer of abstraction
    

## **Conclusion**

In conclusion, the Facade Design Pattern is a useful tool for providing a simple interface to a complex system and decoupling the client from the implementation details of the system. It can improve the readability and maintainability of the code, but it can also make the system more inflexible