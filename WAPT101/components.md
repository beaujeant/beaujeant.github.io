---
layout: default
permalink: /WAPT101/components/
title: Components for a web application
---

| [<<](https://beaujeant.github.io/WAPT101/introduction/) | [Table of content](https://beaujeant.github.io/WAPT101/) | [>>](https://beaujeant.github.io/WAPT101/http/) |

Components for a web application
--------------------------------

Back in the days, websites were a collection of static pages that a users could browse without any interaction. Developers wanted more from website, like for instance the possibility to provide dynamic content where administrators could easily edit/add/delete content, or create some restricted areas with granular access control. Simple web server were not enough, therefore new technologies came in place. _Scripting languages_ and _frameworks_ were developed to add some logic in web applications and _database_ started to be use to easily store and manipulate dynamic content.

Here is an example that put all the components together. Let say you want to read the latest post on this trendy threat "My little poney" of your knitted's group forum. You click on the title. You browser will send an _HTTP request_ to the web server. The web server will parse the HTTP request to identify the resource requested together with the query (i.e. read the content of the thread "My little poney"). The web server will then pass it to the framework that will parse and execute the resource requested with that specific query in order to build the final page with its custom texts. The custom texts are stored in the database, therefore, the framework will send a _SQL_ query to the database and ask for all posts in the thread "My little poney", and for each post, a subsequent query will be sent to get the profile of the authors. Once the page built, the framework will send the result to the web server that will wrap it into a HTTP response to convey the content to the browser.

Here is a non-exhaustive list of web servers, frameworks and database management systems:

__Web servers:__
* Apache
* Nginx
* IIS

__Frameworks:__
* PHP
* ASP.NET
* JavaServer Page
* Ruby on Rails

__Databases:__
* MySQL
* MSSQL
* Oracle
* SQLite3
* MongoDB
* PostgreSQL
* IBM DB2

> Here to be honest, I'm not really sure about the terminology, I might be mixing up a bit scripting language, framework, technology, etc.

Each components use a different language, e.g. apache uses HTTP, JavaServer Pages uses Java and MySQL uses SQL. It is important to understand those languages and protocols if you want to find vulnerabilities in web application, and ultimately fixing them.

Most of the time, the user go on web server to view web pages. Those pages are written in _HTML_, _CSS_ and _JavaScript_ and can embed resources such as pictures, videos, Flash, etc. Here is a short description for each elements:
 * __HTML__ is used to define the elements (blocks, text, images, etc) and give the structure of the web page.
 * __CSS__ is meant for the design of the page, with the colors, margins, positions, etc.
 * __JavaScript__ allows you to add some logic on the browser side to improve the user experience, e.g. creating a cascading menu when the pointer is on top of a text.

> Note: JavaScript is only meant to improve the user experience, never for security reason.

| [<<](https://beaujeant.github.io/WAPT101/introduction/) | [Table of content](https://beaujeant.github.io/WAPT101/) | [>>](https://beaujeant.github.io/WAPT101/http/) |
