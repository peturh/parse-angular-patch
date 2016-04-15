
Parse Angular Patch
=========

Fork comments
-------------

Since this is the best angular-patch I've stumbled across, I forked it and fixed it since the guys no longer maintain it.

Hope it works.


Originally brought to you by [Try.com](https://www.try.com)

  - Seamless Parse integration with AngularJS, using promises ($q)
  - Never worry about $scope digests again
  - Additional (and optional) module to enhance Parse Objects.



How to use
----

I. [Grab the latest version of the patch here](https://raw2.github.com/brandid/parse-angular-patch/master/dist/parse-angular.js) or install it using [Bower](http://bower.io/)

```
bower install parse-angular-patch
```

II. Include the module in your project

```javascript
angular.module('myApp', ['ngAnimate', 'parse-angular'])
```

III. That's it. How hard was that?! You can now do ANYWHERE in your angular app things such as :

```javascript
// Queries
var query = new Parse.Query("Monsters");
query.equalTo("name", "Frankeistein");
query.first()
.then(function(result){
        $scope.monsters = result;
});
// Cloud Code is patched too!
Parse.Cloud.run("myCloudCodeFunction", function(results) {
    $scope.data = results;
});
```

  And your scope will always be updated. Any asynchronous Parse method is patched and wrapped inside Angular kingdom (Parse.FacebookUtils methods, Parse.User methods, etc etc)
 

Extra Features
----

This patch also extends the Parse SDK to add the following features :
* Automatic getters & setters generation from a simple attrs array
* Adds a static getClass() method on Objects to fetch them easily anywhere in your apps

### How to use

Simply add the 'parse-angular.enhance' module after the 'parse-angular' one in your app dependencies

```javascript
angular.module('myApp', ['ngAnimate', 'parse-angular', 'parse-angular.enhance'])
```

### Auto generate getters & setters

Nothing simpler, just attach an array of attributes to your Object definition, and the enhancer will generate according getters/setters. Please note that the first letter of your attribute will be transformed to uppercase.

```javascript
Parse.Object.extend({
  className: "Monster",
  attrs: ['kind', 'name', 'place_of_birth']
});


var myMonster = new Parse.Object("Monster");
// You can do :
myMonster.getKind();
myMonster.getName();
myMonster.setPlace_of_birth('London');
```

Please note that if you already set a getter or setter on the Object, it won't be overrided. It is just a double-check protection, otherwise just don't add the attribute to your attrs array.


### getClass static method

With this extra module, you get a static getClass method on Parse.Object that allows you to retrieve a previously defined class. Let's see some example that will make it clearer

```javascript
// Define an object with static methods
Parse.Object.extend({
    className: "Monster",
    getName: function() {
        return this.get('name');
    }
}, 
// Static methods
{
    loadAll: function() {
        var query = new Parse.Query("Monster");
        query.limit(1000);
        return query.find();
    }
}
});
```

The problem here is that if you want to call loadAll() on the Class definition, you need a reference to it. To make it easier, the getClass static method allow you to grab it anywhere in your code.

```javascript
Parse.Object.getClass("Monster").loadAll()
.then(function(monsters){
 // my array of monsters is here
});

var newMonster = new Parse.Object("Monster");
var otherMonster = new (Parse.Object.getClass("Monster"));

// ^ both are equivalent, first syntax is preferred cuz shorter
```


Wanna build a large Parse+Angular app?
----

Wait no more and check our [parse-angular-demo](https://github.com/brandid/parse-angular-demo) project


License
----

MIT
  
    
