### Introduction

Hi there again! Time to continue.

In [Part I][1] I started talking about debugging in Javascript. I metioned old school techniques and then focused on client side debugging using great tools such as Google Chrome Dev Tools and Firebug. You probably knew about them, they are very popular. 

Today I want to focus on the server side. In this post I'll talk about step by step execution. I'll finish with logging and the console on Part III.

### Introducing node.js

Back end development in Javascript is quite new. The platform that made it possible is  [node.js][2]. According to their site:

> Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

Using node.js, the whole stack can be developed using Javascript, including both front and back end. This allows to use only one language, taking more advantage of the devs skills and reusing code for both the client and the server.

node.js allows to create lightweight applications using a fully async model with callbacks (or 
better yet, promises). This will be all very familiar to experienced JS developers. 

But there's so much we can talk about, let's leave that for another post and focus on debugging.

Since the code is back end, it's not executed inside a Web Browser, so different tools are required.


### Step by step debugging with breakpoints

Development tools are not so mature on the backend, but there are options that have been showing up and we can expect more to come. The two main alternatives I've tried are:

#### IDE 
If you'd like to use a fully featured IDE, [WebStorm][3] is a great option (but unfortunately, not free). It's quite clever regarding autocomplete and understading the code (in a dynamic language like JS that's not an easy job) and specifically, it allows to debug node.js applications, similar to what you can do in Java or PHP.

In order to debug using WebStorm, you need to:

 - Go to *File* -> *Settings* -> *JavaScript* -> **Node.js and NPM** (or click ![][4] in the toolbar) to configure Node.js integration, in special the path to node executable
    > WebStorm allows you to install modules from their GUI, both for the project and globally


 - Add a new *Run configuration* for a node.js application

 ![][5]

 
 - Ensure the working directory is correct and specify the initial js file to execute:

 <img src="https://raw.github.com/martinmoscovich/posts/master/img/webstorm-run_configuration.png" width="500">

 > As you can see above, if you are developing a web application, it is possible to instruct WebStorm to open the WebBrowser automatically.


 - Set breakpoints by clicking the space on the right of the  of code.


 - Hit the ![enter image description here][6] (*Debug*) button on the toolbar.



This is how it will look when debugging (notice the variable inspection):

![WebStorm debugging][7]


As you can see, you get step by step execution and inspection of variables and objects. You also get the stack trace and a window with the console output (as you would see on the terminal).

<img src="https://raw.github.com/martinmoscovich/posts/master/img/webstorm-console.png" width="600">


Of course, there are other products you can try.

#### Text Editor + External debugger

If you prefer a simpler (and **free**) option, you can use a text editor combined with an external tool for debugging.
I've personally used and recommend [Sublime Text][8] for a while (not only for JS, also as a text editor). Enhanced with the proper plugins, it can become quite powerful, and understand Javascript a lot better than a simple text editor.

But the real star here is [node-inspector][9]. This great tool allows you to connect to a running node.js instance and debug the code like you'd do it with Chrome DevTools on the front end. 
The github page contains all the details but I'll give you a quick tour.

 - Install node-inspector globally: ```npm install -g node-inspector``` (you may need administrator rights to do it).

 - Go to your project's directory.

 - Run: ```node-inspector```.

 - Run the node application as usual but add the *debug* flag. Eg. ```node --debug app.js```.

    >If your application ends quickly (eg: not a web server) or you'd like to debug the very first lines, use ```--debug-brk``` instead of ```--debug```. This will pause the execution at the first line of code and wait for the debugger to connect. 
    Otherwise, by the time you open node-inspector, the program will have already finished.

 - Navigate to http://127.0.0.1:8080/debug?port=5858 (if using default values).

 - A debugging application very similar to Chrome DevTools will appear: 
 
 <img src="https://raw.github.com/martinmoscovich/posts/master/img/code-inspector.png" alt="node-inspector screen" width=600>


 - Happy debugging!



Among the cool features of **node-inspector**:

- Conditional breakpoints, step by step execution.
- Variable values and complex objects inspection and modification.
- Includes stacktrace information.
- Live modification of the code, that will change for the current execution (and will flush changes to the filesystem if the ```--save-live-edit``` flag is used when running node-inspector)
- Easy navigation of the scripts in the project, in order to set the breakpoints in any file
- Breakpoints are saved between executions on your browser's storage
- [More...][10]


### Conclusion

By using any of these tools, developing and debugging will be much easier and that helps to reduce the number of runtime errors.
Using the console and logging is still very helpful, but the two techniques complement. Logging is your best friend when analyzing a bug on production code and it also helps while developing. 
In some situations, like async and/or time sensitive code, pausing the execution and running step by step will produce undesired side-effects, so using the log is your only option.

I will talk about it in Part III. See you there!


  [1]: http://nan-labs.com/javascript/2014/01/03/evolving-of-javascript-debugging/
  [2]: http://nodejs.org/
  [3]: http://www.jetbrains.com/webstorm/index.html
  [4]: https://raw.github.com/martinmoscovich/posts/master/img/webstorm-nodejs_icon.png
  [5]: https://raw.github.com/martinmoscovich/posts/master/img/webstorm-new_run_config.png
  [6]: https://raw.github.com/martinmoscovich/posts/master/img/webstorm-debug_button.png
  [7]: http://www.jetbrains.com/webstorm/whatsnew/screenshots/30/nodeJSDebugging_thumb.png
  [8]: http://www.sublimetext.com/
  [9]: https://github.com/node-inspector/node-inspector
  [10]: https://github.com/node-inspector/node-inspector
