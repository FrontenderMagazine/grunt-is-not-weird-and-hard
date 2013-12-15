# Grunt for People Who Think Things Like Grunt are Weird and Hard

Front-end developers are often told to do certain things:

*   **Work in as small chunks of CSS and JavaScript** as makes sense to you,
    then concatenate them together for the production website.
   
*   **Compress your CSS and minify your JavaScript** to make their file sizes
    as small as possible for your production website.
   
*   **Optimize your images** to reduce their file size without affecting
    quality.
   
*   **Use Sass** for CSS authoring because of all the useful abstraction it
    allows.
   

That’s not a comprehensive list of course, but those are the kind of things
we need to do. You might call them*tasks*.

I bet you’ve heard of [Grunt][1]. Well, Grunt is a *task runner*. Grunt can do
all of those things for you. Once you’ve got it set up, which isn’t particularly
difficult, those things can happen automatically without you having to think 
about them again.

But let’s face it: Grunt is one of those fancy newfangled things that all the
cool kids seem to be using but at first glance feels strange and intimidating. I
hear you. This article is for you.

## Let’s nip some misconceptions in the bud right away

Perhaps you’ve *heard* of Grunt, but haven’t done anything with it. I’m
sure that applies to many of you. Maybe one of the following hang-ups applies to
you.

### I don’t need the things Grunt does

You probably do, actually. Check out that list up top. Those things aren’t
nice-to-haves. They are pretty vital parts of website development these days. If
you already do all of them, that’s awesome. Perhaps you use a variety of 
different tools to accomplish them. Grunt can help bring them under one roof, so
to speak. If you don’t already do all of them, you probably should and Grunt can
help. Then, once you are doing those, you can keep using Grunt to do more for 
you, which will basically make you better at doing your job.

### Grunt runs on Node.js — I don’t know Node

You don’t have to know Node. Just like you don’t have to know Ruby to use
Sass. OrPHP to use WordPress. Or C++ to use Microsoft Word.

### I have other ways to do the things Grunt could do for me

Are they all organized in one place, configured to run automatically when
needed, and shared among every single person working on that project? Unlikely, 
I’d venture.

### Grunt is a command line tool — I’m just a designer

I’m a designer too. I prefer native apps with graphical interfaces when I can
get them. But I don’t think that’s going to happen with Grunt.

The extent to which you need to use the command line is:

1.  Navigate to your project’s directory.
2.  Type `grunt` and press **Return**.

After set-up, that is, which again isn’t particularly difficult.

## OK. Let’s get Grunt installed

Node is indeed a prerequisite for Grunt. If you don’t have Node installed,
don’t worry, it’s very easy. You literally download an installer and run it. 
Click the big**Install** button [on the Node website][2].

You install Grunt on a per-project basis. Go to your project’s folder. It
needs a file there named*package.json* at the root level. You can just create
one and put it there.

![package.json at root][3]

The contents of that file should be this:

    {
      "name": "example-project",
      "version": "0.1.0",
      "devDependencies": {
        "grunt": "~0.4.1"
      }
    }
    

Feel free to change the name of the project and the version, but the 
`devDependencies` thing needs to be in there just like that.

This is how Node does dependencies. Node has a package manager called [NPM][4]

Once that *package.json* file is in place, go to the terminal and navigate to
your folder. Terminal rubes like me do it like this:

![Terminal rube changing directories][5]

Then run the command:

    npm install
    

After you’ve run that command, a new folder called *node_modules* will show up
in your project.

![Example of node_modules folder][6]

The other files you see there, *README.md* and *LICENSE* are there because I’
m going to put this project[on GitHub][7] and that’s just standard fare there
.

The last installation step is to install the Grunt CLI (command line interface
). That’s what makes the`grunt` command in the terminal work. Without it,
typing`grunt` will net you a “Command Not Found”-style error. It is a
separate installation for efficiency reasons. Otherwise, if you had ten projects
you’d have ten copies of GruntCLI.

This is a one-liner again. Just run this command in the terminal:

    npm install -g grunt-cli
    

You should close and reopen the terminal as well. That’s a generic good
practice to make sure things are working right. Kinda like restarting your 
computer after you install a new application was in the olden days.

