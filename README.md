##Contents
* [Introduction](#introduction)
* [Custom Object and Field Definitions](#objects)
* [Workflow Rules](#workflow)
* [Apex](#apex)
* [Visualforce](#visualforce)

##<a name="introduction"></a>Introduction

One of the great things about Salesforce is that it is intuitive and generally easy to use. 
This document outlines some basic guidelines for object naming conventions, Apex code style, etc. 
None of these are hard and fast rules, but the aim is to maximize consistency across a Salesforce deployment 
in order to make it easier to maintain, easier to add new features, and easier for new administrators and 
developers to work in the environment. 

##<a name="objects"></a>Custom Object and Field Definitions
Custom object names should accurately and succinctly describe the business object that they represent.

They should be written in the singular form, with underscores separating words.

Bad: StudentPrograms__c
Good: Student_Program__c

Custom field names should follow the same convention as custom object names. In cases where a field represents the same type of data as a field on a different object, use the same name whenever possible

Bad: Object_1__c.Postal_Code__c, Object_2__c.Zip_Code__c
Good: Object_1__c.Postal_Code__c, Object_2__c.Postal_Code__c

Master-detail or lookup fields should typically match the corresponding object name. There can be exceptions: for example, if an object represents a Class, and has a lookup to an Account, then the lookup field might be called School.

Some specific cases:

* Postal Code fields should always be called Postal Code, to match the standard Salesforce fields, never Zip Code

##<a name="workflow"></a>Workflow Rules

##<a name="apex"></a>Apex Code

##<a name="visualforce"></a>Visualforce
