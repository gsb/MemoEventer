# MemoEventer
## About
MemoEventer uses KeyPath strings with wildcards as event identifiers - think of Object dot-notation or namespacing slash-notation.  Event identifier strings are parsed into a small Memo class instance for faster event string matching.

MemoEventer includes all the typical eventing methods - trigger, on, off and once, as well as a *triggerOrQueue* eventing method that will queue an event if it has no listeners currently registered.  Queued events are later triggered, if and when, an appropriate listener gets registered.

Both the Memo class and the Eventer class have extensive unit tests using Jasmine 2.2.0.  And yes, the MemoEventer package is quite small, weighing in at only ~3k for the minified source.


##Status
Although the MemoEventer classes have extensive unit testing (Jasmine 2.2.0) they require additional cross-browser testing.


##Memo Class
The Memo class is an event string parsing template.  It preserves the original event identifier string and then parses it into a valid event name as both a string and a segments array.

> **example**
> ```
var memo = new Memo( '/a/b/c' );
memo.identifier = '/a/b/c';
memo.eventName = 'a/b/c';
memo.parts = ['a','b','c'];
memo.length = 3;
memo.hasWildcard = this.parts.indexOf(wildcard) !== -1;
```

For event name matching, first the number of segments are compared as integers.  If both have the same segment count, the cleaned up event names are compared as strings.  If string compare fails and one or both of the event names have a wildcard, the segments are iteratively compared.

> **example**
> ```
var memo = new Memo('/a/*/c/');
memo.matches('a/b/c'); // true
```

##Eventer Class
The Eventer class includes all the typical eventing methods - trigger, on, off and once, as well as *triggerOrQueue*.  Instances are suitable as an application or module specific event-busses and through extension, inclusion in other classes datum element observables. 

> **example**
> ```
var appEventBus = new Eventer();
...
function Observable ( value ) { ... };
...
Eventer.prototype.extend(Observable);
Observable.watch = Observable.on;
Observable.unwatch = Observable.off;
```

<br/>

# Usage Snippets
(Also see the source code comments and unit test specs .js files.)

**Initialization**  The following code snippets creates a local instance of the MemoEventer:
> ```
var eventBus = new MemoEventer;
```

<br/>
**Add/Trigger Listener**:  This registers a handler for the wildcard event ' a/*anything*/c ' and then triggers a matching event that will be processed by the handler function, 'fn1'.
> ```
eventBus.on('a/*/c', fn1);
eventBus.trigger('a/b/c');
```

<br/>
**Remove Listener**:  The following removes the handler, 'fn1', for the wildcard event ' a/*anything*/c '.
> ```
eventBus.off('a/*/c', fn1);
```



<br/>

## License

(The MIT License)

Copyright (c) 2015 by Greg Baker 

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.


