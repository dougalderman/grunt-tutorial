A Simple Guide to Getting Started With Grunt
  Chris Sevilleja (@sevilayha)  April 4, 2014	 25  Tutorials	 dev environment, grunt, node.js
12 shares
 0
 12
VIEW CODE
During our web development workflows, there are many tasks that we have to repeat. These are tasks things like minifying JavaScript and CSS files, unit testing, linting your files to check for errors, compiling CSS preprocessor files (LESS, SASS), and much more. Grunt is a task runner. This means that those repetitive tasks we deal with in our daily workflow gets automated. This will be a simple look at how to get up and running with Grunt. We will look at doing the basic tasks:
Linting our JS files
Minifying JS files
Compiling LESS files
Minifying CSS files
Watching our files for changes and doing the above tasks
A Quick Look at Grunt in Action

Let’s say you wanted to check for errors in your JavaScript file. After you have setup Grunt, just run

$ grunt jshint
and you will see the errors in your JavaScript files!

grunt-jshint It’s that easy to use Grunt. Just set it up and then call the task you want. Grunt will run the task for you. That’s probably the official Grunt site calls it a taskrunner.
Making This Super Simple

There are many tutorials out there that talk about the great things you can do with Grunt. While great, sometimes they come with confusing and fully fledged configurations that are hard to understand for someone just getting into Grunt. This will be

a guide in the basics of using Grunt and creating an incredibly simple and easy setup to handle the tasks mentioned above. This setup will teach us the basics but will let us think how we can expand Grunt for future more advanced uses.

Getting Started

To use Grunt, we will need to have

Node.js installed. Don’t worry, you can use Grunt in any application you want, whether it is a Node app, a PHP app, WordPress, or just a plain old HTML/CSS/JS site. Node and its package manager (npm) are used to pull in the packages we need. Each package will have a different function like minifying or linting. If you don’t already have Node installed on your computer, go ahead and grab it and we’ll start working with Grunt. To make sure that you have Node and npm installed, go into your command line and type node -v and npm -v. If you see version numbers, then you’re ready to go!

Overview

We’ll keep our project files very simple. This will be the file structure for our samples. Go ahead and create the folders and files shown. We’ll leave our files blank and start adding to them later.

- dist              // will hold all our of final files (minified files)
----- css
----- js
- src               // will hold all of our original files
----- css
---------- style.css
---------- pretty.less
----- js
---------- magic.js
Gruntfile.js        // our Grunt configuration
package.json        // our npm pacakge configuration (how we pull in packages)

Notice how we have a

src folder and a dist folder. We will be doing our work inside the src folder and Grunt will minify those files and save them into the dist folders. The files in dist are the ones that we will use for our final site.
Getting The Grunt Packages We Need

When using npm, we define the packages we need in a

package.json file. Let’s go into that file and add the packages we need. We’ll explain what each package does also.

// package.json

{
  "name": "grunt-getting-started",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "~0.4.4",
    "grunt-contrib-jshint": "latest",
    "jshint-stylish": "latest",
    "grunt-contrib-uglify": "latest",
    "grunt-contrib-less": "latest",
    "grunt-contrib-cssmin": "latest",
    "grunt-contrib-watch": "latest"
  }
}

Here we have defined the

name of our project, the version, and the devDependencies. This may seem weird at first for someone that hasn’t used Node or npm before, but you’ll see how npm is a very cool package manager for our project very soon.
Grunt Packages

You may be wondering what all of the

grunt-contrib-**** packages do. Here’s a handy table of the popular pacakages.

Plugin	Description
contrib-jshint	Validate files using jshint
contrib-uglify	Minify JS files using UglifyJS
contrib-watch	Run tasks whenever watched files are changed
contrib-clean	Clean up files and folders
contrib-copy	Copy files and folders
contrib-concat	Combine files into a single file
contrib-cssmin	Compress CSS files
contrib-less	Compile LESS files to CSS
contrib-imagemin	Minify PNG, JPG, and GIFs
contrib-compass	Compile SASS to CSS using Compass
contrib-htmlmin	Minify HTML files
For the full list of packages, visit the

Grunt plugin repository. Now that we have the packages we need defined, let’s install them.
Installing The Packages
With our

package.json file ready to go, go into your command line and type:

$ npm install
You will see npm do its thing and pull those packages into a newly created

