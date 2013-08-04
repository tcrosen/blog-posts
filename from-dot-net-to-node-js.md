#From .NET to Node.js (Part 1)

Date: 08-07-2013

Are you a [.NET developer](http://4.bp.blogspot.com/_kD34xgIwKhc/TFaBKdAaUuI/AAAAAAAABs4/P35B6sRajug/s640/NET_ROCKS_ANDRIY_SLAVSKO.jpg)? 
Have you heard of [Node.js](http://nodejs.org/)?  Windows Azure [supports it](http://www.windowsazure.com/en-us/develop/nodejs/). Scott Hanselman [blogs about it](http://www.hanselman.com/blog/InstallingAndRunningNodejsApplicationsWithinIISOnWindowsAreYouMad.aspx).  Veteran Silverlight & .NET guru Tomasz Janczuk says [it can save you $5 million](http://tomasz.janczuk.org/2013/05/how-to-save-5-million-running-nodejs.html)!

Okay so you've heard of it but you still don't know if it's a JavaScript library or some kind of server farm component (*hint: it's neither!*). Hopefully I can shed some light on what Node.js actually is and how it can *positively* affect your development life whether it be at your current job or in your weekend experiments.

###What is Node.js?

From the official site:

>Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

No idea what that means?  It's okay most people probably don't.

I'm going to give a brief summary of what Node is for the target audience of this article, but for an in-depth explanation (the best one I've read so far) I *highly* recommend Max Ogden's free WIP book [Art of Node](https://github.com/maxogden/art-of-node) (donate if it helps you!).

###In plain language, what is Node.js?

