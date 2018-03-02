---
title: gulp使用指南
date: 2017-08-22 16:31:52
tags:
---

### gulp启动服务
* gulpfile.js 内容如下
``` bash
var gulp = require('gulp');
var connect = require('gulp-connect');
gulp.task('server', function () {
    connect.server({
        root: './',//path
        host: '10.2.1.181',//ip
        port: '8888',//端口
        livereload: true,
        middleware: function () {
            return [
            ]
        }
    });
});
// 启动
gulp.task('default', ['server']);
```

### gulp之sass编译
* 目录结构

> `dist`
>> `css`
>>> `style.css`

> `scss`
>> `style.scss`

> `gulpfile.js`

* gulpfile.js 内容如下
``` bash
var gulp = require('gulp');
var sass = require('gulp-sass');
var cssmin = require('gulp-minify-css');

gulp.task('sass', function () {
    return gulp.src('scss/**/*.scss')
    .pipe(sass())
    .pipe(cssmin())
    .pipe(gulp.dest('dist/css'))
})

gulp.task('watch', function () {
    gulp.watch('scss/**/*.scss', ['sass']);
})

gulp.task('default', ['watch'])
```

### gulp之js压缩

* 目录结构

> `dist`
>> `scripts`
>>> `all.js`
>>> `all.min.js`

> `scripts`
>> `index.js`
>> `other.js`

> `gulpfile.js`

* gulpfile.js 内容如下
``` bash
var gulp = require('gulp');
var uglify = require('gulp-uglify');

var concat = require('gulp-concat');
var rename = require('gulp-rename');

gulp.task('scripts', function () {
    //读取JS文件，合并，输出到新目录，重新命名，压缩，输出
    return gulp.src('scripts/**/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('dist/scripts'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('dist/scripts'));
})

gulp.task('watch', function () {
    gulp.watch('scripts/**/*.js', ['scripts']);
})

gulp.task('default', ['watch'])
```

### gulp之图片压缩

* 目录结构

> `dist`

> `source`
>> `images`

> `gulpfile.js`

* gulpfile.js 内容如下
``` bash
var gulp = require('gulp');
var imagemin = require('gulp-imagemin');

//压缩图片
gulp.task('imgmin', function () {
  return gulp.src('source/images/*')
    .pipe(imagemin())
    .pipe(gulp.dest('dist'));
});

gulp.task('default', ['imgmin'])
```