node_modules folder. Now we have these packages and are ready to use them in our project. Now that our setup is all ready to go, let’s set up our tasks for Grunt to do!

Grunt Setup and Configuration

To define our configuration for Grunt, we will use our

Gruntfile.js file. This is the default place where our settings will go.
The Base Gruntfile
In our

Gruntfile.js, let’s go ahead and add in the basic things we need for our project.

// Gruntfile.js

// our wrapper function (required by grunt and its plugins)
// all configuration goes inside this function
module.exports = function(grunt) {

  // ===========================================================================
  // CONFIGURE GRUNT ===========================================================
  // ===========================================================================
  grunt.initConfig({

    // get the configuration info from package.json ----------------------------
    // this way we can use things like name and version (pkg.name)
    pkg: grunt.file.readJSON('package.json'),

    // all of our configuration will go here

  });

  // ===========================================================================
  // LOAD GRUNT PLUGINS ========================================================
  // ===========================================================================
  // we can only load these if they are in our package.json
  // make sure you have run npm install so our app can find these
  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-uglify');
  grunt.loadNpmTasks('grunt-contrib-less');
  grunt.loadNpmTasks('grunt-contrib-cssmin');
  grunt.loadNpmTasks('grunt-contrib-watch');

};

We will use the

module.exports (wrapper) function. This is the Node way of exposing our configuration to the rest of our application. We don’t have to worry about this too much, but if you are interested, read this great article on the topic. Inside of our grunt.initConfig(), we have taken the information from our package.json and saved it to pkg. With this, we can use the attributes from our package.json file. We can call the name of our project using pkg.name and the version with pkg.version. We could also expand this and even use an author. Why do we need this? Good question. We’ll see the usage in a bit, but one of the cool things we can do is use these attributes to create comments at the top of our files with project name, author, date built, and version! Pretty neat. We have also loaded our grunt plugins using grunt.loadNpmTasks(). This is the way we can use the plugins that we brought in earlier using npm.
Package Configuration
With our basic Grunt configuration ready, let’s look at configuring one of our packages. Let’s start with the JSHint package to lint our JavaScript files and tell us if there are any errors. This is the way for Grunt to know which files we want it to lint, minify, or anything else we want to do. When we configure packages, it will go into our

grunt.initConfig() section and it will follow a certain structure. Here is the basic structure of configuring a Grunt package:

// Gruntfile.js

grunt.initConfig({

    // configure jshint to validate js files -----------------------------------
    jshint: {
      options: {
        reporter: require('jshint-stylish') // use jshint-stylish to make our errors look and read good
      },

      // when this task is run, lint the Gruntfile and all js files in src
      build: ['Gruntfile.js', 'src/**/*.js']
    }

});

This will be the basic format for how we configure our packages. We will:

Call the name of the package (jshint)
Set options if we have to. These are usually found on the docs for each specific package
Create a build attribute and pass in files, directories, or anything else we want.
Naming Conventions
When naming the tasks, we are going to name our main task

build. You can name this what you want and you could even create more than one task. When you run grunt, all of the tasks will automatically be run. If you wanted to create tasks within the jshint configuration, you could name them dev and production. Then we can call the tasks later by using jshint:dev or jshint:production. Now that we have seen the basic setup for a Grunt package, let’s go ahead and start creating configurations for the tasks we want to do.

Linting JavaScript Files

Here is the configuration for linting JavaScript files. It is the same as the example above. We are also bringing in the

'jshint-stylish' package to make our error output look good.

// Gruntfile.js

grunt.initConfig({

    ...

    // configure jshint to validate js files -----------------------------------
    jshint: {
      options: {
        reporter: require('jshint-stylish') // use jshint-stylish to make our errors look and read good
      },

      // when this task is run, lint the Gruntfile and all js files in src
      build: ['Grunfile.js', 'src/**/*.js']
    }

});

Go ahead and add in some JS into

src/js/magic.js

// src/js/magic.js

var hello = 'look im grunting!'

var awesome = 'yes it is awesome!'

Now if we run:

$ grunt jshint
in our command line, we’ll see it lint the Gruntfile and all JS files inside the

src folder.

grunt-jshint-errors
We can tell it to watch all JS files in our application, specific files, or all the files in a given folder using the

** for all folders and * for all files. With linting out of the way, let’s look at minifying.

