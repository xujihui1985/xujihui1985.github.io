---
layout: post
title: "Class Loading"
description: "Java Class Loading"
category: "java"
tags: [class loading]
---
{% include JB/setup %}


#### The java classes
1. Core classes
2. Extension classes

Core classes exists in jre/lib/rt.jar while ext classes exists in jre/lib/ext/

the location of ext can be set by `java -cp classes -Djava.ext.dirs=c:\lib`

java一般使用两个path：classpath 和 java.library.path
1. classpath是指向jar包的位置
2. java.library.path是非java类包的位置如（dll,so）

LINUX下的系统变量LD_LIBRARY_PATH来添加java.library.path 或者 在vm arguments里添加-Djava.library.path= /usr/local/lib

### Delegation modal

set classpath on command line

```
java -cp classes;lib\helper.jar com.sean.Main

```

java is going to find com.sean.Main class under classes directory, beside the classpath you provided, java will also find class from 

1. `$JAVA_HOME/lib/rt.jar` which contains core java classes
2. `$JAVA_HOME/lib/ext/*.jar` which contains extension class


假设我自定义了一个String类，并且也放在java.lang package下，当使用java来执行这个方法的时候，会发生什么情况？

java仍然会找到rt.jar中的String class，因为java使用委托模型，先由BootstrapClassLoader加载，然后再由ExtensionClassLoader, 最后由ApplicationClassloader加载，由于已经在BootstrapClassLoader中加载了String class, 所以我自己定义的String class不会被加载

#### classLoaders

the sequence of classloader is 
`Bootstrap class loader` ->  `Extension class loader` -> `Application class loader`

```
package com.sean;

import java.net.URL;
import java.net.URLClassLoader;

public class Delegation {

	public static void main(String[] argss) {

		URLClassLoader classLoader = (URLClassLoader) ClassLoader.getSystemClassLoader();
		do {
			System.out.println(classLoader);
			for (URL url : classLoader.getURLs()) {
				System.out.printf("\t %s\n", url.getPath());
			}
		} while((classLoader = (URLClassLoader) classLoader.getParent()) != null);
		System.out.println("Bootstrap classloader");
	}
}

```

#### implement Our Own Class Loader

```
package com.sean;

public class SqlServerClassLoader extends ClassLoader {
  private ClassLoader parent;
  private String connectionString;

  public SqlServerClassLoader(String connectionString) {
    this(ClassLoader.getSystemClassLoader(), connectionString);
  }

  public SqlServerClassLoader(ClassLoader parent, String connectionString) {
    super(parent);
    this.parent = parent;
    this.connectionString = connectionString;
  }

  protected Class<?> findClass(String name) throws ClassNotFoundException {
    Class cls = null;
    try {
      // load the class by parent classLoader
      cls = parent.loadClass(name);
    } catch(ClassNotFoundException e) {
      // if not found, try load the class by this classLoader
      byte[] bytes = new bytes[0];
      try {
        bytes = loadClassFromDatabase(name);
      } catch(SqlException sqle) {
        throw new ClassNotFoundException();
      }
      return defineClass(name, bytes, 0, bytes.length);
    }
    return cls;

  }
private byte[] loadClassFromDatabase(String name) throws SQLException {
        PreparedStatement pstmt = null;
        Connection connection = null;

        try {
            connection = DriverManager.getConnection(connectionString);
            String sql = "select class from CLASSES where ClassName = ?";
            pstmt = connection.prepareStatement(sql);
            pstmt.setString(1, name);
            ResultSet rs = pstmt.executeQuery();
            if (rs.next()) {
                Blob blob = rs.getBlob(1);
                byte[] data = blob.getBytes(1, (int) blob.length());
                return data;
            }
        } catch(Exception e) {

        } finally {
            if(pstmt != null) {
                try {
                    pstmt.close();
                } catch(e) {

                }
            }
            if (connection != null) {
                try {
                    connection.close();
                } catch(e) {
                }
            }
        }
        return null;
    }

}
```

### Class.forName

```
ClassLoadedr ucl = new URLClassLoader();
Class clazz = Class.forName("com.sean.Hello", true, ucl);

Class.forName is used to load class by name, and it can also provide a class loader to load the class
```

**if two same class loaded by two classloader, they are now same**

```
...
URLClassLoader ucl1 = new URLClassLoader(...);
URLClassLoader ucl2 = new URLClassLoader(...);

Class clazz1 = Class.forName("com.sean.Hello", true, ucl1);
Class clazz2 = Class.forName("com.sean.Hello", true, ucl2);

IHello h1 = (IHello)clazz1.newInstance();
IHello h2 = (IHello)clazz2.newInstance();


clazz1 != clazz2
h1.getClass() != h2.getClass()

```


### difference between Class.forName and ClassLoader.loadClass

1. they both return Class from classLoader
2. use Class.forName to load class will
	1. initialize the class all ** static initializer** will be run
3. use ClassLoader.getSystemClassLoader().loadClass("SomeClass")
	1. use the system class loader
	2. not initialize the class


### Reflection

1. difference between Class.getMethods and Class.getDeclaredMethods
	1. getMethods returns all the methods defined on the class inclucing the method from parent class
	2. getDeclaredMethods only return the methods defined on the Class, including private methods

2. load constructor
	```
		Constructor[] ctors = cls.getDeclaredConstructors();
		for(Constructor ctor : ctors) {
			ctor.getParameterTypes();
			System.out.println(ctor.getName());
		}
	```
3. execute the code
	```
	method.invoke(objectToBeInvoked, parameters);
	```