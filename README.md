# api documentation for  [watch (v1.0.2)](https://github.com/mikeal/watch)  [![npm package](https://img.shields.io/npm/v/npmdoc-watch.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-watch) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-watch.svg)](https://travis-ci.org/npmdoc/node-npmdoc-watch)
#### Utilities for watching file trees.

[![NPM](https://nodei.co/npm/watch.png?downloads=true)](https://www.npmjs.com/package/watch)

[![apidoc](https://npmdoc.github.io/node-npmdoc-watch/build/screen-capture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-watch_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-watch/build..beta..travis-ci.org/apidoc.html)

![package-listing](https://npmdoc.github.io/node-npmdoc-watch/build/screen-capture.npmPackageListing.svg)



# package.json

```json

{
    "author": {
        "name": "Mikeal Rogers",
        "email": "mikeal.rogers@gmail.com"
    },
    "bin": {
        "watch": "./cli.js"
    },
    "bugs": {
        "url": "https://github.com/mikeal/watch/issues"
    },
    "dependencies": {
        "exec-sh": "^0.2.0",
        "minimist": "^1.2.0"
    },
    "description": "Utilities for watching file trees.",
    "devDependencies": {},
    "directories": {
        "lib": "lib"
    },
    "dist": {
        "shasum": "340a717bde765726fa0aa07d721e0147a551df0c",
        "tarball": "https://registry.npmjs.org/watch/-/watch-1.0.2.tgz"
    },
    "engines": {
        "node": ">=0.1.95"
    },
    "gitHead": "07e57379393271f33e3d30153fbaa4b790e6ff04",
    "homepage": "https://github.com/mikeal/watch",
    "keywords": [
        "util",
        "utility",
        "fs",
        "files"
    ],
    "license": "Apache-2.0",
    "main": "./main",
    "maintainers": [
        {
            "name": "finnpauls",
            "email": "derfinn@gmail.com"
        },
        {
            "name": "levithomason",
            "email": "me@levithomason.com"
        },
        {
            "name": "mikeal",
            "email": "mikeal.rogers@gmail.com"
        }
    ],
    "name": "watch",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/mikeal/watch.git"
    },
    "scripts": {
        "release:major": "bash scripts/release.sh major",
        "release:minor": "bash scripts/release.sh minor",
        "release:patch": "bash scripts/release.sh patch"
    },
    "version": "1.0.2"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module watch](#apidoc.module.watch)
1.  [function <span class="apidocSignatureSpan">watch.</span>createMonitor (root, options, cb)](#apidoc.element.watch.createMonitor)
1.  [function <span class="apidocSignatureSpan">watch.</span>unwatchTree (root)](#apidoc.element.watch.unwatchTree)
1.  [function <span class="apidocSignatureSpan">watch.</span>walk (dir, options, callback)](#apidoc.element.watch.walk)
1.  [function <span class="apidocSignatureSpan">watch.</span>watchTree ( root, options, callback )](#apidoc.element.watch.watchTree)



# <a name="apidoc.module.watch"></a>[module watch](#apidoc.module.watch)

#### <a name="apidoc.element.watch.createMonitor"></a>[function <span class="apidocSignatureSpan">watch.</span>createMonitor (root, options, cb)](#apidoc.element.watch.createMonitor)
- description and source-code
```javascript
createMonitor = function (root, options, cb) {
  if (!cb) {cb = options; options = {}}
  var monitor = new events.EventEmitter();
  monitor.stop = exports.unwatchTree.bind(null, root);

  var prevFile = {file: null,action: null,stat: null};
  exports.watchTree(root, options, function (f, curr, prev) {
    // if not curr, prev, but f is an object
    if (typeof f == "object" && prev == null && curr === null) {
      monitor.files = f;
      return cb(monitor);
    }

    // if not prev and either prevFile.file is not f or prevFile.action is not created
    if (!prev) {
      if (prevFile.file != f || prevFile.action != "created") {
        prevFile = { file: f, action: "created", stat: curr };
        return monitor.emit("created", f, curr);
      }
    }

    // if curr.nlink is 0 and either prevFile.file is not f or prevFile.action is not removed
    if (curr) {
      if (curr.nlink === 0) {
        if (prevFile.file != f || prevFile.action != "removed") {
          prevFile = { file: f, action: "removed", stat: curr };
          return monitor.emit("removed", f, curr);
        }
      }
    }

    // if prevFile.file is null or prevFile.stat.mtime is not the same as curr.mtime
    if (prevFile.file === null) {
      return monitor.emit("changed", f, curr, prev);
    }
    // stat might return null, so catch errors
    try {
      if (prevFile.stat.mtime.getTime() !== curr.mtime.getTime()) {
        return monitor.emit("changed", f, curr, prev);
      }
    } catch(e) {
      return monitor.emit("changed", f, curr, prev);
    }
  })
}
```
- example usage
```shell
...
  })
</pre>

### watch.unwatchTree(root)

Unwatch a previously watched directory root using 'watch.watchTree'.

### watch.createMonitor(root, [options,] callback)

This function creates an EventEmitter that gives notifications for different changes that happen to the file and directory tree
under the given root argument.

The options object is passed to watch.watchTree.

The callback receives the monitor object.
...
```

#### <a name="apidoc.element.watch.unwatchTree"></a>[function <span class="apidocSignatureSpan">watch.</span>unwatchTree (root)](#apidoc.element.watch.unwatchTree)
- description and source-code
```javascript
unwatchTree = function (root) {
  if (!watchedFiles[root]) return;
  Object.keys(watchedFiles[root]).forEach(fs.unwatchFile);
  watchedFiles[root] = false;
}
```
- example usage
```shell
...
      // f was removed
    } else {
      // f was changed
    }
  })
