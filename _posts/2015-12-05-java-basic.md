---
layout: post
title: "java basic"
description: ""
category: ""
tags: [java]
---
{% include JB/setup %}


#### Exception Class Hierarchy


## Object

### Throwable

#### Error     

> Virtual machine related errors, we don't really handle these errors very offen

> eg: LinkageError

#### Exception

RuntimeException

> Repersent errors in your program, eg: NullPointerException, runtime exception are consider unchecked exception


IOException  

> Checked exceptions, checked exceptions must be handled


any classes that inherite from Exception and not inherite from RuntimeException are consider a Checked Exception

### Package Importation

- Types in current package do not need to be qualified
- Types in the java.lang package do not need to be qualified


Import on Demand

```
import com.sean.package.*
```

Single type import  **Preferred way to import types**

```
import com.sean.package.MyClassA
```


#### Access Modifiers

no access modifier:  Only within its own package
public
privat:  only within its own class, can not used on Type
protected


#### Nested Types

purposes for structure and scoping, No relationship between instances of nested and enclosing type

```

public class Passenger implements Comparable {
  
    public static class RewardProgram {
        private int memberLevel;
		private int memberDays;
    }

	private RewardProgram rewardProgram = new RewardProgram();
}

```

#### Inner Classes

Each instance of the nested class is associated with an instance of the enclosing class


```

public class Flight {
	
	private CrewMember[] crew;
	private Passenger[] roster;

	public Iterator<Person> iterator() {
		return new FlightIterator()
    }

// none static
	private class FlightIterator 
		implements Iterator<Persion> {
		private int index = 0;
		public boolean hasNext() {
			return index < (crew.length + roster.length);
		}

		public Person next() {
			// Flight.this.crew
			Person p = (index < crew.length) ? crew[index] : roster[index-crew.length];
			index++;
			return p;
		}
	}
}

```

in FlightIterator,  crew represent for `Flight.this.crew`

#### Anonymous Classes

Anonymous classes are inner classes, Create as if you are constructing an instance of the interface or base class

the iterator example previously seen can be rewritten in anonymous class

```
...
public Iterator<Person> iterator() {

	return new Iterator<Person>() {
		private int index = 0;
		public boolean hasNext() {
			new index < (crew.length + roster.length);
		}
...
	}
}

```