---
author: Joe Kampschmidt
layout: post
slug: useful-angularjs-filters
title: Useful AngularJS Filters
desc:  'A list of code examples for useful and easily reusable AngularJS filters.'
tags:
- angularjs
- javascript
---

I wanted to share some of the common AngularJS filters that I find myself using in numerous projects.

# Useful AngularJS Filters

- [Path Combine filter](#combine)
- [Boolean Mask filter](#boolmask)
- [Camel Case to Human Readable String filter](#camelcase)

<a name="combine"></a>
## Path Combine

    (function () {
      'use strict';

      angular.module('myapp').filter('filePathCombine', ['$filter', filePathCombine]);

      /*
        Works like C# Path.Combine()
        Example usage: {{ 'example.com' | filePathCombine:'test.html' }}
        Outputs: example.com\test.html
      */
      function filePathCombine() {

        return function (path1, path2) {

          if (!path1) return path2;

          if (path1.indexOf('\\') == path1.length-1) {
            return path1 + path2;
          } else {
            return path1 + '\\' + path2;
          }
        };
      }

    })();


<a name="boolmask"></a>
## Bool Mask Filter

A common task is masking a boolean value with replacement text. Often times I want to show "yes" or "no" instead of "true" and "false."

    (function() {
      'use strict';

      // Anything in maskTextForTrue will be displayed if bool is True

      angular.module('myapp').filter('boolMask', function() {
        return function(input, maskTextForTrue, maskTextForFalse) {
          if (input == null) {
            return "";
          }

          if (input) return maskTextForTrue;

          return maskTextForFalse;
        };
      });

    })();


<a name="camelcase"></a>
## Convert Camel Case to Human Readable String

A simple regular expression wrapper to convert a camel case string into a string with spaces created by [jdpedrie](https://github.com/jdpedrie/angularjs-camelCase-to-human-filter).
For example, changes `myCamelCaseString` into `My Camel Case String`

    'use strict';

    angular.module('camelCaseToHuman', []).filter('camelCaseToHuman', function() {
      return function(input) {
        return input.charAt(0).toUpperCase() + input.substr(1).replace(/[A-Z]/g, ' $&');
      }
    });

View code on github at [jdpedrie/angularjs-camelCase-to-human-filter](https://github.com/jdpedrie/angularjs-camelCase-to-human-filter/blob/20311cf9d8107bd1a16579d702e95c4cf3493aac/camelCaseToHuman.js)