## Let’s make Grunt concatenate some files

Perhaps in our project there are three separate JavaScript files:

1.  ***jquery.js*** – The library we are using.
2.  ***carousel.js*** – A jQuery plug-in we are using.
3.  ***global.js*** – Our authored JavaScript file where we configure and
    call the plug-in.
   

In production, we would concatenate all those files together for performance
reasons (one request is better than three). We need to tell Grunt to do this for
us.

But wait. Grunt actually doesn’t do anything all by itself. Remember Grunt is
a task*runner*. The tasks themselves we will need to add. We actually haven’t
set up Grunt to do anything yet, so let’s do that.

The official Grunt plug-in for concatenating files is [grunt-contrib-concat][8]

    npm install grunt-contrib-concat --save-dev
    

A neat thing about doing it this way: your *package.json* file will
automatically be updated to include this new dependency. Open it up and check it
out. You’ll see a new line:

    "grunt-contrib-uglify": "~0.2.2"
    

Now we’re ready to use it. To use it we need to start configuring Grunt and
telling it what to do.

You tell Grunt what to do via a configuration file named *Gruntfile.js*

Just like our *package.json* file, our *Gruntfile.js* has a very special format
that must be just right. I wouldn’t worry about what every word of this means. 
Just check out the format:

    module.exports = function(grunt) {
    
        // 1. All configuration goes here grunt.initConfig({
            pkg: grunt.file.readJSON('package.json'),
    
            concat: {
                // 2. Configuration for concatinating files goes here.
            }
    
        });
    
        // 3. Where we tell Grunt we plan to use this plug-in.
        grunt.loadNpmTasks('grunt-contrib-concat');
    
        // 4. Where we tell Grunt what to do when we type "grunt" into the terminal.
        grunt.registerTask('default', ['concat']);
    
    };
    

Now we need to create that configuration. The documentation can be overwhelming
. Let’s focus just on the very simple[usage example][9].

Remember, we have three JavaScript files we’re trying to concatenate. We’ll
list file paths to them under`src` in an array of file paths (as quoted strings
) and then we’ll list a destination file as`dest`. The destination file doesn
’t have to exist yet. It will be created when this task runs and squishes all 
the files together.

Both our *jquery.js* and *carousel.js* files are libraries. We most likely won
’t be touching them. So, for organization, we’ll keep them in a*/js/libs/*
folder. Our*global.js* file is where we write our own code, so that will be
right in the*/js/* folder. Now let’s tell Grunt to find all those files and
squish them together into a single file named*production.js*, named that way to
indicate it is for use on our real live website.

    concat: {   
        dist: {
            src: [
                'js/libs/*.js', // All JS in the libs folder
                'js/global.js'  // This specific file
            ],
            dest: 'js/build/production.js',
        }
    }
    

**Note:** throughout this article there will be little chunks of configuration
code like above. The intention is to focus in on the important bits, but it can 
be confusing at first to see how a particular chunk fits into the larger file. 
If you ever get confused and need more context, refer to[the complete file][10]

With that `concat` configuration in place, head over to the terminal, run the
command:

    grunt
    

and watch it happen! *production.js* will be created and will be a perfect
concatenation of our three files. This was a big aha! moment for me. Feel the 
power course through your veins. Let’s do more things!

## Let’s make Grunt minify that JavaScript

We have so much prep work done now, adding new tasks for Grunt to run is
relatively easy. We just need to:

1.  Find a Grunt plug-in to do what we want
2.  Learn the configuration style of that plug-in
3.  Write that configuration to work with our project

The official plug-in for minifying code is [grunt-contrib-uglify][11]. Just
like we did last time, we just run anNPM command to install it:

    npm install grunt-contrib-uglify --save-dev
    

Then we alter our *Gruntfile.js* to load the plug-in:

    grunt.loadNpmTasks('grunt-contrib-uglify');
    

Then we configure it:

    uglify: {
        build: {
            src: 'js/build/production.js',
            dest: 'js/build/production.min.js'
        }
    }
    

Let’s update that `default` task to also run minification:

    grunt.registerTask('default', ['concat', 'uglify']);
    

Super-similar to the concatenation set-up, right?

Run `grunt` at the terminal and you’ll get some deliciously minified
JavaScript:

![Minified JavaScript][12]

That *production.min.js* file is what we would load up for use in our *index.
html* file.

## Let’s make Grunt optimize our images

We’ve got this down pat now. Let’s just go through the motions. The
official image minification plug-in for Grunt is[grunt-contrib-imagemin][13].
Install it:

    npm install grunt-contrib-imagemin --save-dev
    

Register it in the *Gruntfile.js*:

    grunt.loadNpmTasks('grunt-contrib-imagemin');
    

Configure it:

    imagemin: {
        dynamic: {
            files: [{
                expand: true,
                cwd: 'images/',
                src: ['**/*.{png,jpg,gif}'],
                dest: 'images/build/'
            }]
        }
    }
    

