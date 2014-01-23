### Introduction

Hi there again! Time to continue.

In [Part I][1] I started talking about debugging in Javascript. I metioned old school techniques and then focused on client side debugging using great tools such as Google Chrome Dev Tools and Firebug. You probably knew about them, they are very popular. 

Today I want to focus on the server side. In this post I'll talk about line by line execution. I'll finish with logging and the console on Part III.

### Introducing node.js

Back end development in Javascript is quiet new. The platform that made it possible is  [node.js][2]. According to their site:

> Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

Using node.js, the whole stack can be developed using Javascript, including both front and back end. This allows to use only one language, taking more advantage of the devs skills and reusing code for both the client and the server.

node.js allows to create lightweight applications using a fully async model with callbacks (or 
better yet, promises). This will be all very familiar to experienced JS developers. 

But there's so much we can talk about, let's leave that for another post and focus on debugging.

Since the code is back end, it's not executed inside a Web Browser, so different tools are required.


### Line by line debugging with breakpoints

Development tools are not so mature on the backend, but there are options that have been showing up and we can expect more to come. The two main alternatives I've tried are:

#### IDE 
If you'd like to use a fully featured IDE, [WebStorm][3] is a great option (but unfortunately, not free). It's quite clever regarding autocomplete and understading the code (in a dynamic language like JS that's not an easy job) and specifically, it allows to debug node.js applications, similar to what you can do in Java or PHP.

In order to debug using WebStorm, you need to:
1. Go to Settings and configure Node.js integration, in special the path to node executable
    >WebStorm allows you to install modules from their GUI, both for the project and globally

2. Add a new *Run configuration*, specifying the initial js file to execute.
3. Set breakpoints by clicking the space on the right of the line of code.
3. Hit the *Debug* button on the toolbar

This screenshot taken from their site shows the debugger:

![WebStorm debugging][4]

As you can see, you get line by line execution and inspection of variables and objects. You also get the stack trace and a window with the console output (as you would see on the terminal).

> PONER FOTO DE LA CONSOLA

Of course there are other products you can try.

#### Text Editor + External debugger

If you prefer a simpler (and **free**) option, you can use a text editor combined with an external tool for debugging.
I've personally used and recommend [Sublime Text][5] for a while (not only for JS, also as a text editor). Enhanced with the proper plugins, it can become quite powerful, and understand Javascript a lot better than a simple text editor.

But the real star here is [node-inspector][6]. This great tool allows you to connect to a running node.js instance and debug the code like you'd do it with Chrome DevTools on the front end. 
The github page contains all the details but I'll give you a quick tour.

1. Install node-inspector globally: ```npm install -g node-inspector``` (you may need administrator rights to do it)

2. Go to your project's directory

3. Run: ```node-inspector```

3. Run the node application as usual but add the *debug* flag. Eg. ```node --debug app.js```

    >If your application ends quickly (eg: not a web server) or you'd like to debug the very first lines, use ```--debug-brk``` instead of ```--debug```. This will pause the execution at the first line of code and wait for the debugger to connect. 
    Otherwise, by the time you open node-inspector, the program will have already finished.

5. Navigate to http://127.0.0.1:8080/debug?port=5858 (if using default values)

6. A debugging application very similar to Chrome DevTools will appear. 

7. Happy debugging!
> PONER FOTO DE NODE INSPECTOR


Among the cool features of *node-inspector*

- Conditional breakpoints, line by line execution.
- Variable values and complex objects inspection and modification.
- Includes stacktrace information.
- Live modification of the code, that will change for the current execution (and will flush changes to the filesystem if the ```--save-live-edit``` flag is used when running node-inspector)
- Easy navigation of the scripts in the project, in order to set the breakpoints in any file
- Breakpoints are saved between executions on your browser's storage
- [More...][6]

### Conclusion

By using any of these tools, developing and debugging will be much easier and that helps to reduce the number of runtime errors.
Using the console and logging is still very helpful, but techniques complement. Logging is your best friend when analyzing a bug on production code and it also helps while debugging. 
In some situations, like async and/or time sensitive code, pausing the execution and running line by line will produce undesired side-effects, so using the log is your only option.

I will talk about it in Part III. See you there!

  [1]: http://nan-labs.com/javascript/2014/01/03/evolving-of-javascript-debugging/
  [2]: http://nodejs.org/
  [3]: http://www.jetbrains.com/webstorm/index.html
  [4]: http://www.jetbrains.com/webstorm/whatsnew/screenshots/30/nodeJSDebugging_thumb.png
  [5]: http://www.sublimetext.com/
  [6]: https://github.com/node-inspector/node-inspector