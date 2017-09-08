# Javascript Cheat Sheet

## Initial Setup

### Create root directory

### Create initial business logic file based off name of object

## Things to install Globally on personal machine
```
npm install -g karma-cli
```

```
$ npm install bower -g
```

```
sudo npm install gulp -g
```

### Initialize npm & bower in root project directory

```
   npm init
```
### Create name of project when prompted by npm. Don't use spaces.

```
  project-name
```

```
   bower init
```

### Install all dev dependencies
```
npm i -D gulp browserify vinyl-source-stream gulp-concat gulp-uglify gulp-util del jshint gulp-jshint bower-files browser-sync jasmine karma karma-jasmine jasmine-core karma-chrome-launcher karma-cli karma-browserify karma-jquery karma-jasmine-html-reporter watchify babelify babel-preset-es2015
```

### Install jasmine
```
$ npm install jasmine --save-dev
```

### Initialize karma
```
karma init
```

### Initialize jasmine

```
$ ./node_modules/.bin/jasmine init
```

### Install all prod dependencies

```
bower i -S jquery bootstrap moment
```

### Create `gulpfile.js` in top-level of root directory and add the code below

```
var gulp = require('gulp');
var concat = require('gulp-concat');
var browserify = require('browserify');
var source = require('vinyl-source-stream');
var uglify = require('gulp-uglify');
var utilities = require('gulp-util');
var del = require('del');
var jshint = require('gulp-jshint');
var browserSync = require('browser-sync').create();
var lib = require('bower-files')({
  "overrides":{
    "bootstrap" : {
      "main": [
        "less/bootstrap.less",
        "dist/css/bootstrap.css",
        "dist/js/bootstrap.js"
      ]
    }
  }
});

var buildProduction = utilities.env.production;
var babelify = require("babelify");

gulp.task('concatInterface', function(){
return gulp.src(['./js/*-interface.js'])
  .pipe(concat('allConcat.js'))
  .pipe(gulp.dest('./tmp'));
});

gulp.task('jsBrowserify', ['concatInterface'], function() {
  return browserify({ entries: ['./tmp/allConcat.js']})
    .transform(babelify.configure({
      presets: ["es2015"]
    }))
    .bundle()
    .pipe(source('app.js'))
    .pipe(gulp.dest('./build/js'))
});

gulp.task("minifyScripts", ["jsBrowserify"], function(){
return gulp.src("./build/js/app.js")
  .pipe(uglify())
  .pipe(gulp.dest("./build/js"));
});

gulp.task('build', ['clean'], function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
  gulp.start('bower');
});

gulp.task("clean", function(){
return del(['build', 'tmp']);
});

gulp.task('jshint', function(){
return gulp.src(['js/*.js'])
.pipe(jshint())
.pipe(jshint.reporter('default'));
});

gulp.task('bowerJS', function () {
  return gulp.src(lib.ext('js').files)
    .pipe(concat('vendor.min.js'))
    .pipe(uglify())
    .pipe(gulp.dest('./build/js'));
});

gulp.task('bowerCSS', function () {
  return gulp.src(lib.ext('css').files)
    .pipe(concat('vendor.css'))
    .pipe(gulp.dest('./build/css'));
});

gulp.task('bower', ['bowerJS', 'bowerCSS']);

gulp.task('serve', function() {
  browserSync.init({
    server: {
      baseDir: "./",
      index: "index.html"
    }
  });
});

gulp.task('serve', function() {
  browserSync.init({
    server: {
      baseDir: "./",
      index: "index.html"
    }
  });

  gulp.watch(['js/*.js'], ['jsBuild']);
  gulp.watch(['bower.json'], ['bowerBuild']);

});

gulp.task('jsBuild', ['jsBrowserify', 'jshint'], function(){
  browserSync.reload();
});

gulp.task('bowerBuild', ['bower'], function(){
  browserSync.reload();
});

```
### Populate karma.conf.js

```
module.exports = function(config) {
  config.set({
    basePath: '',
    frameworks: ['jquery-3.2.1', 'jasmine', 'browserify'],
    files: [
      'js/*.js',
      'spec/*-spec.js',
    ],
    exclude: [
    ],
    preprocessors: {
      'js/*.js': [ 'browserify'],
      'spec/*.js': ['browserify'],
    },
    plugins: [
      'karma-jquery',
      'karma-browserify',
      'karma-jasmine',
      'karma-chrome-launcher',
      'karma-jasmine-html-reporter'
    ],
    browserify: {
       debug: true,
       transform: [ [ 'babelify', {presets: ["es2015"]} ] ]
     },

    reporters: ['progress', 'kjhtml'],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    browsers: ['Chrome'],
    singleRun: false,
    concurrency: Infinity
  })
}
```

### Edit `package.json` file
```
"scripts": {
  "test": "jasmine"
}
```
or

```
"scripts": {
  "test": "karma start karma.conf.js"
}
```

### Create .jshintrc in the root directory

```
{ "esversion":6 }
```

### Create a constructor in business logic and corresponding prototype methods

```
  function Calculator(constructorParameter) {
    // constructor
  }
```

```
  Calculator.prototype.pingPong = function(methodParameter) {
    // method code
  }
```


### Instantiated the object in interface and called method with arguments from user

```
  var simpleCalculator = new Calculator("hot pink");
  var output = simpleCalculator.pingPong(goal);
 ```

### Use `exports` at bottom of business logic to turn it into a node module

```
  exports.calculatorModule = Calculator;
```

### Use `require` at top of UI logic to bring in the business logic

```
  var Calculator = require('./../js/pingpong.js').calculatorModule;
```


### Create `.gitignore` file in root

```
  node_modules/
  bower_components/
  build/
  tmp/
  .DS_Store
```

## Testing with Jasmine and Karma

### Initialize `jasmine`

```
./node_modules/.bin/jasmine init
```




### Run `npm` test
```
npm test
```


### Place script tag in `index.html` for gulp and bower

```
  <link rel="stylesheet" href="build/css/vendor.css">
  <script src="build/js/vendor.min.js"></script>
  <script type="text/javascript" src="build/js/app.js"></script>
```


### Run `jslint` in terminal to check for errors

```
  gulp jslint
```

## When cloning a project that uses `npm` and `bower/webpack`

### Install `npm` and `bower` dependencies

```
npm install
```
```
bower install
```
