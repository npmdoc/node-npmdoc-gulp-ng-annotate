# api documentation for  [gulp-ng-annotate (v2.0.0)](https://github.com/Kagami/gulp-ng-annotate)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-ng-annotate.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-ng-annotate) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-ng-annotate.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-ng-annotate)
#### Add angularjs dependency injection annotations with ng-annotate

[![NPM](https://nodei.co/npm/gulp-ng-annotate.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/gulp-ng-annotate)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-ng-annotate/build/screenCapture.buildCi.browser.apidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-ng-annotate/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-ng-annotate/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-ng-annotate/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Kagami Hiiragi"
    },
    "bugs": {
        "url": "https://github.com/Kagami/gulp-ng-annotate/issues"
    },
    "dependencies": {
        "bufferstreams": "^1.1.0",
        "gulp-util": "^3.0.7",
        "merge": "^1.2.0",
        "ng-annotate": "^1.2.1",
        "through2": "^2.0.1",
        "vinyl-sourcemaps-apply": "^0.2.1"
    },
    "description": "Add angularjs dependency injection annotations with ng-annotate",
    "devDependencies": {
        "gulp-sourcemaps": "*",
        "mocha": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "84a83db1f016520bd70f9a5cfa9f3fe89e25a205",
        "tarball": "https://registry.npmjs.org/gulp-ng-annotate/-/gulp-ng-annotate-2.0.0.tgz"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "gitHead": "3e8d5040f0e113ab0ea3cdd214710133c08d01ab",
    "homepage": "https://github.com/Kagami/gulp-ng-annotate",
    "keywords": [
        "gulpplugin",
        "angular",
        "angularjs",
        "annotate",
        "ng-annotate"
    ],
    "license": "CC0-1.0",
    "main": "index.js",
    "maintainers": [
        {
            "name": "kagami"
        }
    ],
    "name": "gulp-ng-annotate",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git+https://github.com/Kagami/gulp-ng-annotate.git"
    },
    "scripts": {
        "test": "mocha"
    },
    "version": "2.0.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-ng-annotate](#apidoc.module.gulp-ng-annotate)
1.  [function <span class="apidocSignatureSpan"></span>gulp-ng-annotate (options)](#apidoc.element.gulp-ng-annotate.gulp-ng-annotate)
1.  [function <span class="apidocSignatureSpan">gulp-ng-annotate.</span>toString ()](#apidoc.element.gulp-ng-annotate.toString)



# <a name="apidoc.module.gulp-ng-annotate"></a>[module gulp-ng-annotate](#apidoc.module.gulp-ng-annotate)

#### <a name="apidoc.element.gulp-ng-annotate.gulp-ng-annotate"></a>[function <span class="apidocSignatureSpan"></span>gulp-ng-annotate (options)](#apidoc.element.gulp-ng-annotate.gulp-ng-annotate)
- description and source-code
```javascript
gulp-ng-annotate = function (options) {
  options = options || {};
  if (!options.remove) {
    options = merge({add: true}, options);
  };

  return through.obj(function (file, enc, done) {
    // When null just pass through.
    if (file.isNull()) {
      this.push(file);
      return done();
    }

    var opts = merge({map: !!file.sourceMap}, options);
    if (opts.map) {
      if (typeof opts.map === "boolean") {
        opts.map = {};
      }
      if (file.path) {
        opts.map.inFile = file.relative;
      }
    }

    // Buffer input.
    if (file.isBuffer()) {
      try {
        file.contents = transform(file, file.contents, opts);
      } catch (e) {
        this.emit("error", e);
        return done();
      }
    // Dealing with stream input.
    } else {
      file.contents = file.contents.pipe(new BufferStreams(function(err, buf, cb) {
        if (err) return cb(new gutil.PluginError(PLUGIN_NAME, err));
        try {
          var transformed = transform(file, buf, opts)
        } catch (e) {
          return cb(e);
        }
        cb(null, transformed);
      }));
    }

    this.push(file);
    done();
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.gulp-ng-annotate.toString"></a>[function <span class="apidocSignatureSpan">gulp-ng-annotate.</span>toString ()](#apidoc.element.gulp-ng-annotate.toString)
- description and source-code
```javascript
toString = function () {
    return toString;
}
```
- example usage
```shell
...
var merge = require("merge");
var BufferStreams = require("bufferstreams");

var PLUGIN_NAME = "gulp-ng-annotate";

// Function which handle logic for both stream and buffer modes.
function transform(file, input, opts) {
var res = ngAnnotate(input.toString(), opts);
if (res.errors) {
  var filename = "";
  if (file.path) {
    filename = file.relative + ": ";
  }
  throw new gutil.PluginError(PLUGIN_NAME, filename + res.errors.join("\n"));
}
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
