#Gulp前端自动化构建
gulp_demo

####张磊  [github](https://github.com/zhnaglei/gulp)
        风流倜傥小小张
#1.1什么是gulp？
gulp就是一个构建工具。
#1.2安装gulp
###1.全局安装
$ npm install --global gulp
###2.开发依赖安装
$ npm install --save-dev gulp
#1.3创建gulpfile
###在项目根目录下创建一个gulpfile.js 的文件:
```
var gulp = require('gulp');

gulp.task('default', function() {
        //将你的默认的任务代码放在这
});
```
#1.4运行gulp
$ gulp

#2编写gulp
###2.1运行hello world
```
var gulp = require('gulp');

gulp.task('default', function() {
    console.log('hello world')
});
```
执行命令:gulp
###2.2依赖作业
```
gulp.task('default',['watch'],function() {
    console.log('default task');
});
```
['watch']这是依赖的作业列表，它们是由顺序的，按数组顺序依次执行 第三个参数是成功执行完上面的依赖作业后执行的回调函数

这里要强调，依赖作业数组里的都执行完了，才会执行第三个参数，即当前作业具体内容

多个依赖定义:
```
gulp.task('default',['watch','task_2','task_3'],function() {
    console.log('依赖作业终于执行完了，下面是我的舞台....');
});
```
###2.3流式处理
比如混淆压缩js，使用gulp-uglify插件
```
var gulp = require('gulp');
var uglify = require('gulp-uglify');

gulp.task('default', function() {
    gulp.src('src/*.js')
        .pipe(uglify())   //压缩js
        .pipe(gulp.dest('dist'))
});
```
#3.常见npm的使用
* 编译less  gulp——less
* 合并文件    gulp-concat
* 最小化js文件 gulp-uglify
* 重命名文件     gulp-rename
* 最小化CSS文件  gulp-cssnano
* 压缩HTML文件 gulp-minify-html
* 最小化图像 gulp-imagemin

#4.具体可参考gulpfile.js文件

```
'use strict';
// 载入Gulp模块
var gulp = require('gulp');
var less = require('gulp-less');
var autoprefixer = require('gulp-autoprefixer');
var cssnano = require('gulp-cssnano');
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var browserSync = require('browser-sync');
var reload = browserSync.reload;

// 注册样式编译任务
gulp.task('style', function() {
  gulp.src('src/styles/*.less')
    .pipe(less())
    .pipe(autoprefixer({
      browsers: ['last 2 versions']
    }))
    .pipe(cssnano())
    .pipe(gulp.dest('dist/styles'))
    .pipe(reload({
      stream: true
    }));
});

// 注册脚本合并压缩任务
gulp.task('script', function() {
  gulp.src('src/scripts/*.js')
    .pipe(concat('app.js'))
    .pipe(uglify())
    .pipe(gulp.dest('dist/scripts'))
    .pipe(reload({
      stream: true
    }));
});

gulp.task('image', function() {
  gulp.src('src/images/*.*')
    .pipe(gulp.dest('dist/images'))
    .pipe(reload({
      stream: true
    }));
})

gulp.task('html', function() {
  gulp.src('src/*.html')
    .pipe(htmlmin({
      collapseWhitespace: true,
      collapseBooleanAttributes: true,
      removeAttributeQuotes: true,
      removeComments: true,
      removeEmptyAttributes: true,
      removeScriptTypeAttributes: true,
      removeStyleLinkTypeAttributes: true,
    }))
    .pipe(gulp.dest('dist'))
    .pipe(reload({
      stream: true
    }));
});

gulp.task('serve', ['style', 'script', 'image', 'html'], function() {
  browserSync({
    notify: false,
    port: 2015,
    server: {
      baseDir: ['dist']
    }
  });


  gulp.watch('src/styles/*.less', ['style']);
  gulp.watch('src/scripts/*.js', ['script']);
  gulp.watch('src/images/*.*', ['image']);
  gulp.watch('src/*.html', ['html']);
});
```