## ipfs-infura

<br />

A super tiny module for querying an IPFS node Infura, that works in the browser and in Node. Only **2.76 kB** compressed!

This module was inspired by [`browser-ipfs`](https://github.com/pelle/browser-ipfs).

## Install

```
npm install --save ipfs-infura
```

## Usage

```js
const IPFS = require('ipfs-infura');
const ipfs = new IPFS({
  host: 'ipfs.infura.io', 
  port: 5001, 
  protocol: 'https', 
  projectId: '', 
  projectSecret: '' 
});

ipfs.add('hello world!').then(console.log).catch(console.log);

// result null 'QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j'

ipfs.cat('QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j', (err, result) => {
  console.log(err, result);
});

// result null 'hello world!'

ipfs.addJSON({ somevalue: 2, name: 'Nick' }, (err, result) => {
  console.log(err, result);
});

// result null 'QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j'

ipfs.catJSON('QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j').then(console.log).catch(console.log);

// result null { somevalue: 2, name: 'Nick' }
```

## About

A very simple module for querying an IPFS node. This module works for both `nodejs` and in the browser. It's extremly light, `<3 kB` when compressed.

This module uses the `js-ipfs-api` module for the adding operations on nodejs. However, in the browser, it uses a very light `FormData` `Blob` handling proceedure which was designed by Pelle Braendgaard, in his `browser-ipfs` module.

## Examples

An example of the module in use for the browser, can be found in [./example](./examples).

Inside is a single, no configuration required, HTML file using the `ipfs-infura` module.

## Browser Usage

`ipfs-infura` is completely browserifiable and webpack ready. The main export found in our distributions [dist](./dist) folder is `IPFS`. There you will find two builds of `ipfs-infura`, one compressed and minified `ipfs-infura.min.js` and one uncompressed `ipfs-infura.js`.

```
<html>
  <body>
    <script type="text/javascript" src="ipfs-infura.min.js">
    <script type="text/javascript">
      var ipfs = new IPFS({ provider: 'ipfs.infura.io', protocol: 'https' });

      // ...
    </script>
  </body>
</html>
```

## Webpack Figures

**2.76 kB** compressed (not gzipped)

```
Hash: 55d261679ea2edac14af                                                         
Version: webpack 2.1.0-beta.15
Time: 612ms
        Asset     Size  Chunks             Chunk Names
    ipfs-infura.js  9.55 kB       0  [emitted]  main
ipfs-infura.js.map    11 kB       0  [emitted]  main
    + 3 hidden modules

Hash: 20584eb8548f596cd97d                                                         
Version: webpack 2.1.0-beta.15
Time: 737ms
        Asset     Size  Chunks             Chunk Names
ipfs-infura.min.js  2.76 kB       0  [emitted]  main
    + 3 hidden modules
```

## API Design

### constructor

[index.js:ipfs-infura](../../../blob/master/src/index.js "Source code on GitHub")

Intakes a single provider object, outputs an `ipfs` instance.

**Parameters**

-   `provider` **Object** a single provider object, see `setProvider` for more details

Result output `ipfs` **Object** instance.

```js
const IPFS = require('ipfs-infura');
const ipfs = new IPFS({ host: 'ipfs.infura.io', port: 5001, protocol: 'https' });

ipfs.cat('QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j', (err, result) => {
  console.log(err, result);
});
```

### setProvider

[index.js:ipfs-infura](../../../blob/master/src/index.js "Source code on GitHub")

Sets the IPFS instance provider.

**Parameters**

-   `provider` **Object** a single provider object.

    default: `{ host: 'localhost', port: 5001, protocol: 'http', base: '/api/v0' }`

No result output.

```js
const IPFS = require('ipfs-infura');
const ipfs = new IPFS();

ipfs.setProvider({ host: 'ipfs.infura.io', port: 5001, protocol: 'https' });

ipfs.cat('QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j', cb);
```

### add

[index.js:ipfs-infura](../../../blob/master/src/index.js "Source code on GitHub")

Queries `/add` and adds a single String or Buffer data to IPFS, returns an IPFS hash.

**Parameters**

-   `input` **String|Buffer** the input data to be added to IPFS.

Result output `ipfsHash` **String**.

```js
const IPFS = require('ipfs-infura');
const ipfs = new IPFS({
  host: 'ipfs.infura.io', 
  port: 5001, 
  protocol: 'https', 
  projectId: '', 
  projectSecret: '' 
});

ipfs.add('hello world!', (err, result) => {
  console.log(err, result);
});

// result null 'QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j'
```

### addJSON

[index.js:ipfs-infura](../../../blob/master/src/index.js "Source code on GitHub")

Queries `/add` and adds stringified JSON to IPFS, returns a single ipfs hash.

**Parameters**

-   `input` **Object** the input data to be added to IPFS.

Result output `ipfsHash` **String**.

```js
const IPFS = require('ipfs-infura');
const ipfs = new IPFS({ host: 'ipfs.infura.io', port: 5001, protocol: 'https' });

ipfs.addJSON({ somevalue: 2, name: 'Nick' }, (err, result) => {
  console.log(err, result);
});

// result null 'QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j'
```

### cat

[index.js:ipfs-infura](../../../blob/master/src/index.js "Source code on GitHub")

Queries a `/cat` request, returns data as a String.

**Parameters**

-   `ipfsHash` **String** the ipfs hash string.

Result output `data` **String**.

```js
const IPFS = require('ipfs-infura');
const ipfs = new IPFS({
  host: 'ipfs.infura.io', 
  port: 5001, 
  protocol: 'https', 
  projectId: '', 
  projectSecret: '' 
});

ipfs.cat('QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j', (err, result) => {
  console.log(err, result);
});

// result null 'Hello world!'
```

### catJSON

[index.js:ipfs-infura](../../../blob/master/src/index.js "Source code on GitHub")

Queries a `/cat` request, returns data as a parsed JSON object.

**Parameters**

-   `ipfsHash` **String** the ipfs hash string.

Result output `data` **Object**.

```js
const IPFS = require('ipfs-infura');
const ipfs = new IPFS({
  host: 'ipfs.infura.io', 
  port: 5001, 
  protocol: 'https', 
  projectId: '', 
  projectSecret: '' 
});

ipfs.catJSON('QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j', (err, result) => {
  console.log(err, result);
});

// result null { somevalue: 2, name: 'Nick' ...}
```

### stat

[index.js:ipfs-infura](../../../blob/master/src/index.js "Source code on GitHub")

Queries a `/object/stat` request, returns data stats object.

**Parameters**

-   `ipfsHash` **String** the ipfs hash string.

Result output stats `data` **Object**.

```js
const IPFS = require('ipfs-infura');
const ipfs = new IPFS({
  host: 'ipfs.infura.io', 
  port: 5001, 
  protocol: 'https', 
  projectId: '', 
  projectSecret: '' 
});

ipfs.stat('QmTp2hEo8eXRp6wg7jXv1BLCMh5a4F3B7buAUZNZUu772j', (err, result) => {
  console.log(err, result);
});

/* result null {
  BlockSize: 14595
  CumulativeSize: 14595
  DataSize: 14592
  Hash: "QmbhrsdhbvQy3RyNiDdStgF4YRVc4arteS3wL5ES5M6cVd"
  LinksSize: 3
  NumLinks: 0
}
*/
```

## Contributing

Please help better the ecosystem by submitting issues and pull requests to default. We need all the help we can get to build the absolute best linting standards and utilities. We follow the AirBNB linting standard and the unix philosophy.

## Help out

There is always a lot of work to do, and will have many rules to maintain. So please help out in any way that you can:

- Create, enhance, and debug silentcicero rules (see our guide to ["Working on rules"](./github/CONTRIBUTING.md)).
- Improve documentation.
- Chime in on any open issue or pull request.
- Open new issues about your ideas for making `ipfs-infura` better, and pull requests to show us how your idea works.
- Add new tests to *absolutely anything*.
- Create or contribute to ecosystem tools, like modules for encoding or contracts.
- Spread the word.

Please consult our [Code of Conduct](CODE_OF_CONDUCT.md) docs before helping out.

We communicate via [issues](https://github.com/silentcicero/ipfs-infura/issues) and [pull requests](https://github.com/silentcicero/ipfs-infura/pulls).

## Important documents

- [Changelog](CHANGELOG.md)
- [Code of Conduct](CODE_OF_CONDUCT.md)
- [License](https://raw.githubusercontent.com/silentcicero/ipfs-infura/master/LICENSE)

## Licence

This project is licensed under the MIT license, Copyright (c) 2016 Nick Dodson. For more information see LICENSE.md.

```
The MIT License

Copyright (c) 2016 Nick Dodson. nickdodson.com

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
