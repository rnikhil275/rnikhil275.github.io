---
layout: post
title: What are you doing this summer ?
comments: True
---

The main reason for this post is because, I got tired of people asking me to explain my project. I shall be explaining things in brief here. 

## What is a [Web Framework]() ? 

From Wikipedia,

> A web framework (WF) or web application framework (WAF) is a software framework that is designed to support the development of web applications including web services, web resources and web APIs.

As it says, it's basically used to remove the same redundant overhead associated with creating web applications. Most web applications have/do the follwing things

- Database access, mapping, configuration
- Session Management
- User Interfaces
- Secure authorization and authentication
- URL routing/mapping

Web frameworks promote code re-use by providing easy ways to do the above mentioned stuff. They differ in each other in their architectural pattern, the most common one being the M(Database logic) V(User Interface) C(Business logic) [MVC architecture](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller).

I will be working on a Web Framework named [Sailor](http://sailorproject.org/) this summer. 

## Sailor
Sailor is a web development framwork and all applications are structed in a MVC(Model-View-Controller) architure. It uses a Javascript virtual machine for use of Lua in the browser if required. An example of the JS Virtual Machine can be found [here](https://github.com/paulcuth/starlight)

### Features

* Compatible with Lua 5.1, Lua 5.2 and LuaJIT
* MVC Structure
* Routing
* Friendly URL's
* Lua at Client using JS virtual machines deployed with the application
* Model generation from the database
* CRUD function generation using the models
* Validation module
* Object relational mapping([ORM](https://en.wikipedia.org/wiki/Object-relational_mapping
) layer for the database.)
* Form Generation
* Integrated Themes and layouts
* Runs on both *nix and windows

-----

<strong>What exactly am I doing for Sailor ?</strong>

<dl>
<dt>Admin Center</dt>
<dd>Most web frameworks generally have an admin center for editing configuration files, making controllers, models etc. Sailor has autogenerator fucntions which create models and controllers for you. My task is to encompass a configuration file editor, the autogen functions inside a protected URL for use in development. </dd>
<dt>Elasticsearch Integration</dt>
<dd><a href="https://www.elastic.co/products/elasticsearch">Elasticsearch</a> is a search database server based on <a href="https://lucene.apache.org">Apache Lucene</a>. It can be used to search all kinds of documents. It provides scalable search, has near real-time search, and supports multitenancy(One instance of a software being shared by multiple users). <br> There is a low level client for this in lua called <a href="https://github.com/DhavalKapil/elasticsearch-lua">elasticsearch-lua</a> and I shall be integrating this into Sailor. Once done, you can search an elasticsearch instance using the form module in Sailor. </dd>
</dl>

Also read [this](https://rnikhil275.github.io/2016/04/24/gsoc-2016/)



