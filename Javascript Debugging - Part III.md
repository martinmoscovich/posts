### Introduction

This is the third and last part of the *famous* "Javascript debugging" series.

In [Part II][1] I talked about step by step debugging of [Node.js][2] applications. 

Now I will talk about Logging and the console. I have a backend background (Java, .NET, some PHP), so, in my Node.js apps, I try to use *(what I consider to be)* good practices I learned when working on those technologies.

<br />

### The Console in Node.js

[Node.js][3] implements the [console API][4], so it can be used just as you do it in the front end.
Taken from the documentation:
> For printing to stdout and stderr. Similar to the console object functions provided by most web browsers, here the output is sent to stdout or stderr.

Summary of the methods:

Methods | Output | Description
----|----|----
|```log()```, ```info()``` | stdout | Prints the message and a *newline*
|```error()```, ```warn()``` | stderr | Prints the message and a *newline*
|```dir()``` | stdout | Prints the content of an object as a string using ```util.inspect```
|```time()```, ```timeEnd()``` | stdout | ```time()``` marks a time and ```timeEnd()``` stops the timer and prints the message with the associated duration.
|```trace()``` | stderr | Print a stack trace to stderr of the current position.
|```assert()``` | - | If the expression evaluates as false an AssertionError with the message is thrown.

**Note** ```debug()``` is not supported (```log()``` is used instead).

I'm going to focus in the methods that only print messages in this post (```log()```, ```info()```, ```error()```, ```warn()```).

<br />
#### **Message Formatting**

Console's methods support the message formatting I explained on [Part I][5]. So, for example, you can do this: 
```javascript
console.log('count: %d', count);
```

Take a look at [Part I][6] for more details.

<br />
#### **Output**

In node.js, most console methods prints the output to *stdout* and some to *stderr*.
You can print them to the terminal or route them to a file to save the logs.
eg: ```node app.js > execution.log ```

<br />
#### **Console shortcomings**

The console in node.js works well, but there are some problems due to lack of functionality.

If you read Part I, you saw I refer to logging levels and why they are useful if used properly.

For some reason, levels **don't really matter** in node. Yes, you have different methods, but if you take a look at the methods summary above, you'll notice that the methods that just log messages (```log()```, ```info()```, ```error()```, ```warn()```) all work the same with the only exception that the former two print to *stdout* and the latter print to *stderr*.
There's no level-based filtering and no easy way to distinguish if you use use method or the other. In my experience, these to features are very helpful when developing and debugging server side apps. 

Also, these methods just print the message without any other useful information. If you have ever worked in other backend technologies, such as Java, you know there are other very useful pieces of info you can retrieve besides the message per se. Think about *filename*, *line number*, the *logging level*, etc.

<br /><br />

### econsole to the rescue!

A while back, I was working on a node.js app and I had an import process that had some complexity and many async calls. In order to debug it, I added many log messages using the console. But the problems I described above made those logs hard to analyze and reduced their utility.

So I started looking for a library that would help me. I found some but they lacked some of the features I was looking for or they had more features than I needed but that made them too big, with many dependencies, etc.

So, I ended up building my own, using pieces of these libraries. And [econsole][7] was born!

The features of [econsole][7]:

 - Small (only one file) with no external dependencies.
 - Compatible with the current API (no need object, the original console is enhanced).
 - Filtering by level (configurable).
 - Useful information added to the message (level, file name and line number).
 - Different colors and styles for each level, making it easier to distinguish (you need to enable colors on the terminal).

The second feature is very important. Existing apps will take advantage of this library, just import it in your main file and call the ```enhance()``` method.

Two new methods are added (but you are not forced to use them):

 - ```debug()```: alias for ```log()```
 - ```verbose()```: a finer detail level (aka *trace*) than debug.

Also, something I found very useful in order technologies (eg: JUnit in Java) is the ability to get extra information when logging an error, so in this library, the ```error()``` method is overloaded. You can continue to send only a message, but you can also send an JavaScript error (or a message AND an error) and you'll get the stacktrace. 


A picture is worth a thousand words, so compare the difference.

From:

<img src="/assets/themes/nanlabs/img/posts/2014-02-03-javascript-debugging-part-iii/econsole-off.png" width="500">

To: 

<img src="/assets/themes/nanlabs/img/posts/2014-02-03-javascript-debugging-part-iii/econsole-on.png" width="500">

Of course, think of this in a process with pages and pages of logs. The different colors really help. Knowing the exact line where the log was called is also **very** helpful. 
I deliberately excluded any other info (such as time or method name) to avoid an info overflow!

If I changed the level to, for example, **WARN**, then the first image would look the same but the second would only contain the first two logs. In production, it's a good idea to disable all levels except **ERROR**. You'll get smaller logs with only the critical messages. Then, while debugging or developing, you enable all the levels and get way more info.

If you are interested, I suggest you read the [documentation and the example][9]. Please let me know if you have any questions or comments. And also it's open source so you can just grab the code if you prefer!

<br />

### Logging to file

Using the regular console or [econsole][10], you can route the output to a file as I mentioned above.

If you want a more complex file logger, with rolling log files, etc, there are some modules in [NPM][11] that can help you, such as [winston][14], [log][12], [log-it][13], etc (I didn't test them)

I chose to leave [econsole][10] small and adjusted to my needs in that moment.  I'll make improvements in the future.

<br /><br />

## Conclusion

To sum up the series, we remembered the old days of js debugging, then we went through today's tools and techniques for js debugging (specifically logging and step by step debugging), first for front-end apps and finally for the back-end. 


Keep in mind that using debugger won't magically improve your apps' quality. 
It really helps, but of course you'll still need a good architecture and design, probably using some of the great JS libraries and frameworks available (such as [jQuery][15], [Underscore.js][16], [Backbone.js][17], [AngularJS][18], [Express][19]), as much testing as possible (using libraries such as [Jasmine][20], [Mocha][21], [Sinon.js][22]) and a lot of good judgement!

Setting up an automated building process with a tool like [Grunt][23] will help as well.


Thanks to all these techniques, tools, libraries and frameworks (and the devs behind them), building apps in JavaScript is getting easier every day. Who knows what will come next...

That's all for today. I hope you enjoy reading (and maybe learned something), feel free to comment.

See you soon!


  [1]: /javascript/2014/01/27/javascript-debugging-part-ii/
  [2]: http://nodejs.org/
  [3]: http://nodejs.org/api/stdio.html
  [4]: http://nodejs.org/api/stdio.html
  [5]: /javascript/2014/01/03/evolving-of-javascript-debugging/
  [6]: /javascript/2014/01/03/evolving-of-javascript-debugging/
  [7]: https://npmjs.org/package/econsole
  [8]: https://github.com/nanlabs/econsole
  [9]: https://npmjs.org/package/econsole
  [10]: https://npmjs.org/package/econsole
  [11]: https://npmjs.org/
  [12]: https://npmjs.org/package/log
  [13]: https://npmjs.org/package/log-it
  [14]: https://npmjs.org/package/winston
  [15]: http://jquery.com/
  [16]: http://underscorejs.org/
  [17]: http://backbonejs.org/
  [18]: http://angularjs.org/
  [19]: http://expressjs.com/
  [20]: http://pivotal.github.io/jasmine/
  [21]: http://visionmedia.github.io/mocha/
  [22]: http://sinonjs.org/
  [23]: http://gruntjs.com/
