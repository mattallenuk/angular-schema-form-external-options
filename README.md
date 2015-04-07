Angular Schema Form External Options
====================================
**Update 2015-04-07** I have only one last display related issue remaining to fix before I will push the more complete version of this repo with a working example to assist in implementation, I am aiming for 2015-04-12 or earlier.

**CURRENTLY IN DEVELOPMENT and not ready for production use** I released too soon and just prior to a lengthy holiday in case it may have helped someone to see the rough code. A more usable version, and example usage, is coming soon.

This is an add-on for [Angular Schema Form](https://github.com/Textalk/angular-schema-form/).

Ever wanted to load select options dynamically based on other fields in your Angular Schema Form?

The external options add-on can do that for your form and still maintain valid JSON Schema compliance.

Installation
------------
The external options plugin is an add-on to the Bootstrap decorator so far. To use it, just include
`bootstrap-external-options.min.js` *after* `bootstrap-decorator.min.js`.

Usage
-----
The external options add-on adds a new default mapping.

| Format Type         |   Becomes            |
|:--------------------|:--------------------:|
| "select-external"   |   A select drop down that loads options from an external URI |


| Schema                                          |   Default Form type  |
|:------------------------------------------------|:--------------------:|
| "type": "string" and "links[].rel": "options"   |   select-external    |


Filtering URI
-----------------

To add a filter to pass over the URI just add a filter called **externalOptionUri**
```javascript
.filter('externalOptionUri', function() {
  function externalOptionUriFilter(input){
    var current = input;
    if(typeof current === 'string') {
      current = current.replace(' ','-').toLowerCase();
    }
    return current;
  }

  return externalOptionUriFilter;
})
```

Example
-----------------
Below is an example. The variables use model.variable at the moment, considering changing that in v2 if I can find an easy way to handle it without that I am happy with.

```javascript
{
  "type": "object",
  "properties": {
    "state": {
      "title":"State",
      "type": "string"
    },
    "city": {
      "title":"City",
      "type": "string"
    },
    "suburb": {
      "title": "Suburb",
      "type": "string",
      "links":[
        { "rel":'options', "href":'./data/{model.state}/{model.city}/suburb.json' }
      ]
    }
  }
}
```

The loaded data must be in one of the following two formats:

**enum**
```javascript
{
  "title":"Suburb",
  "description":"Suburbs for Melbourne, Victoria",
  "enum":["Hawthorn","Melbourne","Richmond"]
}
```
**OR titleMap**
```javascript
{
  "title":"Suburb",
  "description":"Suburbs for Melbourne, Victoria",
  "titleMap":[
    {"name":"Hawthorn", "value":"Hawthorn"},
    {"name":"Melbourne",   "value":"Melbourne"},
    ...
    {"name":"Richmond", "value":"Richmond"}
  ]
}
```
