# Javascript Cheat Sheet

## Initial Setup

### Create root directory

### Create initial business logic file based off name of object

### Initialize npm & bower in root project directory

```
   npm init
```

```
   bower init
```

### Create name of project when prompted by npm. Don't use spaces.

```
  project-name
```

### Install all dev dependencies
```
npm i -D gulp browserify vinyl-source-stream gulp-concat gulp-uglify gulp-util del jshint gulp-jshint bower-files browser-sync jasmine karma karma-jasmine jasmine-core karma-chrome-launcher karma-cli karma-browserify karma-jquery karma-jasmine-html-reporter watchify
```

### Initialize karma
```
karma init
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

gulp.task('concatInterface', function(){
return gulp.src(['./js/*-interface.js'])
  .pipe(concat('allConcat.js'))
  .pipe(gulp.dest('./tmp'));
});

gulp.task('jsBrowserify', ['concatInterface'], function() {
return browserify({ entries: ['./tmp/allConcat.js'] })
  .bundle()
  .pipe(source('app.js'))
  .pipe(gulp.dest('./build/js'));
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

### Edit `package.json` file
```
"scripts": {
  "test": "jasmine"
}
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
