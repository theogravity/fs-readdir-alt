## [![npm][npmjs-img]][npmjs-url]

> fs.readdir done right! Support sync, async and stream API, recursiveness and filters.

## Fork notice

This is forked off https://github.com/tunnckoCore/fs-readdir.

Differences:

- Option to disable directory recursion
- Option to stop operation asap on first error encountered
- Option to only include files or directories in results
- Option to only use the emitter (instead of using the callback) to send back results, resulting in memory savings

## Install, Test & Benchmark
```
npm i --save fs-readdir-alt
node benchmark
```


## Usage
> For more use-cases see the [tests](./test.js)

```js
var fsReaddir = require('fs-readdir-alt');
var through2 = require('through2');

// callback api
fsReaddir('../gitclone-cli', function _cb(err, filepaths) {
  // as usual
  console.log('callback err:', err)
  console.log('callback res:', filepaths)
});

// callback api with opts

fsReaddir('../gitclone-cli', {
  // recurse through directories
  recurse: true,
  // check for files?
  includeFiles: true,
  // emit files?
  emitFiles: true,
  // emit paths that are directories?
  emitDirs: true,
  // only emit results instead of returning them in callback (saves memory as results are not pushed to an internal array)
  emitOnly: false,
  // stops new operations if an error is encountered
  stopOnError: false
}, function _cb(err, filepaths) {
  // as usual
  console.log('callback err:', err)
  console.log('callback res:', filepaths)
});


// as stream
var stream = fsReaddir('../gitclone-cli')
.on('error', function(err) {
  console.log('error:', err);
})
.on('finish', function(obj) {
  console.log('finish:', obj);
})
.on('data', function(obj) {
  console.log('data:', obj);
})
.on('folder', function(folder) {
  console.log('folder:', folder);
})
.on('file', function(file) {
  console.log('file:', file);
})
.pipe(through2.obj(function(objArray, enc, next) {
  objArray = objArray.map(function(fp) {
    return path.basename(fp);
  })
  console.log('pipe1:', objArray);
  this.push(objArray)
  next();
})).pipe(through2.obj(function(modified, enc, next) {
  console.log('pipe2:', modified);
  this.push(modified)
  next();
}))
```

## Fork Author
**Theo Gravity**
+ [github/theogravity][author-github]
+ [npmjs/theo.gravity][author-npmjs]

## Orig Author
**Charlike Mike Reagent**
+ [gratipay/tunnckoCore][author-gratipay]
+ [twitter/tunnckoCore][author-twitter]
+ [github/tunnckoCore][author-github]
+ [npmjs/tunnckoCore][author-npmjs]
+ [more ...][contrib-more]

## License
Copyright (c) 2014-2015 [Charlike Mike Reagent][contrib-more], [contributors][contrib-graf].  
Released under the `MIT` license.
