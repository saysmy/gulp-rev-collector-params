
Based on [gulp-rev-collector](https://www.npmjs.com/package/gulp-rev-collector)


> Static asset revision data collector from manifests, generated from different streams, and replace their links in html template.
> Static asset revisioning by appending content hash to filenames
> `recharge.css` → `recharge.css?v=66704ea636`

## Install

```sh
$ npm install --save gulp-rev-collector-params
```

## Usage

We can use [gulp-rev-params](https://www.npmjs.com/package/gulp-rev-params) to cache-bust several assets and generate manifest files for them. Then using gulp-rev-collector-params we can collect data from several manifest files and replace links to assets in html templates.

```js
var gulp         = require('gulp');
var rev = require('gulp-rev-params');

gulp.task('css', function () {
    return gulp.src('src/css/*.css')
        .pipe(rev())
        .pipe(gulp.dest('dist/css'))
        .pipe( rev.manifest() )
        .pipe( gulp.dest( 'rev/css' ) );
});

gulp.task('scripts', function () {
    return gulp.src('src/js/*.js')
        .pipe(rev())
        .pipe(gulp.dest('dist/js'))
        .pipe( rev.manifest() )
        .pipe( gulp.dest( 'rev/js' ) );
});

...

var revCollector = require('gulp-rev-collector-params');
var minifyHTML   = require('gulp-minify-html');

gulp.task('rev', function () {
    return gulp.src(['rev/**/*.json', 'templates/**/*.html'])
        .pipe( revCollector({
            replaceReved: true,
            dirReplacements: {
                'css': '/dist/css',
                '/js/': '/dist/js/',
                'cdn/': function(manifest_value) {
                    return '//cdn' + (Math.floor(Math.random() * 9) + 1) + '.' + 'exsample.dot' + '/img/' + manifest_value;
                }
            }
        }) )
        .pipe( minifyHTML({
                empty:true,
                spare:true
            }) )
        .pipe( gulp.dest('dist') );
});
```

### Options
Please find [gulp-rev-collector](https://www.npmjs.com/package/gulp-rev-collector)

### Works with gulp-rev-params

- [gulp-rev-params](https://www.npmjs.com/package/gulp-rev-params)

## License

[MIT](http://opensource.org/licenses/MIT)
