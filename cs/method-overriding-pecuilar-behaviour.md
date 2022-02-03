## Question?

- Parent class has method A and B. A internally calls B.
- Child class extends Parent and overrides B.
- When we call child.A() which B() method would be called by A? Parent's or child class?

## C++

#### Code

```cpp
#include <iostream>
using namespace std;

class Parent {
public:
	void A() {
		cout<<"Parent A"<<"\n";
		B();
	}
	void B() {
		cout<<"Parent B"<<"\n";
	}
};

class Child: public Parent {
public:
	void B() {
		cout<<"Child B"<<"\n";
	}
};

int main() {
	// Parent p;
	// p.B();
	// p.A();
	
	// cout<<"\n";
	
	Child c;
	// c.B();
	c.A();
	return 0;
}
```

#### Output

```
Parent A
Parent B
```

## Golang

#### Code

```go
package main

import "fmt"

type TestI interface {
	A()
	B()
}

type Parent struct {
}

func (p Parent) A() {
	fmt.Println("Parent A")
	p.B()
}

func (p Parent) B() {
	fmt.Println("Parent B")
}

type Child struct {
	Parent
}

func (c Child) B() {
	fmt.Println("Child B")
}

func main() {
	// p := Parent{}
	// p.B()
	// p.A()

	// fmt.Println("\n")
	c := Child{}
	// c.B()
	c.A()

}
```

#### Output

```
Parent A
Parent B
```

## Java

#### Code

```java
import java.util.*;
import java.lang.*;
import java.io.*;

/* Name of the class has to be "Main" only if the class is public. */
class Parent 
{
	public void A() 
	{
		System.out.println("Parent A");
		this.B();
	}
	
	public void B() 
	{
		System.out.println("Parent B");
	}
}
	
class Child extends Parent 
{
	public void B() 
	{
		System.out.println("Child B");
	}
}
	
	
class Ideone
{	
	public static void main (String[] args) throws java.lang.Exception
	{
		// Parent p = new Parent();
		// p.B();
		// p.A();
		// System.out.println("\n");
		
		Child c = new Child();
		// c.B();
		c.A();
	}
}
```

#### Output

```
Parent A
Child B
```

## Python

#### Code

```python
class Base:
    def A(self):
        print("Base: A")
        self.B()
    
    def B(self):
        print("Base: B")

class Derived(Base):
    def B(self):
        print("Dervived: B")

def main():
    # b = Base()
    # b.B()
    # b.A()

    # print("\n")

    d = Derived()
    # d.B()
    d.A()

if __name__ == "__main__":
    main()
```

#### Output

```
Base: A
Dervived: B
```