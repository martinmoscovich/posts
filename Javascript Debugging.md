
# Javascript debugging: RIP alert()

For those who started working with Javascript years ago, life wasn't as good as it is now. The Javascript ecosystem has evolved so much in the last decade. Now we even build our backend in JS thanks to nodejs.

But if I start enumerating all the changes, this post would never end. I wanted to focus on my experience regarding debugging and logging.

### The dark ages

Before I discovered the amazing [Firebug][1], I used two methods to help me debug code:

 - alert() function: that ugly window that pops up in your browser and totally annoys you. 
 - Using a DOM Element: create an special development ```<div id="log">``` and create a JS function that would change its content with the message

These solutions had many disadvantages, especially when debugging event handlers and other async code.
 
### The modern times

Ever since I discovered [Firebug][2], the way I develop with Javascript changed forever. Now I use Chrome Dev Tools, but both include the many useful features I use (including real time DOM and styles manipulation, network traffic details, etc).

The two features I wanted to talk about are line by line debugging with breakpoints and console logging.
But the Javascript world can now be separated in two main streams, frontend (running on the browsers) and backend (using the great nodejs). The two run on different platforms so debugging is different on each.


## Front end

Front end development in Javascript is the oldest of the two, and it has more alternatives (Chrome, Firefox, Safari, IE, etc) and more mature platforms. 

I personally started with Firefox (Firebug) but now I use Chrome Dev Tools.

To use Dev Tools, just open your page on Chrome and press F12.

#### Line by line debugging with breakpoints

It's pretty straightforward to do this in Dev Tools, just go to "Sources" tab, choose the js file and add the breakpoint in the line you want. When the code reaches that point, the execution will be paused and the line will be highlighted. Then the variable values and DOM can be examined and modified and the code execution can be resumed or you can continue line by line as in any other back end technology (Java, .NET, etc).

The current version of the tools (Chrome or Firebug) includes many features, such as conditional breakpoints, breakpoints on exceptions.
For more information check out the [Chrome Dev Tools Debugging Documentation][3].

Screenshot taken from the Chrome Dev Tools documentation:

![enter image description here][4]



> NOTE: This kind of debugging is very useful but can produce side effects when running async or time sensitive code. For example, debugging a "mouse over" event handler using this technique could make your application crazy. 
In that case, using the console might be a better idea. 

### Console

Chrome Dev Tools provides a console where you can evaluate expressions real time. 

This console can be open by clicking the "console" tab.
A very useful feature is using the famous ```console.log();``` in your code. Every time that method is called, the message passed as parameter will be logged in the console.

```javascript
console.log("Node count: %d", 2);
```
![enter image description here][5]
    
    
    
Console logging evolved since it was first introduced. Unfortunately the exact implementation is different in each browser but most provide similar functionality.

Some of the relevant features are:

 - Logging levels
 - String formatting. Example: ```console.log("%s has %d points", "Martin", "100");```
 - Grouping messages visually
 - Logging JSON, JS Objects or DOM Elements in a readable way. Example ```console.log(document.body.firstElementChild)```

A detailed description can be found at [Chrome Dev Tools Console Documentation][6].

#### Logging Levels

I wanted to explain the logging levels a little more, since it's useful but is sometimes ignored. 
The console object includes other methods that allow to use different levels:

Method | Level | Chrome | Firebug
----|------|----|----
|```console.error()``` | ERROR  | Shown in red and include the stack trace| Same
|```console.warn()``` | WARN  | Shows a yellow icon before the message| Same as Chrome and also the background is yellow
|```console.info()``` | INFO  | Behaves as DEBUG level| Different from DEBUG level and includes a blue icon
|```console.debug()``` | DEBUG  | Just prints the message | Same
|```console.log()``` | DEBUG  | Behaves as DEBUG level | Same

As you can see, different levels are displayed differently, so they can be distinguished (except INFO in Chrome). 

Screenshot from Firebug:

![Firebug log level styles][7]

One very helpful feature is Level based filtering. You can ask the console to only display a particular level (in both Chrome and Firebug). 

![Level based filtering][8]

So if you use the levels wisely, it makes debugging easier, as you can see more or less detailed logging depending on your needs and without changing the code. No need to comment out a log line for a particular case, just change the log level.


## Back end

Back end development in Javascript is quiet new. The only real platform used is [node.js][9]. According to their site:

> Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

Since the code is back end, it's not executed inside a Web Browser, so none of the tools above can be used. There are alternatives, tough.

But I think it's enough for today, I'll write about the tools I found useful for line by line debugging and console logging (since node.js implementation is not fully featured) on part II!

## To be continued!


  [1]: http://getfirebug.com/
  [2]: http://getfirebug.com/
  [3]: https://developers.google.com/chrome-developer-tools/docs/javascript-debugging
  [4]: https://developers.google.com/chrome-developer-tools/docs/javascript-debugging/image_8.png
  [5]: https://developers.google.com/chrome-developer-tools/docs/console-files/log-basic.png
  [6]: https://developers.google.com/chrome-developer-tools/docs/console#writing_to_the_console
  [7]: http://getfirebug.com/img/logging/colorCoding.png
  [8]: https://developers.google.com/chrome-developer-tools/docs/console-files/filter-errors.png
  [9]: http://nodejs.org/
