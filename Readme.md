
# Commander.js

  The complete solution for [node.js](http://nodejs.org) command-line interfaces, inspired by Ruby's [commander](https://github.com/visionmedia/commander).

## Installation

    $ npm install commander

## Option parsing

 Options with commander are defined with the `.option()` method, also serving as documentation for the options. The example below parses args and options from `process.argv`, leaving remaining args as the `program.args` array which were not consumed by options.

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

var program = require('../');

program
  .version('0.0.1')
  .option('-p, --peppers', 'Add peppers')
  .option('-P, --pineapple', 'Add pineapple')
  .option('-b, --bbq', 'Add bbq sauce')
  .option('-c, --cheese <type>', 'Add the specified type of cheese [marble]')
  .parse(process.argv);

console.log('you ordered a pizza with:');
if (program.peppers) console.log('  - peppers');
if (program.pineapple) console.log('  - pineappe');
if (program.bbq) console.log('  - bbq');
console.log('  - %s cheese', program.cheese || 'marble');
```

 Short flags may be passed as a single arg, for example `-abc` is equivalent to `-a -b -c`. Multi-word options such as "--template-engine" are camel-cased, becoming `program.templateEngine` etc.

## Automated --help

 The help information is auto-generated based on the information commander already knows about your program, so the following `--help` info is for free:

```  
 $ ./examples/pizza --help

   Usage: pizza [options]

   Options:

     -v, --version        output the version number
     -p, --peppers        Add peppers
     -P, --pineapple      Add pineappe
     -b, --bbq            Add bbq sauce
     -c, --cheese <type>  Add the specified type of cheese [marble]
     -h, --help           output usage information

```

## Coercion

```js
function range(val) {
  return val.split('..').map(Number);
}

function list(val) {
  return val.split(',');
}

program
  .version('0.0.1')
  .option('-i, --integer <n>', 'An integer argument', parseInt)
  .option('-f, --float <n>', 'A float argument', parseFloat)
  .option('-r, --range <a>..<b>', 'A range', range)
  .option('-l, --list <items>', 'A list', list)
  .option('-o, --optional [value]', 'An optional value')
  .parse(process.argv);

console.log(' int: %j', program.integer);
console.log(' float: %j', program.float);
console.log(' optional: %j', program.optional);
program.range = program.range || [];
console.log(' range: %j..%j', program.range[0], program.range[1]);
console.log(' list: %j', program.list);
console.log(' args: %j', program.args);
```

## .prompt(msg, fn)

 Single-line prompt:

```js
program.prompt('name: ', function(name){
  console.log('hi %s', name);
});
```

 Multi-line prompt:

```js
program.prompt('description:', function(name){
  console.log('hi %s', name);
});
```

 Coercion:

```js
program.prompt('Age: ', Number, function(age){
  console.log('age: %j', age);
});
```

```js
program.prompt('Birthdate: ', Date, function(date){
  console.log('date: %s', date);
});
```

## .password(msg[, mask], fn)

Prompt for password without echoing:

```js
program.password('Password: ', function(pass){
  console.log('got "%s"', pass);
  process.stdin.destroy();
});
```

Prompt for password with mask char "*":

```js
program.password('Password: ', '*', function(pass){
  console.log('got "%s"', pass);
  process.stdin.destroy();
});
```

## .confirm(msg, fn)

 Confirm with the given `msg`:

```js
program.confirm('continue? ', function(ok){
  console.log(' got %j', ok);
});
```

## .choose(list, fn)

 Let the user choose from a `list`:

```js
var list = ['tobi', 'loki', 'jane', 'manny', 'luna'];

console.log('Choose the coolest pet:');
program.choose(list, function(i){
  console.log('you chose %d "%s"', i, list[i]);
});
```

## Links

 - [terminal tables](https://github.com/LearnBoost/cli-table)
 - [progress bars](https://github.com/visionmedia/node-progress)
 - [more progress bars](https://github.com/substack/node-multimeter)

## License 

(The MIT License)

Copyright (c) 2011 TJ Holowaychuk &lt;tj@vision-media.ca&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.