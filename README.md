# Watch.js 1.4.2 [Download](https://raw.github.com/melanke/Watch.JS/master/src/watch.js)

## ⚠ This project is no longer maintained, for more active development, check [on-change](https://github.com/sindresorhus/on-change)

## 关于

Watch.JS is a small library with a lot of possibilities. You may know that the "Observer" design pattern involves executing some function when an observed object changes. Other libraries exist that do this, but with Watch.JS you will not have to change the way you develop. Take a look at the examples to see how simple it is to add Watch.JS to your code.

## Compatible with all serious browsers :P
Works with: IE 9+, FF 4+, SF 5+, WebKit, CH 7+, OP 12+, BESEN, Node.JS , Rhino 1.7+

# Installing

#### HTML Script TAG
```html
<script src="watch.js" type="text/javascript"></script>
<!-- watch will be a global variable -->
```

#### Via NPM
```
npm install melanke-watchjs
```

# Importing

#### Import as ECMA2015 module

```javascript
import WatchJS from 'melanke-watchjs';

var watch = WatchJS.watch;
var unwatch = WatchJS.unwatch;
var callWatchers = WatchJS.callWatchers;
```

#### Require

```javascript
var WatchJS = require("melanke-watchjs")
var watch = WatchJS.watch;
var unwatch = WatchJS.unwatch;
var callWatchers = WatchJS.callWatchers;
```

#### RequireJS
```javascript
require("watch", function(WatchJS){
    var watch = WatchJS.watch;
    var unwatch = WatchJS.unwatch;
    var callWatchers = WatchJS.callWatchers;
});
```

# 例子

## 监听对象的一个属性的变化

```javascript
//defining our object however we like
var ex1 = {
	attr1: "initial value of attr1",
	attr2: "initial value of attr2"
};

//defining a 'watcher' for an attribute
watch(ex1, "attr1", function(){
	alert("attr1 changed!");
});

//when changing the attribute its watcher will be invoked
ex1.attr1 = "other value";
```

