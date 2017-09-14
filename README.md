# TSM Theme for MDCSS

This serves as a slightly branded theme for style guides built with [mdcss](https://github.com/jonathantneal/mdcss).

## Setup

To setup a style guide in a project, first install both `mdcss` and this style guide theme, `mdcss-theme-tsm`:

```
npm install mdcss @subjectmatter/mdcss-theme-tsm --save
```

Now that everything is installed, you'll need to set up a gulp task to build your style guide. Details on how to do so can be found [here](https://github.com/jonathantneal/mdcss#gulp) in the MDCSS documentation. Here is a sample:

```
import gulp from 'gulp'
import sass from 'gulp-sass'
import postcss from 'gulp-postcss'

gulp.task('styles', () => {
  return gulp.src('src/sass/style.scss')
    .pipe( sass().on('error', sass.logError) )
    .pipe( postcss([
      require('mdcss')({
        theme: require('@subjectmatter/mdcss-theme-tsm')
      })
    ]))
    .pipe( gulp.dest('./') )
})
```

Now, running `gulp styles` from the command line will generate your style guide, by default, in a `styleguide/` directory at the root level of your project.

The best practice is to develop the style guide in isolation. When doing so, it's helpful to have a `watcht` task that will reload the style guide using `browserync`. A sample task for that would look like this:

```
import gulp         from 'gulp'
import browserSync  from 'browser-sync'

/*
 * A task for rebuilding your stylesheet when any scss files change
 */
gulp.task('styles', () => {
  return gulp.src('src/sass/style.scss')
    .pipe( sass().on('error', sass.logError) )
    .pipe( postcss([
      require('mdcss')({
        theme: require('@subjectmatter/mdcss-theme-tsm')
      })
    ]))
    .pipe( gulp.dest('./') )
})

/*
 * A task for moving the updates style.css into your styleguide
 */
gulp.task('styleguide', () => {
  return gulp.src('style.css')
    .pipe(gulp.dest('./styleguide'))
})

/*
 * Finally, a task to watch your styleguide for changes, serve it on 
 * localhost:4000 and reload it whenever it gets updated
 */
gulp.task('watch:styleguide', () => {

  browserSync.create().init({
    notify: true,
    port: 4000,
    ui: {
      port: 4001
    },
    server: 'styleguide',
    files: [
      'styleguide/index.html',
      'styleguide/style.css'
    ],
    open: false
  })

  gulp.watch('src/sass/**/*', ['styles'])

})
```

## Further Reading
- [MDCSS Options](https://github.com/jonathantneal/mdcss#options)
- [Documenting your CSS with MDCSS](https://github.com/jonathantneal/mdcss#writing-documentation)
- [A sample theme](http://jonathantneal.github.io/mdcss-theme-github/demo/)