Make sure it runs:

    grunt.registerTask('default', ['concat', 'uglify', 'imagemin']);
    

Run `grunt` and watch that gorgeous squishification happen:

![Squished images][14]

Gotta love performance increases for nearly zero effort.

## Let’s get a little bit smarter and automate

What we’ve done so far is awesome and incredibly useful. But there are a
couple of things we can get smarter on and make things easier on ourselves, as 
well as Grunt:

1.  Run these tasks automatically when they should
2.  Run only the tasks needed at the time

For instance:

1.  Concatenate and minify JavaScript when JavaScript changes
2.  Optimize images when a new image is added or an existing one changes

We can do this by watching files. We can tell Grunt to keep an eye out for
changes to specific places and, when changes happen in those places, run 
specific tasks. Watching happens through the official[grunt-contrib-watch][15]
plugin.

I’ll let you install it. It is exactly the same process as the last few plug-
ins we installed. We configure it by giving`watch` specific files (or folders,
or both) to watch. By watch, I mean monitor for file changes, file deletions or 
file additions. Then we tell it what tasks we want to run when it detects a 
change.

We want to run our concatenation and minification when anything in the */js/*
folder changes. When it does, we should run the JavaScript-related tasks. And 
when things happen elsewhere, we should*not* run the JavaScript-related tasks,
because that would be irrelevant. So:

    watch: {
        scripts: {
            files: ['js/*.js'],
            tasks: ['concat', 'uglify'],
            options: {
                spawn: false,
            },
        } 
    }
    

Feels pretty comfortable at this point, hey? The only weird bit there is the 
`spawn` thing. And you know what? I don’t even really know what that does.
From what I understand from the documentation it is the smart default. That’s 
real-world development. Just leave it alone if it’s working and if it’s not, 
learn more.

**Note:** Isn’t it frustrating when something that looks so easy in a
tutorial doesn’t seem to work for you? If you can’t get Grunt to run after 
making a change, it’s very likely to be a syntax error in your*Gruntfile.js*.
That might look like this in the terminal:

![Errors running Grunt][16]

Usually Grunt is pretty good about letting you know what happened, so be sure
to read the error message. In this case, a syntax error in the form of a missing
comma foiled me. Adding the comma allowed it to run.

## Let’s make Grunt do our preprocessing

The last thing on our list from the top of the article is using Sass — yet
another task Grunt is well-suited to run for us. But wait? Isn’t Sass 
technically in Ruby? Indeed it is. There is
[a version of Sass that will run in Node][17] and thus not add an additional
dependency to our project, but it’s not quite up-to-snuff with the main Ruby 
project. So, we’ll use the official[grunt-contrib-sass][18] plug-in which just
assumes you have Sass installed on your machine. If you don’t, follow the
[command line instructions][19].

What’s neat about Sass is that it can do concatenation and minification all
by itself. So for our little project we can just have it compile our main*
global.scss* file:

    sass: {
        dist: {
            options: {
                style: 'compressed'
            },
            files: {
                'css/build/global.css': 'css/global.scss'
            }
        } 
    }
    

We wouldn’t want to manually run this task. We already have the watch plug-in
installed, so let’s use it! Within the`watch` configuration, we’ll add
another subtask:

    css: {
        files: ['css/*.scss'],
        tasks: ['sass'],
        options: {
            spawn: false,
        }
    }
    

That’ll do it. Now, every time we change any of our Sass files, the CSS will
automaticaly be updated.

Let’s take this one step further (it’s absolutely worth it) and add
LiveReload. With LiveReload, you won’t have to go back to your browser and 
refresh the page. Page refreshes happen automatically and in the case ofCSS,
new styles are injected without a page refresh (handy for heavily state-based 
websites
).

It’s very easy to set up, since the LiveReload ability is built into the
watch plug-in. We just need to:

1.  Install the [browser plug-in][20] 
2.  Add to the top of the `watch` configuration: 
        . watch: {
            options: {
                livereload: true,
            },
            scripts: {   
            /* etc */