Minifying JavaScript Files

We’ll follow the same format. We will call the

uglify package, configure it, and tell it what files to use and create.

// Gruntfile.js
grunt.initConfig({

    // get the configuration info from package.json ----------------------------
    // this way we can use things like name and version (pkg.name)
    pkg: grunt.file.readJSON('package.json'),

    ...

    // configure uglify to minify js files -------------------------------------
    uglify: {
      options: {
        banner: '/*\n <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> \n*/\n'
      },
      build: {
        files: {
          'dist/js/magic.min.js': 'src/js/magic.js'
        }
      }
    }

});
Here we are configuring an option called

banner. This will add a nice comment to the top of our minified file. Notice we are using the pkg.name from the package.json file. In our build, we are defining the file we want to create (dist/js/magic.min.js) from the src file (src/js/magic.js). We will want more code in our file than just those two lines we made so let’s go grab a giant file to minify. Let’s go get jQuery. Copy everything from the unminified jQuery file and paste it into our src/js/magic.js file. See how they have their cool comment at the top of the file? We can build one of those out using the banner option. With our file, let’s go ahead and minify it. Go into your console and type:

$ grunt uglify
grunt-uglify
Now we have taken our file from the original 360kb to a nice sized 97.1kb!

Minifying Multiple Files into One
We can also do much more than just minify a single file. We can take multiple files and minify them to one output file. This can speed up our sites since we are only serving one JS file to the users visiting our site. To configure multiple files is straightforward:

// Gruntfile.js
grunt.initConfig({

    // get the configuration info from package.json ----------------------------
    // this way we can use things like name and version (pkg.name)
    pkg: grunt.file.readJSON('package.json'),

    ...

    // configure uglify to minify js files -------------------------------------
    uglify: {
      options: {
        banner: '/*\n <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> \n*/\n'
      },
      build: {
        files: {
          'dist/js/magic.min.js': ['src/js/magic.js', 'src/js/magic2.js']
        }
      }
    }

});
 
In this example, we are taking these two files and minifying them into

magic.min.js. We can also use the shortcut we learned earlier and just say all js files in the src folder should be minified. You would use (src/**/*.js) for that.

Compiling LESS to CSS

While we will be using LESS for this example, you can also use the

grunt-contrib-compass package to do the same for SASS. Here’s our configuration for compiling LESS files. Like our minifying JS example, we will use our config to define the source and output files.

// Gruntfile.js
grunt.initConfig({

    ...

    // compile less stylesheets to css -----------------------------------------
    less: {
      build: {
        files: {
          'dist/css/pretty.css': 'src/css/pretty.less'
        }
      }
    }

});
For this example, we aren’t going to be setting any options. Just compile our

src/css/pretty.less to a dist/css/pretty.css file. Let’s add in some LESS into our source file.

/* src/css/pretty.less */

@red        : #CC594A;
@yellow     : #B8CC24;
@blue       : #8BC5FF;
@purple     : #6F3596;

body        { 
    background:@red;
    color:@yellow;
}

button      {
    background:@blue;
}

div         {
    background:@purple;
}

Now we have defined some LESS variables and applied them to our elements. Let’s go ahead and run our task!

$ grunt less
grunt-less-compile
Now we have created the

dist/css/pretty.css file all of our LESS compiled to CSS and that file now looks like a normal stylesheet.

/* dist/css/pretty.css */

body {
  background: #cc594a;
  color: #b8cc24;
}
button {
  background: #8bc5ff;
}
div {
  background: #6f3596;
}

Let’s look at two more things we can do with Grunt.

Minifying CSS Files

Like minifying JS files, this configuration will be straightforward. We get the drill now so let’s go ahead and create this configuration.

// Gruntfile.js
grunt.initConfig({

    ...

    // configure cssmin to minify css files ------------------------------------
    cssmin: {
      options: {
        banner: '/*\n <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> \n*/\n'
      },
      build: {
        files: {
          'dist/css/style.min.css': 'src/css/style.css'
        }
      }
    }

});
Now if you run:

$ grunt cssmin
We will have our newly minified

dist/css/style.min.css file.

Running Multiple Tasks at Once

Now that we see how all of our tasks work, let’s make things more efficient and run everything with just one task. This is much better than running separate calls for

