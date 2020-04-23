# 01 淘宝可伸缩布局方案 flexible

> package.json

```json
{
  "name": "ren_da_magazine",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "gulp",
    "build": "gulp build"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "autoprefixer": "^9.7.6",
    "cssnano": "^4.1.10",
    "gulp": "^4.0.2",
    "gulp-connect": "^5.7.0",
    "gulp-postcss": "^8.0.0",
    "gulp-run-command": "0.0.10",
    "gulp-sass": "^4.0.2",
    "gulp-sourcemaps": "^2.6.5",
    "postcss-px2rem": "^0.3.0",
    "serve": "^11.2.0"
  }
}
```

> gulp.js

```js
const { src, dest, series, watch } = require('gulp');
const sass = require('gulp-sass');
const sourcemaps = require('gulp-sourcemaps');
const postcss = require('gulp-postcss');
const px2rem = require('postcss-px2rem')
const autoprefixer = require('autoprefixer');
const cssnano = require('cssnano');
// const prettier = require('gulp-prettier');
// const gulpif = require('gulp-if');
// const rename = require('gulp-rename');

const files = {
  scssPath: ['scss/*.scss', 'scss/*.css']
};

function isScss(file) {
  return file.extname === '.scss';
}

function scssTask() {
  var processors = px2rem({remUnit: 75});
  return src(files.scssPath)
    .pipe(sourcemaps.init())
    .pipe(sass())
    .pipe(postcss([autoprefixer(),processors]))
    .pipe(sourcemaps.write('.'))
    .pipe(dest('css'));
}

function watchTask() {
  watch(files.scssPath, scssTask);
}

function buildFile() {
  var processors = px2rem({remUnit: 75});
  return src(files.scssPath)
    .pipe(sourcemaps.init())
    .pipe(sass())
    .pipe(postcss([autoprefixer(),processors, cssnano()]))
    .pipe(sourcemaps.write('.'))
    .pipe(dest('dist'));
}

exports.default = series(scssTask, watchTask);
exports.build = buildFile;

```