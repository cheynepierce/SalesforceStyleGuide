##Contents
* [Introduction](#introduction)
* [Custom Object and Field Definitions](#objects)
* [Workflow Rules](#workflow)
* [Apex](#apex)
* [Visualforce](#visualforce)
* [Other](#other)

##<a name="introduction"></a>Introduction

One of the great things about Salesforce is that it is intuitive and generally easy to use, but 
this ease of use can be greatly diminished if the conventions used across a Salesforce instance are inconsistent. 
This document provides some basic guidelines for object naming conventions, Apex code style, etc. 
None of these are hard and fast rules, but the aim is to maximize consistency across a Salesforce deployment 
in order to make it easier to maintain, easier to add new features, and easier for new administrators and 
developers to begin working in the environment. 

##<a name="objects"></a>Custom Object and Field Definitions
Custom object names should accurately and succinctly describe the business object that they represent.

They should be written in the singular form, with underscores separating words.

```Bad: StudentPrograms__c```

```Good: Student_Program__c```

Custom field names should follow the same convention as custom object names. 
In cases where a field represents the same type of data as a field on a different object, 
use the same name whenever possible

```Bad: Object_1__c.Postal_Code__c, Object_2__c.Zip_Code__c```

```Good: Object_1__c.Postal_Code__c, Object_2__c.Postal_Code__c```

Master-detail or lookup fields should typically match the corresponding object name. 
There can be exceptions, typically in the cases where standard objects are used: 
for example, if an object represents a Class, and has a lookup to an Account, then the lookup field might be called School.

Some specific cases:

* Postal Code fields should always be called Postal Code, to match the standard Salesforce fields, never Zip Code

##<a name="workflow"></a>Workflow Rules

##<a name="apex"></a>Apex Code
Apex code is probably the area of Salesforce where using a set of conventions is the most important. 
It is far easier to read code that is written in a consistent manner than code that is written haphazardly, using no set of standards.
Since, with any code, it is probable that someone will have to read the code somewhere down the line, it is imperative to write it in a clean, consistent manner.

####Naming Conventions
Class names should be nouns that clearly describe their purpose. 
Don't use abbreviations in the name unless they are obvious and the class name would be too long if not using them. 
CamelCase should be used for class names, with the first letter capitalized. 

```Bad: revenueScheduler```

```Good: RevenueScheduler```

Visualforce controller names should be suffixed with the word Controller, i.e. `DonationPageController`. 
Similarly, controller extensions should be suffixed with the word Extension. 

Interface names should be prefixed with the letter I, i.e. `ITriggerHandler`.

####Formatting
Blocks of code should be indented.

Bad:
```
for (Account acct : accounts) {
acct.Name = 'New name';
}
```

Good;
```
for (Account acct : accounts) {
    acct.Name = 'New name';
}
```

Class methods should be separated by blank lines. 

Blank lines can be used to separate logical sections of code, but this should be done sparingly.

####Comments
In order to be useful, comments must accurately describe the code that they are commenting. It is best to avoid 
comments if the code itself is sufficient to understand what it does. Often, comments should explain 
why the code is there, and not simply describe what a particular line of code does. 

Bad:
```
//Loop through accounts and add related contacts to map
for (Account acct : accounts) {
    List<Contact> contacts = new List<Contact>();
	for (Contact c : acct.Contacts) {
	    contacts.add(c);
	}
	accountMap.put(acct.Id, contacts);
}
```

Good:
```
//Construct a map so that we can easily access a list of contacts based on AccountId
for (Account acct : accounts) {
    List<Contact> contacts = new List<Contact>();
	for (Contact c : acct.Contacts) {
	    contacts.add(c);
	}
	accountMap.put(acct.Id, contacts);
}
```


##<a name="visualforce"></a>Visualforce

##<a name="other"></a>Other

####Sandboxes

Sandboxes should be named in a way that accurately describes their purpose. 
For example, a sandbox that is used for development purposes could be called "dev" 
and a sandbox made for training purposes could be called "training". 