grunt uglify, grunt jshint, and so on.
Default Task
With Grunt, you can create tasks that will run multiple tasks at the same time. For example, let’s say we wanted to do all of our tasks above by just calling

grunt. When you run grunt from the command line, Grunt will look for a task called default. Let’s create that now so we can see what it looks like.

// Gruntfile.js

grunt.initConfig({

  ...

  // ============= // CREATE TASKS ========== //
  grunt.registerTask('default', ['jshint', 'uglify', 'cssmin', 'less']); 


});
Now just run:

$ grunt
and all of the tasks in our

default task will run! Now let’s look at how we can register different tasks for different environments.

Different Tasks for Different Environments

Let’s say we want to have Grunt work for us in a development environment and then when we go to production, we want different tasks to be run. We could define multiple tasks inside of each configuration. For example:

// Gruntfile.js

grunt.initConfig({

  // get the configuration info from package.json ---------------------------- 
  // this way we can use things like name and version (pkg.name) 
  pkg: grunt.file.readJSON('package.json'),

...

// configure uglify to minify js files ------------------------------------- 
  uglify: { 
    options: { 
      banner: '/\n <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> \n/\n' 
    }, 
    dev: { 
      files: { 'dist/js/magic.min.js': ['src/js/magic.js', 'src/js/magic2.js'] } 
    }, 
    production: { 
      files: { 'dist/js/magic.min.js': 'src/**/*.js' } 
    } 
  }
});
Now we can call them differently using a colon. Let’s create a task for development and production.

// Gruntfile.js

grunt.initConfig({

  // ========= // CREATE TASKS =========

  // this default task will go through all configuration (dev and production) in each task 
  grunt.registerTask('default', ['jshint', 'uglify', 'cssmin', 'less']);

  // this task will only run the dev configuration 
  grunt.registerTask('dev', ['jshint:dev', 'uglify:dev', 'cssmin:dev', 'less:dev']);

  // only run production configuration 
  grunt.registerTask('production', ['jshint:production', 'uglify:production', 'cssmin:production', 'less:production']);

});
Now we can call development by running:

$ grunt dev
or production by running:

$ grunt production
Now we’ve seen how we can create multiple configurations for our tasks and call them differently. Let’s look at the last thing we’ll be creating today. We’ll watch our files and have Grunt run every time a file changes!

Watching For Changes and Running Tasks

The watch task will run every time a file is changed and saved. All we have to do is configure it to watch certain files and tell it what to do when those files are changed. We’re going to break from the basic

build configuration we’ve been using. Here we’ll separate it out into stylesheets and scripts. We do this because we want to use different tasks for each.

// Gruntfile.js

grunt.initConfig({

...

// configure watch to auto update ----------------
watch: {
  
  // for stylesheets, watch css and less files 
  // only run less and cssmin stylesheets: { 
  files: ['src//*.css', 'src//*.less'], 
  tasks: ['less', 'cssmin'] },

  // for scripts, run jshint and uglify 
  scripts: { 
    files: 'src/**/*.js', tasks: ['jshint', 'uglify'] 
  } 
}

});
Now we have configured watch to watch our stylesheets and scripts. In your console, run:

$ grunt watch
Now we can see that Grunt will watch for changes and run the tasks it needs to.

grunt-watch-example
In this picture, a JavaScript file and a CSS file was changed. The respective tasks were run and we can go on developing! This is a very powerful tool since we can do things like linting our files every time we save, compiling LESS, and we can even

minify images.

Conclusion

Hopefully this simplified look at Grunt will begin to grow some ideas about how to use Grunt for your specific workflow. Getting up and running is as easy as

installing a package, configuring the package, and typing grunt! Definitely look through the official docs and the plugins list to get more ideas on how to use Grunt.

VIEW CODE
 
CHRIS SEVILLEJA	(@SEVILAYHA)
Design, development, and anything in between that I find interesting.
 View My Articles
READ NEXT
USING GRUNTJS IN A MEAN STACK APPLICATION
AUTOMATE YOUR TASKS EASILY WITH GULP.JS
GETTING STARTED WITH VIM: AN INTERACTIVE GUIDE
A QUICK GUIDE TO USING LIVERELOAD WITH GULP
GETTING STARTED WITH LESS
THE COMPLETE VISUAL GUIDE TO SUBLIME TEXT 3: GETTING STARTED AND KEYBOARD SHORTCUTS