[试一试](http://jsfiddle.net/NbJuh/62/)

## 监听对象的多个属性的变化(属性使用数组传入)

```javascript
//defining our object however we like
var ex2 = {
    attr1: 0,
    attr2: 0,
    attr3: 0
};

//defining a 'watcher' for the attributes
watch(ex2, ["attr2", "attr3"], function(){
    alert("attr2 or attr3 changed!");
});

//when changing one of the attributes its watcher will be invoked
ex2.attr2 = 50;​
```

[试一试](http://jsfiddle.net/2zT4C/23/)

## 监听该对象的所有属性的变化

```javascript
//defining our object however we like
var ex3 = {
    attr1: 0,
    attr2: "initial value of attr2",
    attr3: ["a", 3, null]
};

//defining a 'watcher' for the object

watch(ex3, function(){
    alert("some attribute of ex3 changes!");
});

//when changing one of the attributes of the object the watcher will be invoked
ex3.attr3.push("new value");​
```

[试一试](http://jsfiddle.net/C83pW/27/)

## 移除监听器

```javascript
var obj = {
    phrase: "hey",
    name: "buddy",
    alert: function(){
        alert(obj.phrase + " " + obj.name);
    },
    alert2: function(){
        alert(obj.name + ", " + obj.phrase);
    }
}
    
watch(obj, "name", obj.alert);
watch(obj, "name", obj.alert2);

obj.name = "johnny";

unwatch(obj, "name", obj.alert);

obj.name = "phil";​
```

[试一试](http://jsfiddle.net/SZ2Ut/9/)

## 获取属性变化的更多信息

```javascript
//defining our object no matter which way we want
var ex1 = {
    attr1: "initial value of attr1",
    attr2: "initial value of attr2"
};

//defining a 'watcher' for an attribute
watch(ex1, "attr1", function(prop, action, newvalue, oldvalue){
    alert(prop+" - action: "+action+" - new: "+newvalue+", old: "+oldvalue+"... and the context: "+JSON.stringify(this));
});

//when changing the attribute its watcher will be invoked
ex1.attr1 = "other value";​
```

[试一试](http://jsfiddle.net/XnbXS/21/)

## 不用担心无限循环

If you don't want to call a second watcher in the current scope just set WatchJS.noMore to true and it will be reset to false when this watcher finishes.
如果你不想在当前事件循环中调用第二个监听器，只需要设置WatchJS.noMore为true就可以了，当该监听七完成的时候其会自动重设为false。

```javascript
//defining our object however we like
var ex1 = {
    attr1: "inicial value of attr1",
    attr2: "initial value of attr2"
};

//defining a 'watcher' for an attribute
watch(ex1, "attr1", function(){
    WatchJS.noMore = true; //prevent invoking watcher in this scope
    ex1.attr2 = ex1.attr1 + " + 1";
});

//defining other 'watcher' for another attribute
watch(ex1, "attr2", function(){
    alert("attr2 changed");
});


ex1.attr1 = "other value to 1"; //attr1 will be changed but will not invoke the attr2`s watcher
```

[试一试](http://jsfiddle.net/z2sJr/16/)

## 设置level决定监听深度

```javascript
//defining our object no matter which way we want
var ex = {
    //level 0
    l1a: "bla bla",
    l1b: {
        //level 1 or less
        l2a: "lo lo",
        l2b: {
            //level 2 or less
            deeper: "so deep"
        }           
    }
};

watch(ex, function(){
    alert("ex changed at lvl 2 or less");
}, 1);

watch(ex, function(){
    alert("ex changed at lvl 3 or less");
}, 2);


ex.l1b.l2b.deeper = "other value";


ex.l1b.l2b = "other value";
```

[试一试](http://jsfiddle.net/7AwbW/5/)

## 添加一个新属性，默认会被忽略，不会调用监听器

在为某个对象声明了一个监听器之后，当你为该对象添加新属性并更改它时，将不会调用监听器。

```javascript
//defining our object however we like
var ex6 = {
    attr1: 0,
    attr2: 1
};

//defining a 'watcher' for the object
watch(ex6, function(){
    alert("some attribute of ex6 changed!")
});

ex6.attr3 = null; //no watcher will be invoked
ex6.attr3 = "value"; //no watcher will be invoked​​​
```

[试一试](http://jsfiddle.net/NFmUc/7/)

## 若是添加新属性，则必须等待50毫秒才能调用监听器

```javascript
//defining our object no matter which way we want
var ex = {
    l1a: "bla bla",
    l1b: {
        l2a: "lo lo",
        l2b: "hey hey"        
    }
};

watch(ex, function (prop, action, difference, oldvalue){
    
    alert("prop: "+prop+"\n action: "+action+"\n difference: "+JSON.stringify(difference)+"\n old: "+JSON.stringify(oldvalue)+"\n ... and the context: "+JSON.stringify(this));    
    
}, 0, true);


ex.l1b.l2c = "new attr"; //it is not instantaneous, you may wait 50 miliseconds

setTimeout(function(){
    ex.l1b.l2c = "other value";
}, 100);
```
[试一试](http://jsfiddle.net/wXWPQ/4/)

## 你可以随时使用callWatchers方法调用监听器

```javascript
//defining our object however we like
var ex7 = {
    attr1: 0,
    attr2: 1
};

//defining a 'watcher' for the object
watch(ex7, function(){
    alert("some attribute of ex6 changed!")
});

callWatchers(ex7, "attr1"); //invoke the watcher​​
```

[试一试](http://jsfiddle.net/98MmB/10/)

## 可以与jQuery配合使用

```javascript
$(function(){

    var obj = {cont: 0};
    
    watch(obj, "cont", function(){
        alert("obj.cont = "+obj.cont);
    });

    $("#button").click(function(){
        obj.cont++;
    });
});
```
[试一试](http://jsfiddle.net/fj2Yb/24/)

## 使用不同的方法构建类/对象并使用Watch.Js

```javascript
//open the browser log to view the messages

var Apple = function(type) {
    var _thisApple = this;
    this.type = type;
    this.color = "red";

    this.getInfo = function() {
        return this.color + ' ' + this.type + ' apple';
    };

    watch(this, function(){
        console.log("although we are using Watch.js the apple structure remains the same");
        for(var i in _thisApple){
            console.log(i+": "+_thisApple[i]);
        }
    });
};


var apple = new Apple("macintosh");
apple.type = "other";


var Banana = function(type) {
    var _thisBanana = this;
    this.type = type;
    this.color = "yellow";

    watch(this, function(){
        console.log("although we are using Watch.js the banana structure remains the same");
        for(var i in _thisBanana){
            console.log(i+": "+_thisBanana[i]);
        }
    });
};

Banana.prototype.getInfo = function() {
    return this.color + ' ' + this.type + ' banana';
};

var banana = new Banana("Cavendish");
banana.type = "other";

var orange = {
    type: "pocan",
    color: "orange",
    getInfo: function () {
        return this.color + ' ' + this.type + ' apple';
    }
};

watch(orange, function(){
    console.log("although we are using Watch.js the orange structure remains the same");

    for(var i in orange){
        console.log(i+": "+orange[i]);
    }
});

orange.type = "other";
```
[试一试](http://jsfiddle.net/t94Vv/58/)