Node.js is an environment for running JavaScript code *outside of web browsers*.  Specifically, it is an *executable* that runs on [pretty much any operating system](http://nodejs.org/download/) and has official installers for both Windows and Mac.

This means you can use the same language on both the **server and client!**

Despite its awkward name, Node.js is written in C++, not JS. It actually *executes* JavaScript. Rather than reinvent the wheel, the JavaScript processor of choice is Google's powerful open-source [V8 Engine](https://code.google.com/p/v8/) which is also found in their [Chrome browser](https://www.google.com/intl/en/chrome/browser/).

The line "non-blocking I/O" from the official description is a favourite piece of controversy amongst Node-haters[1]. What it basically means is that Node embraces JavaScript's asynchronous nature. Events are triggered and handled independently, without waiting for the previous task to finish.

[1] *I'm intentionally not referencing any links because they are mostly [FUD](http://en.wikipedia.org/wiki/Fear,_uncertainty_and_doubt) or at the very least excessively negative and I don't want to deter people who actually embrace change*

### 1 + 2 in Node.js

Node can run JavaScript in a command-line environment or from .js files.

Command line:

    C:\> node 
    > 1 + 2
    > 3

Create file (**add.js**):
    
    console.log(1 + 2);

and run it:

    C:\> node add
    3
    
What actually happened there is a simple text file was parsed and executed through the Node environment which inherently includes the `console` object with the `log` method.  

> `console.log()` is not officially part of the JavaScript namespace but it is found in almost all modern web browsers.  You can open the JavaScript console in your browser (usually with F12) and run the the exact same code.

###Side-by-side with the .NET stack

Here is a table to help you get a quick overview of some common technologies from the .NET web stack and their Node counterparts. This is not an exhaustive list, it is merely a quick overview using the typical components found in beginner guides for both environments.

<table>
    <thead>
      <th></th>
      <th>
       .NET
      </th>
      <th>
       Node
      </th>
    </thead>
    <tr>
        <th>Core Languages</th>
        <td>C#, VB.NET</td>
        <td>JavaScript</td>
    </tr>
    <tr>
        <th>Web Server</th>
        <td>IIS</td>
        <td>Built-in[1], <a href="http://expressjs.com/" target="_blank">Express</a></td>
    </tr>
    <tr>
        <th>Web Framework</th>
        <td>MVC3, WebAPI</td>
        <td><a href="http://expressjs.com/" target="_blank">Express</a></td>
    </tr>
    <tr>
        <th>HTML Templating</th>
        <td>Razor, ASP.NET</td>
        <td><a href="http://jade-lang.com/" target="_blank">Jade</a>, <a href="http://embeddedjs.com/" target="_blank">EJS</a>
    </tr>
    <tr>
        <th>Package Management</th>
        <td>Web Platform Installer, Nuget</td>
        <td><a href="https://npmjs.org/" target="_blank">NPM</a>, <a href="http://bower.io//" target="_blank">Bower</a></td>
    </tr>
    <tr>
        <th>Editors / IDE</th>
        <td>Visual Studio, WebMatrix</td>
        <td><a href="http://www.sublimetext.com/2" target="_blank">Sublime Text</a>[2]</td>
    </tr>
</table>

[1] *While almost no one does this in <strike>practice</strike> production, Node allows you to create a simple web server in a few lines of code.  An example is available on the front page of <a href="nodejs.org">nodejs.org</a>.  If you're interested, [here is a link](http://www.youtube.com/watch?v=jo_B4LTHi3I) to Ryan Dahl's (the creator of Node) now famous introduction to his project in which he demonstrates this feature.*

[2] *Because everything that runs in Node is pure JavaScript, IDEs are not required and I would argue can actually get in the way. The current most popular editor of choice among JS developers is [Sublime Text 2](http://www.sublimetext.com/2) but honestly any text editor or IDE works just fine.  Keen readers will note that .NET applications can also be written in any text editor or IDE, however Visual Studio is easily the de facto tool for the job and is almost too powerful to not use in that regard. If you are interested in actually using Sublime Text 2 for C# development, my friend [Filip](https://twitter.com/filip_woj) is involved in a project called [ScriptCS](http://scriptcs.net/) which allows you to do exactly that.*

###A simple example

Let's create a simple application that **lists files in a directory** in both .NET and Node.js.

####.NET

>As a .NET developer I hope this would be trivial for you but I will summarize the steps anyway.  I am going to use C#, my preferred language within the .NET framework.

Open Visual Studio, create a **new Console Application project** with default settings.  You should end up with an application containing a `Program.cs` file looking pretty close to this: 

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    
    namespace ConsoleApplication1
    {
        class Program
        {
            static void Main(string[] args)
            {
            }
        }
    }

As you are probably aware, the simplest way to run the application is to click the handy "Start" button in Visual Studio.  Doing this will import the various libraries specified in the `using` list, trigger the C# compiler to build the application, output the executable to the `\bin` directory, and finally run it for you.  You may not realize all of that is happening but that's the beauty of IDEs (or reason IDEs are awful, depending on who you ask).

>One thing I'd like to mention about what we have so far is that 5 libraries are included for you without asking for them: `System`, `System.Collections.Generic`, `System.Linq`, `System.Text`, `System.Threading.Tasks`.  Furthermore, you have a Solution File, Project File, `AssemblyInfo.cs` file, some references in the form of binaries & and `app.config` file.

>This is what is known as a *Project Template* in Visual Studio and is the standard way of creating projects of any kind. It can be very convenient, however some people see it as bloat, especially because almost all of these can be removed (along with all 5 references in your `Program.cs`) and the application will still execute correctly.

Let's add the code for traversing a directory.  There are 10,000 ways of doing this but I'm going to use the first one that comes to mind:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

	using System.IO;  
    
    namespace ConsoleApplication1
    {
        class Program
        {
            static void Main(string[] args)
            {
	            DirectoryInfo di = new DirectoryInfo(".");

        	    foreach (FileInfo fi in di.EnumerateFiles())
            	{
	                Console.WriteLine(fi.Name);
    	        }
  
            	Console.ReadKey();
            }
        }
    }

Output:

    ConsoleApplication1.exe
    ConsoleApplication1.exe.config
    ConsoleApplication1.pdb
    ConsoleApplication1.vshost.exe
    ConsoleApplication1.vshost.exe.config
    ConsoleApplication1.vshost.exe.manifest


Easy right?  Okay let's see how to do it in Node.


####Node.js


>*Before you start*:

>1. I'm going to assume you have Node installed if you are actually following along. If you try to run anything using the Node executable and get an error, you probably haven't installed it or don't have it correctly configured in your PATH. There is a nice install guide [here](http://sailsjs.org/#documentation/new-to-nodejs) if you are new to Node.

>2. I'm also going to be using the Windows command prompt to run scripts.  If command prompts scare you, feel free to use any [IDE that supports Node](https://www.google.ca/webhp?sourceid=chrome-instant&ion=1&ie=UTF-8#sclient=psy-ab&q=node+ide&oq=node+ide&gs_l=serp.3..0l4.1867.2528.0.2648.8.7.0.0.0.0.239.943.0j5j1.6.0...0.0.0..1c.1.17.psy-ab.KWkpHdnmx8Y&pbx=1&bav=on.2,or.r_cp.r_qf.&fp=bbd822b2da43e1c1&ion=1&biw=1920&bih=955).

Open a text editor, create new file and call it whatever you want, I'm going to use `list-files.js`.  Now enter the following code which I will explain after:

    var fs = require('fs');

    fs.readdir('.', function(err, files) {
      for (var i = 0; i < files.length; i++) {
        console.log(files[i]);
      }
    });

Run it (output will vary depending on what you have in your directory - mine only contains the file we just created):

    C:\test> node list-files
    list-files.js

####Analysis

Okay so what happened there?  First of all, we created a simple JavaScript file and ran it using the Node executable.  Let's go through the file line-by-line:

    var fs = require('fs');

This uses a global Node [module](http://nodejs.org/api/modules.html#modules_modules) called [require](http://nodejs.org/api/globals.html#globals_require) to import another module ([File System](http://nodejs.org/api/fs.html#fs_file_system) or *fs*) and assign it to a variable called `fs`.

>*Note that the variable name you assign an imported module to does not have to be the same as the module name, this is only for simplicity and is generally considered good practice.*

This line is almost no different from our .NET program's `using System.IO;` except that in Node we assign the imported reference to a variable, so we have to call its methods using the variable `fs.readdir(...)`.

This may seem verbose coming from the Visual Studio environment where imported class properties are automagically available for us within the working file, however it is nice because it avoids namespace collisions (multiple libraries with the same class/method/property name).  This type of aliasing is also possible (though rarely practiced) in .NET:

    using io = System.IO;
    ...
    io.DirectoryInfo di = new io.DirectoryInfo(".");

Or of course you can also use the full library name:

    System.IO.DirectoryInfo di = new System.IO.DirectoryInfo(".");

Now let's take a look at the method [`readdir`](http://nodejs.org/api/fs.html#fs_fs_readdir_path_callback) that actually reads files from the directory:

    fs.readdir('.', function(err, files) {
      ...
    });

The definition is **fs.readdir(path, callback)**.  This is an example of an asynchronous function. I won't go into too much detail about asynchronous programming, but here's a quick explanation:

If you ran the same method like this:

	var fileList;

    fs.readdir('.', function(err, files) {
      fileList = files;
    });

    console.log(fileList);

You would see the following:

    C:\test> node list-files
    undefined

This is because in JavaScript, asynchronous methods start to run and then the program continues to execute the next commands without waiting for a response.

The **callback** function parameter is what we want to do *after* the asynchronous method has completed its work.  To make it a bit cleaner we can re-write the problem as follows:

    var fs = require('fs');

    function readDirectoryComplete(err, files) {
      for (var i = 0; i < files.length; i++) {
        console.log(files[i]);
      }
    }

    fs.readdir('.', readDirectoryComplete);

###Summary 

There you go, you've written your first Node.js program alongside a C# .NET program.  In part 2 of this series we will get into some more web-related activities and discuss the power of NPM and why Node has such a thriving open-source ecosystem of libraries.





 