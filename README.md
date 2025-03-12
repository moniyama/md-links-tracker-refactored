# Markdown Links

## Índice

* [1. Project](#1-project)
* [2. Objectives and Criterias](#2-objectives-and-criterias)
* [3 Usage and Instalattion](#3-usage-and-instalattion)

***

## 1. Project

Library which reads directories or markdown files (.md extension files). 
It identify any links in archives and its texts. It can also check if they are valid and show some statics.

- Updated and improved version of old project [md-links-tracker](https://github.com/moniyama/md-links-tracker).

You can use to read directly files and directories. 
It does not read directories inside directories.

## 2. Objectives and Criterias

* use of then/catch and promises to reinforcement skills
* unit tests
* use of CommonJS Modules

### Improvements to be done

#### Majors
* Add Recursion: Read directories inside directories
* CLI usability: Choose to not use libraries to color CLI output responses, as the priority was the practice of logic implementation

#### Minors
* Path error: Path needs to end in `/` otherwise returns an not friendly readable message
* Shortcuts CLI: add shortcuts eg as `--v` and help menu. 

## 3. Usage and instalattion

This module must include both an executable that we can invoke from the command line 
and an interface that we can import to use programmatically.

### 1) JavaScript API

In your project directory install the module with `npm install moniyama/md-links-tracker-refactored`

#### `mdLinks(path, options)`

##### Params

* `path`: Absolute or relative path of the file or directory
* `options`: Object with property:
  - `validate`: Boolean value. If you want to make a request to check whether the url is valid or not.

##### Return

It returns a Promise, in which its resolved is an array of objects. 
The propeties of the object depends on the value of the param in validate.

If `validate:false` :

* `href`: URL of the link found.
* `text`: Anchor text of URL.
* `file`: Path of the file.

If `validate:true` :

* `href`: URL of the link found.
* `text`: Anchor text of URL.
* `file`: Path of the file.
* `statusCode`: HTTP Response.
* `statusMessage`: HTTP Response Message.

#### Example os usage

```js
const mdLinks = require("md-links");

mdLinks("./some/example.md")
  .then(links => {
    // expected: [{ href, text, file }, ...]
  })
  .catch(console.error);

mdLinks("./some/example.md", { validate: true })
  .then(links => {
    // expected: [{ href, text, file, statusCode, statusMessage }, ...]
  })
  .catch(console.error);

mdLinks("./some/dir")
  .then(links => {
    // expected: [{ href, text, file }, ...]
  })
  .catch(console.error);
```

### 2) CLI

In your terminal install the module globally with `npm install moniyama/md-links-tracker-refactored -g`.

Now you can use it in terminal with de command:

`md-links <path-to-file> [options]`

```sh
$ md-links ./some/example.md
./some/example.md http://algo.com/2/3/ Link a algo
./some/example.md https://otra-cosa.net/algun-doc.html algún doc
./some/example.md http://google.com/ Google
```

#### Options

##### `--validate`

Besides the standard response, it also returns each HTTP Code and Message Responses.

```sh
$ md-links ./some/example.md --validate
./some/example.md http://algo.com/2/3/ Link a algo 200 OK
./some/example.md https://otra-cosa.net/algun-doc.html algún doc 404 NOT FOUND
./some/example.md http://google.com/ Google 200 OK
./some/example.md http://google.com/ Google 200 OK
```

##### `--stats`

It returns how many links were found and how many are unique.

```sh
$ md-links ./some/example.md --stats
Total: 4
Unique: 3
```

You can also use both options `--stats` and `--validate` together. Order does not matter.
It returns the same statistics but includes the quantity of broken links.

```sh
$ md-links ./some/example.md --stats --validate
Total: 4
Unique: 3
Broken: 1
```
