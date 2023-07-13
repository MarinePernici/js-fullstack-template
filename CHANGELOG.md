# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Installed `@faker-js/faker` in backend.

- Made the backend more verbose with additional logs and more comments.

### Changed

- Exposed header `Location` in CORS configuration in backend.

- Moved to async/await syntax in `backend/src/controllers/itemControllers.js`, and moved error handling into an error middleware in `app.js`.

- **Breaking change:** refactored models. Managers like `backend/src/models/ItemManager.js` should declare every CRUD methods: they do not inherit read and delete methods from `AbstractManager` anymore. Methods `find`, `findAll` and `insert` are renamed as `read`, `readAll` and `create`. Moved to async/await syntax.

- **Breaking change:** manager registration should be done in `backend/src/tables.js` instead of `backend/src/models/index.js`.

For example, a `FooManager.js` model was previously registered in `backend/src/models/index.js` like this:

```js
const models = {};

const FooManager = require("./FooManager");

models.foo = new FooManager();
models.foo.setDatabase(pool);
```

Now it should be registered in `backend/src/tables.js` like this:

```js
const tables = {};

const FooManager = require("./models/FooManager");

tables.foo = new FooManager();
```

(note you don't need the `setDatabase(pool)` line anymore)

Usage in controllers changes from this:

```js
const models = require("../models");

// ...

models.foo.callSomeCrudMethod();
```

To this:

```js
const tables = require("../tables");

// ...

tables.foo.callSomeCrudMethod();
```

- **Breaking change:** split ̀`database.sql` logic into table creation in a file `backend/database/schema.sql` and table filling in a file `backend/seed.js`. Updated `backend/migrate.js` accordingly.

- **Breaking change:** renamed `migrate` script as `db:migrate`, and added a `db:seed` script.

## [3.0.0] - 2023-05-29

### Added

- Git commands for Windows users, to fix issues with different newline formats (see [README.md](README.md#windows-users)).

### Changed

- Changed default ports configuration to 3000 for frontend and 6000 for backend. Thanks to [Loris Chastanet](https://github.com/lchastanet).

- **Breaking change:** removed cutomized alias for imports in frontend.

### Fixed

- Moved `vite` `and `@`vitejs/plugin-react` as regular dependencies in frontend, and fixed imports in config. Thanks to [Pierre Paillard](https://github.com/PPaillard/).

[Open an issue](https://github.com/WildCodeSchool/js-template-fullstack/issues/new) if you have any request/feedback :)

## [2.0.1] - 2023-03-24

### Changed

- Removed useless code in `package.json` files.

## [2.0.0] - 2023-02-10

### Added

- Deployment workflows using CapRover. Thanks to [Anthony Gorski](https://github.com/GorskiAnthony).
- Compatibility with `npm` alternatives (`yarn`, `pnpm`...). Set `config.cli` in root `package.json` with the wanted value.

### Changed

- Allowed usage `console.info` in ESLint configuration (front and back).
- Bumped dependencies versions. Thanks to [Valentin Dupin](https://github.com/ydainna).
- Cleaned backend/src/app.js and removed public index.html file to avoid conflicts when serving react build.

- **Breaking change:** removed setup script: `npm install` (or any other alternative) triggers a `postinstall` script.

- **Breaking change:** removed models "autoloading": now managers should be instantiated manually in `backend/src/models/index.js`.

For example, given you created a `FooManager.js` file to be associated with a `foo` table,
you should add to index, after `const models = {}` statement:

```js
const FooManager = require("./FooManager");

models.foo = new FooManager();
models.foo.setDatabase(pool);
```

- **Breaking change:** renamed `connection` property of managers as `database` to be consistent with quests.

Managers methods should be fixed from:

```js
findAll() {
  return this.connection.query(`select * from  ${this.table}`);
}
```

To:

```js
findAll() {
  return this.database.query(`select * from  ${this.table}`);
}
```
