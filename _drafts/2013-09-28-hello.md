---
layout: post
title: "Covariance and Contravariance"
description: ""
category: "Csharp"
tags: [Generic]
---
{% include JB/setup %}

## Overview

### New Keyword in Generic


	public interface IReadOnlyRepository<out T>
	
	public interface IWriteOnlyRepository<in T>



### What's the meaning?

First of all, Covariance and Contravariance can only be used in Interface or Delegate
In my point of view, the keyword `out` means the Generic Type can only be used as return value, it can not be used as parameter,we can use Derived class to replace the Generic Type.

and the keyword `in` means the Generic Type can only be used as parameter and can not be used as return value, we can use Base class to replace the Generic Type

### Still, What's that mean?
image we have three class

	public class Person {}
	
	public class Employee : Person {}
	
	public class Manager : Employee {}


now we have a generic repository

	public interface IRepository<T>  {}
	
	public void Dump(IRepository<Person> repository)
	{
	    // do sth
	}


what if I want to Dump both `Employee` and `Manager`?

we can use Covairance to help this out


	public interface IReadOnlyRepository<out T>
	{
	    T FindById(int id);
	}

 	public interface IRepository<T> : IReadOnlyRepository<T>
    {

    }

 	class ConcreateRepository : IRepository<Employee>
    {
        public Employee FindById(int id)
        {
            throw new System.NotImplementedException();
        }

        public void Add(Employee newEntity)
        {
            throw new System.NotImplementedException();
        }
    }

 	[TestFixture]
    public class CovarianceTest
    {
        public void DumpPeople(IReadOnlyRepository<Person> repository)
        {
            repository.FindById(1);
        }

        [Test]
        public void Covariance()
        {
            IRepository<Employee> rep = new ConcreateRepository();
            DumpPeople(rep);
        }
    }

as we can see `ConcreateRepository` implements `IRepository<Employee>` and Method `DumpPeople` accpet a parameter as `IReadOnlyRepository<Person>`, we still able to pass `ConcrateRepository` as parameter to `DumpPeople` this is the power of Covariance

On the other hand, if new requirement coming, said we need a method AddManager that accept a parameter as 
`IRepository<Manager>`

### How can our ConcreateRepository fit this requirement?

We need the power of Contravariance

 	public interface IWriteOnlyRepository<in T>
    {
        void Add(T newEntity);
    }
    public interface IRepository<T> : IWriteOnlyRepository<Manager>
    {

    }

 	[TestFixture]
    public class CovarianceTest
    {
       
        public void AddManager(IWriteOnlyRepository<Manager> repository)
        {
            repository.Add(new Manager());
        }

        [Test]
        public void Covariance()
        {
            IRepository<Employee> rep = new ConcreateRepository();
            AddManager(rep);
        }
    }

We create a new interface IWriteOnlyRepository<`in` T>

because `IRepository<T>` is the subclass of `IWriteOnlyRepository<Manager>` and Manager is BaseClass of Employee, so we could pass `ConcreateRepository` as parameter to `AddManager`