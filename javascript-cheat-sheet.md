# Javascript Cheat Sheet

## Initial Setup

### Create root directory

### Create initial business logic file based off name of object

### Initialize npm in root project directory

```
  $ npm init
```

### Create name of project when prompted by npm. Don't use spaces.

```
  project-name
```

### Install all dependencies
```
npm i -D gulp browserify vinyl-source-stream gulp-concat gulp-uglify gulp-util del jshint gulp-jshint
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

gulp.task("build", ['clean'], function(){
if (buildProduction) {
  gulp.start('minifyScripts');
} else {
  gulp.start('jsBrowserify');
}
});

gulp.task("clean", function(){
return del(['build', 'tmp']);
});

gulp.task('jshint', function(){
return gulp.src(['js/*.js'])
.pipe(jshint())
.pipe(jshint.reporter('default'));
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


### Create .gitignore file in root

```
  node_modules/
  .DS_Store
```


### Place script tag in `index.html`

```
  <script type="text/javascript" src="build/js/app.js"></script>
```


### Run `jslint` in terminal to check for errors

```
  gulp jslint
```
