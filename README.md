# grunt-awssum-deploy

> Upload your static assets to an S3 bucket
> Based on grunt-awssum-deploy by [Jordan Sitkin](http://github.com/dustMason)

## Getting Started
This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-awssum-deploy-branch --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-awssum-deploy-branch');
```

To Run:
```js
grunt s3deploy --production
```
Defaults to target = staging.

## The "s3deploy" task

### Overview
In your project's Gruntfile, add a section named `s3deploy` to the data object passed into `grunt.initConfig()`.

### Options

#### options.key
Type: `String`

Your AWS access key, **mandatory**.

#### options.secret
Type: `String`

Your AWS secret, **mandatory**.

#### options.stagingBucket
Type: `String`

The bucket to upload to for deploying to staging, **mandatory**.

#### options.productionBucket
Type: `String`

The bucket to upload to for deploying to production. **mandatory**.

#### options.connections
Type: `Integer`

How many concurrent connections to keep open while uploading to S3. Defaults to **3**.

#### options.nocache
Type: `String`

Array of filepaths to apply s3 metadata Cache-Control max-age=0 to. Useful for preventing caching when using s3 with Cloudfront

### File Options

#### ContentType
Type: `String`

Define Content-Type for file. Defaults to determining content-type automatically based on filename

### Note
The project is based on [awssum](http://awssum.io), and uses code from a [gist](https://gist.github.com/chilts/3687910) by @chilts and @twhid.

### Usage Examples

```js
grunt.initConfig({
  s3deploy: {
    options: {
      key: '<%= secret.awsKey %>',
      secret: '<%= secret.awsSecret %>',
      stagingBucket: 'staging',
      productionBucket: 'prod',      
      access: 'public-read',
      connections: 5,
      nocache: ['index.html'] // files to to set cache-control max-age=0 on in s3 metadata
    },
    dist: {
      files: [{
        expand: true,
        cwd: 'dist/',
        src: '**/*.*', // note: you must not include directories
        dest: './'
      }]
    },
    snapshots: {
      files: [{
        expand: true,
        cwd: 'dist/snapshots/',
        src: '**/*.*', // note: you must not include directories
        dest: './',
        contentType: 'text/html'
      }]
    }
  }
})
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style.

## Release History
* 2013-07-28   v0.1.0   First release
* 2013-07-28   v0.1.1   Added async queue and connections option
* 2013-08-01   v0.1.2   Automatically set ContentType property on S3 using mime package
