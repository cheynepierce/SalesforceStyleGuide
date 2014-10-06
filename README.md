##Contents
* [Introduction](#introduction)
* [Custom Objects](#objects)
* [Custom Fields](#fields)
* [Workflow Rules](#workflow)
* [Apex](#apex)
* [Visualforce](#visualforce)
* [Other](#other)

##<a name="introduction"></a>Introduction

One of the great things about Salesforce is that it is intuitive and generally easy to use, but 
this ease of use can be greatly diminished if the conventions used across a Salesforce instance are inconsistent. 
This document provides some basic guidelines for object naming conventions, Apex code style, etc. 
None of these are hard and fast rules, but the aim is to maximize consistency across a Salesforce deployment 
in order to make it easier to maintain and add new features, as well as easier for new administrators and 
developers to begin working in the environment. 

##<a name="objects"></a>Custom Objects
Custom object names should accurately and succinctly describe the business object that they represent. 
They should be written in the singular form, with underscores separating words.

Bad:
```
StudentPrograms__c
```

Good:
```
Student_Program__c
```

Object labels should match their name, with spaces taking the place of underscores.

```
Name: Student_Program__c, Label: Student Program
```

Don't give a custom object the same name or label as a standard object. The API names will be different, 
but it is still impossible to tell them apart when selecting them from a menu, such as when creating a workflow rule.

##<a name="fields"></a>Custom Fields
Custom field names should follow the same conventions as custom object names. 

Ideally, the field's purpose should be clear from the name. 
In cases where it is not, enter descriptions in the Help Text and Description areas. 
It's okay to make frequent use of the Description field, since it can be helpful to have 
a high level of documentation, but only use the Help Text if it's absolutely necessary. 
The Help Text appears on the page layout, and may not be as useful if too much information is provided.

In cases where a field represents the same type of data as a field on a different object, 
use the same name whenever possible. For example, Postal Code fields should always be called 
Postal Code, instead of Zip or Zip Code, since standard Postal Code fields already exist. 
Also, define the field in the same way: i.e., make sure text 
fields representing the same data have the same length, and make sure validation rules are 
the same. This is especially important when defining lead fields that will map upon conversion, 
to avoid truncating data.

Bad:
```
Object_1__c.Postal_Code__c, Object_2__c.Zip_Code__c
```

Good:
```
Object_1__c.Postal_Code__c, Object_2__c.Postal_Code__c
```

Unless there is a compelling reason to do otherwise, name master-detail or lookup fields to match the corresponding object name. 
There are often exceptions here, especially in the cases where standard objects are used: 
for example, if an object represents a Class, and has a lookup to an Account, then the lookup field might be called School.

##<a name="workflow"></a>Workflow and Validation Rules
Be cautious to not accidentally make fields required through validation rules 
when your intention is to enforce a certain rule only when a value is entered.

Bad:
```
NOT(REGEX(\d{3}-\d{2}-\d{4}, Phone))
```

Good:
```
AND(NOT(ISBLANK(Phone)), NOT(REGEX(\d{3}-\d{2}-\d{4}, Phone)))
```

In the first case, the data will not be considered valid if it is completely blank, 
which may or not be what you want.

Give workflow rules names that clearly describe their purpose. 
If further details are needed, always include a description. 
What you are doing may seem obvious now, but for all but the simplest rules, 
it will not be obvious 6 months down the road.

Do not set workflow criteria to "True" unless absolutely necessary. Always use more specific 
criteria, when possible, since it makes it the rule's purpose clearer.

##<a name="apex"></a>Apex Code
Apex code is probably the area of Salesforce where using a set of conventions is the most important. 
It is far easier to read code that is written in a consistent manner than code that is written haphazardly, 
using no set of standards.
With any code, it is probable that someone will have to read the code somewhere down the line, 
so it is imperative to write it in a clean, consistent manner.

####Naming Conventions
Class names should be nouns that clearly describe their purpose. 
Don't use abbreviations in the name unless they are obvious and the class name would be too long if not using them. 
CamelCase should be used for class names, with the first letter capitalized. 

Bad:
```
revenueScheduler
```

Good:
```
RevenueScheduler
```

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

Good:
```
for (Account acct : accounts) {
    acct.Name = 'New name';
}
```

Class methods should be separated by blank lines. 

Sparingly, blank lines can be used to separate logical sections of code.

Don't use blank spaces before or after parentheses in method names.

```
Bad: String s = getString ( 1 );
```

```
Good: String s = getString(1);
```

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
//Construct a map that will allow us to easily access a list of contacts based on AccountId
for (Account acct : accounts) {
    List<Contact> contacts = new List<Contact>();
	for (Contact c : acct.Contacts) {
	    contacts.add(c);
	}
	accountMap.put(acct.Id, contacts);
}
```
Comments should be clearly written and follow all grammatical rules.

####Debug Statements
Use debug statements at critical points, such as in catch blocks, so that when problems 
arise in production, it is easier to find their source. 

Bad:
```
try {
    http.send(req);
} catch (Exception e) { 
	//Do nothing
}
```

Good:
```
try {
	http.send(req);
} catch (Exception e) {
	system.debug('Error sending http request: ' + e);
}
```

##<a name="visualforce"></a>Visualforce
Always indent blocks that span multiple lines.

Bad:
```
<div id="block1">
some text
</div>
```

Good:
```
<div id="block1">
    some text
</div>
```

When using ```<apex:>``` tags, always provide an element ID, as it is necessary to reference the full path to the element 
using the {!$Component} variable when referencing these tags by ID from javascript. 

Bad:
```
<apex:pageBlock>
    <apex:outputText id="outputText"></apex:outputText>
</apex:pageBlock>
<script>
    //This will not work
    var outputText = document.getElementById('outputText').value;
</script>
```

Good:
```
<apex:pageBlock id="outputBlock">
    <apex:outputText id="output"></apex:outputText>
</apex:pageBlock>
<script>
    var outputText = document.getElementById('{!$Component.outputBlock.outputText}').value;
</script>
```

##<a name="other"></a>Other

####Sandboxes

Sandboxes should be named in a way that accurately describes their purpose. 
For example, a sandbox that is used for development purposes could be called "dev" 
and a sandbox made for training purposes could be called "training". 

When developing in a sandbox, always use a deployment tool, such as change sets or the metadata API, 
to move the changes to production, in order to ensure that the customizations are consistent between environments. 
It can be frustrating to deploy code to production and have to debug issues that came up as a 
result of subtle differences between field definitions.

####Installed Apps
Installed apps are meant to be able to be dropped in to an existing implementation, adding functionality. 
Try not to reference components from installed apps in areas outside of the app, as it can make it extremely 
difficult to un-install the app later on.