</pre>

### watch.unwatchTree(root)

Unwatch a previously watched directory root using 'watch.watchTree'.

### watch.createMonitor(root, [options,] callback)

This function creates an EventEmitter that gives notifications for different changes that happen to the file and directory tree
under the given root argument.
...
```

#### <a name="apidoc.element.watch.walk"></a>[function <span class="apidocSignatureSpan">watch.</span>walk (dir, options, callback)](#apidoc.element.watch.walk)
- description and source-code
```javascript
function walk(dir, options, callback) {
  if (!callback) {callback = options; options = {}}
  if (!callback.files) callback.files = {};
  if (!callback.pending) callback.pending = 0;
  callback.pending += 1;
  fs.stat(dir, function (err, stat) {
    if (err) return callback(err);
    callback.files[dir] = stat;
    fs.readdir(dir, function (err, files) {
      if (err) {
        if(err.code === 'EACCES' && options.ignoreUnreadableDir) return callback();
        return callback(err);
      }
      callback.pending -= 1;
      files.forEach(function (f, index) {
        f = path.join(dir, f);
        callback.pending += 1;
        fs.stat(f, function (err, stat) {
          var enoent = false
            , done = false;

          if (err) {
            if (err.code !== 'ENOENT' && (err.code !== 'EPERM' && options.ignoreNotPermitted)) {
              return callback(err);
            } else {
              enoent = true;
            }
          }
          callback.pending -= 1;
          done = callback.pending === 0;
          if (!enoent) {
            if (options.ignoreDotFiles && path.basename(f)[0] === '.') return done && callback(null, callback.files);
            if (options.filter && !options.filter(f, stat)) return done && callback(null, callback.files);
            callback.files[f] = stat;
            if (stat.isDirectory() && !(options.ignoreDirectoryPattern && options.ignoreDirectoryPattern.test(f))) walk(f, options
, callback);
            done = callback.pending === 0;
            if (done) callback(null, callback.files);
          }
        })
      })
      if (callback.pending === 0) callback(null, callback.files);
    })
    if (callback.pending === 0) callback(null, callback.files);
  })

}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.watch.watchTree"></a>[function <span class="apidocSignatureSpan">watch.</span>watchTree ( root, options, callback )](#apidoc.element.watch.watchTree)
- description and source-code
```javascript
watchTree = function ( root, options, callback ) {
  if (!callback) {callback = options; options = {}}
  walk(root, options, function (err, files) {
    if (err) throw err;
    var fileWatcher = function (f) {
      var fsOptions = {};
      if (options.interval) {
        fsOptions.interval = options.interval * 1000;
      }
      fs.watchFile(f, fsOptions, function (c, p) {
        // Check if anything actually changed in stat
        if (files[f] && !files[f].isDirectory() && c.nlink !== 0 && files[f].mtime.getTime() == c.mtime.getTime()) return;
        files[f] = c;
        if (!files[f].isDirectory()) callback(f, c, p);
        else {
          fs.readdir(f, function (err, nfiles) {
            if (err) return;
            nfiles.forEach(function (b) {
              var file = path.join(f, b);
              if (!files[file] && (options.ignoreDotFiles !== true || b[0] != '.')) {
                fs.stat(file, function (err, stat) {
                  if (options.filter && !options.filter(file, stat)) return;
                  callback(file, stat, null);
                  files[file] = stat;
                  fileWatcher(file);
                })
              }
            })
          })
        }
        if (c.nlink === 0) {
          // unwatch removed files.
          delete files[f]
          fs.unwatchFile(f);
        }
      })
    }
    fileWatcher(root);
    for (var i in files) {
      fileWatcher(i);
    }
    watchedFiles[root] = files;
    callback(files, null, null);
  })
}
```
- example usage
```shell
...
  npm install watch
</pre>

## Purpose

The intention of this module is provide tools that make managing the watching of file & directory trees easier.

#### watch.watchTree(root, [options,] callback)

The first argument is the directory root you want to watch.

The options object is passed to fs.watchFile but can also be used to provide two additional watchTree specific options:

* ''ignoreDotFiles'' - When true this option means that when the file tree is walked it will ignore files that being with "."
* ''filter'' - You can use this option to provide a function that returns true or false for each file and directory to decide whether
 or not that file/directory is included in the watcher.
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
