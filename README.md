ST558 Project 1
================
Yu Jiang
6/10/2020

# 1\. Describe JSON Data

## What is JSON?

  - Json stands for JavaScript Object Notation.

  - The lightweight format for storing and transporting data, specified
    by Douglas Crockford.

  - A way to store information in an organized, easy-to-access manner.

  - It has been extended from the JavaScript scripting language.

  - It was designed for human-readable data interchange.

## Where does it get used?

  - It is used when data is sent from a server to a web page.

  - The JSON format is used for serializing and transmitting structured
    data over network connection.

  - It is used while writing JavaScript based applications that includes
    browser extensions and websites.

  - Web services and APIs use JSON format to provide public data.

  - It can be used with modern programming languages.

## Why is it a good way to store data?

  - It is ‘self-describing’ and easy to understand.

  - It is a generic data format with a minimal number of value types:
    strings, numbers, booleans, lists, objects, and null.

  - It can be supported by many databases like PostgreSQL and MySQL.

  - JSON data is stored in a set of key-value pairs.

  - Spacing (spaces, tabs, new lines) does not matter in a JSON file.

For more information about JSON, please check [this
website](https://en.wikipedia.org/wiki/JSON).

# 2\. Three Packages for Reading JSON Data into R

## ‘rjson’

The package ‘rjson’ converts a JSON object into an R object.

## ‘RJSONIO’

The package ‘RJSONIO’ allows conversion to and from data in Javascript
object notation (JSON) format. This allows R objects to be inserted into
Javascript/ECMAScript/ActionScript code and allows R programmers to read
and convert JSON content to R objects.

## ‘jsonlite’

A fast JSON parser and generator optimized for statistical data and the
web.

I am going to choose the package ‘jsonlite’ since it is the fastest
among these three packages and this package offers flexible, robust,
high performance tools for working with JSON in R and is particularly
powerful for building pipelines and interacting with a web API.

# 3\. Write Functions to Contact the NHL Records

# 4\. Data Analysis
