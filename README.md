<p align="center"><img src="https://raw.githubusercontent.com/totaljs-es/toto/main/assets/brand/favicon-192.png" width="128" height="128" alt="Toto, the fox of Spanish Total.js" /></p>

<h1 align="center">Total.js guidelines</h1>

<p align="center">
  <a href="https://www.totaljs.com"><img src="https://img.shields.io/badge/built%20with-Total.js%205-f26d21" alt="Built with Total.js 5" /></a>
</p>

__Topics__:

- [JavaScript Style](#)
	- [Filenames](#)
	- [Common](#)
	- [Examples](#)
- [Total.js](#)
- [Using `require()`](#)
- [CSS / Styles](#)
- [UI components / jComponent](#)
- [Total.js](#)
- [SQL scripts in Total.js](#)
- [Folders/Files structure](#)

## JavaScript Style

### Filenames

- __filenames should be all lowercase__
- use `-` dashes instead of `_` underscores for filenames

### Common

- use `tabs` (tab width `4`) instead of `spaces`
- remove all unnecessary white-spaces
- always use `;` semicolon at the end of the command
- don't use `const` constant in the method scopes
- short strings should be always wrapped in `'` apostrophes
- avoid adding dependencies as much as possible
- learn from our existing code
- use classic `loops` over `forEach()` where possible

### Examples

__Basics__:

```javascript
// BAD:
var num=123;

// GOOD:
var num = 123;

// BAD:
var str = "Total.js";

// GOOD:
var str = 'Total.js';
```

__Semicolons:__

```javascript
// BAD:
someobj.somefn = function() {
}

// or

var a
var b
var c

// GOOD:
someobj.somefn = function() {
};

var a;
var b;
var c;
```

__Declaration:__

```javascript
// BAD:
someobj.somefn = function() {
	const age = 30;
};

// GOOD:
someobj.somefn = function() {
	var age = 30;
	// or
	// let age = 30;
};
````

__Conditions:__

```javascript
// BAD:
if(true) {doSomething();}

// or
if(true) doSomething();

// or
if (true) {
	doSomething();
}

// GOOD:
if (true)
	doSomething();

// or for a simple conditions you can use inline expression:
true && doSomething();
````

## Total.js

We recommend using similar variable/schema names across all projects. In most cases, our projects use these variable names:

- `id`
- `name`
- `value`
- `email`
- `items` or `arr` contains an object collection/array
- `item`
- `tmp`
- `dtcreated`
- `dtupdated`

__Schemas__:

- schema names must be in plural
- first char in uppercase
- always use `UID()` for generating identifiers

```js
// BAD:
NEWSCHEMA('user', function(schema) {

	schema.define('name', 'Capitalize2(40)', true);

});

// GOOD:
NEWSCHEMA('Users', function(schema) {

	schema.define('name', 'Capitalize2(40)', true);

});
```

## Using `require()`

- use `const` instead of `var`
- always use first upper-case letter in the variable name

__Example__:

```js
// BAD:
var path = require('path');

// GOOD:
const Path = require('path');
```

## CSS / Styles

- __filenames should be all lowercase__
- for filenames use `-` dashes instead of `_` underscores for filenames
- __keep styles__ in the same line
- don't use vendor prefixes, but use `/*auto*/` comment for Total.js auto-prefixing
- instead of quotes use `'` apostrophes
- don't use LESS or SASS because Total.js supports variables and nested selector too

```css
/*auto*/

.class { key1: value; key2: value; key3: value; }
.class > div { key1: value; key2: value; }
```

## UI components / jComponent

- keep all names in Plugins/Components/FUNC./MAIN./REPO. in __lowercase__
- keep all variables in __lowercase__
- do not create complicated objects with a lot of objects in depth (keep max. 2-3 of keys in depth)
- if you can, do not use `_` underscore for variables
- keep same variable/plugin names across all projects
- __think reusable__

```javascript
// =======================================
// PLUGINS
// =======================================

// BAD:
PLUGIN('Name', function(plugin) {

	plugin.doSomething = function() {
	};

});

// GOOD:
PLUGIN('name', function(exports) {

	// keep "exports." name and keep all names in lowercase
	exports.dosomething = function() {
	};

});

// =======================================
// COMPONENTS
// =======================================

// BAD:
COMPONENT('Name', function(com, settings) {

});

// GOOD:
COMPONENT('name', function(self, config, cls) {
	// keep names: "self.", "config" and "cls"
});
````

## SQL scripts in Total.js

- __table names / field names should be all lowercase__
- use `tabs` (tab width `4`) instead of `spaces`
- remove all unnecessary white-spaces
- always use `;` semicolon at the end of a SQL command
- remove all unnecessary type-casting `name<>'something'::text` to `name<>'something'`
- format SQL scripts
- divide scripts TABLES, VIEWS, STORED PROCEDURES/FUNCTIONS, INDEXES
- table names starts with `tbl_` in singular --> `tbl_user`, `tbl_product`
- tables with codelists must start with `cl_` in singular --> `cl_type`, `cl_product`
- view names starts with `view_` in singular --> `view_user`
- stored procedures starts with `sp_` in singular --> `sp_user`, etc..
- functions starts with `fn_` in singular --> `fn_user`, etc..

__Fields creating__:

- dates starts with `dt` --> `dtcreated`, `dtupdated`, `dtpaid`, etc..
- booleans must starts with `is` --> `ispaid`, `isremoved`, `ispublished`, etc.. with few exceptions
- identifiers `id` (primary key), `userid` (foreign key), `productid` (foreign key), etc..
- use `text` data types instead of `varchar` ([more info](https://stackoverflow.com/questions/4848964/difference-between-text-and-varchar-character-varying))
- keep same names in different tables for example: `name`, `body`, etc..
- keep short names
- if the situation allows it, use always these columns `id`, `name` and `dtcreated`
- keep the sort of fields below:

```sql
-- BAD:
CREATE TABLE "public"."tbl_channel_message" (
	"channelid" varchar(25),
	"body" text,
	"id" varchar(25) NOT NULL,
	"ispinned" bool DEFAULT FALSE,
	"isrobot" bool DEFAULT FALSE,
	"userid" varchar(25),
	"countupdate" INT2 DEFAULT 0,
	"dtupdated" timestamp,
	"openplatformid" varchar(30),
	"dtcreated" timestamp DEFAULT timezone('utc'::text, now()),
	"ismobile" bool DEFAULT FALSE,
	"isremoved" bool DEFAULT FALSE,
	PRIMARY KEY ("id")
);

-- GOOD:
CREATE TABLE "public"."tbl_channel_message" (

	-- IDENTIFIERS FIRST
	"id" text NOT NULL,
	"userid" text,
	"channelid" text,
	"openplatformid" text,

	-- MAIN FIELDS
	"body" text,

	-- NUMBERS
	"countupdate" INT2 DEFAULT 0,

	-- BOOLEANS
	"ismobile" bool DEFAULT FALSE,
	"ispinned" bool DEFAULT FALSE,
	"isremoved" bool DEFAULT FALSE,
	"isrobot" bool DEFAULT FALSE,

	-- AND LAST DATES
	"dtupdated" timestamp,
	"dtcreated" timestamp DEFAULT timezone('utc'::text, now()),

	-- DO NOT FORGET FOR FOREIGN KEYS
	CONSTRAINT ...
	CONSTRAINT ...

	PRIMARY KEY ("id")
);
```

## Folders/Files structure

Keep the file structure according to the file types:

- `/img/` for images
- `/js/` for scripts
- `/css/` for styles
- `/fonts/` for fonts
- `/videos/` for videos

---

Use `-` dashes for spaces instead of underscores `_` for the filenames.