3.  Restart the browser and click the LiveReload icon to activate it.
4.  Update some Sass and watch it change the page automatically.

![Live reloading browser][21]

Yum.

## Leveling up

As you might imagine, there is a *lot* of leveling up you can do with your
build process. It surely could be a[full time job][22] in some organizations.

Some hardcore devops nerds might scoff at the simplistic setup we have going
here. But I’d advise them to slow their roll. Even what we have done so far is 
tremendously valuable. And don’t forget this is all free and open source, which 
is amazing.

You might level up by adding more useful tasks:

*   Running your CSS through [Autoprefixer][23] (A+ Would recommend) instead of
    a preprocessor add-ons.
   
*   Writing and running JavaScript unit tests (example: [Jasmine][24]).
*   Build your image sprites and SVG icons automatically (example: 
    [Grunticon][25]).
*   Start a server, so you can link to assets with proper file paths and use
    services that require a real
   URL like TypeKit and such, as well as remove the need for other tools that
    do this, like
   MAMP.
*   Check for code problems with [HTML-Inspector][26], [CSS Lint][27], or 
    [JS Hint][28].
*   Help you commit or push to a version control repository like GitHub.
*   Add version numbers to your assets (cache busting).
*   Help you deploy to a staging or production environment (example: 
    [DPLOY][29]).

You might level up by simply understanding more about Grunt itself:

## Let’s share

I think some group sharing would be a nice way to wrap this up. If you are
installing Grunt for the first time (or remember doing that), be especially 
mindful of little frustrating things you experience(d) but work(ed) through. 
Those are the things we should share in the comments here. That way we have this
safe place and useful resource for working through those confusing moments 
without the embarrassment. We’re all in this thing together!

Perhaps a decent middleground is this [Grunt DevTools][30] Chrome add-on.

 [1]: http://gruntjs.com/
 [2]: http://nodejs.org/
 [3]: img/package-json-file.gif
 [4]: https://npmjs.org/
 [5]: img/drag-folder.gif
 [6]: img/node_modules.gif
 [7]: https://github.com/chriscoyier/My-Grunt-Boilerplate
 [8]: https://github.com/gruntjs/grunt-contrib-concat
 [9]: https://github.com/gruntjs/grunt-contrib-concat#usage-examples

 [10]: https://github.com/chriscoyier/My-Grunt-Boilerplate/blob/master/Gruntfile.js
 [11]: https://github.com/gruntjs/grunt-contrib-uglify
 [12]: img/uglify-code.gif
 [13]: https://github.com/gruntjs/grunt-contrib-imagemin
 [14]: img/squished-images.gif
 [15]: https://github.com/gruntjs/grunt-contrib-watch
 [16]: img/error-running-grunt.gif
 [17]: https://github.com/sindresorhus/grunt-sass
 [18]: https://github.com/gruntjs/grunt-contrib-sass
 [19]: http://sass-lang.com/install

 [20]: http://feedback.livereload.com/knowledgebase/articles/86242-how-do-i-install-and-use-the-browser-extensions-
 [21]: img/style-injection.gif
 [22]: http://www.smashingmagazine.com/2013/06/11/front-end-ops/
 [23]: http://css-tricks.com/autoprefixer/
 [24]: https://github.com/pivotal/jasmine
 [25]: https://github.com/filamentgroup/grunticon
 [26]: http://philipwalton.com/articles/introducing-html-inspector/
 [27]: http://csslint.net/
 [28]: http://www.jshint.com/
 [29]: http://leanmeanfightingmachine.github.io/dploy/
 [30]: https://github.com/vladikoff/grunt-devtools