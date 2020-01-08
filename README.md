# trying native Node ESModules

- run `yarn setup`
- then `yarn try`


What is happening is that we copy `./some-pkg` to the `node_modules/some-pkg`.
This `some-pkg` has `main`, `module` and `exports` fields. Intentionally don't set `type: module`
because this makes the meaning that the file pointed in `main` will be treated as ESM, which clearly
isn't the intention because such freaking behavior will bugs with the bundlers or the consumers of
a library to which want using bundlers.


So. Neither this root package.json has `type` field, nor the `node_modules/some-pkg`.


1. When you run `node src/index.js` it fails

```
$ node src/index.js
/home/charlike/dev/node-native-esm/src/index.js:1
import somepkg from 'some-pkg'
^^^^^^

SyntaxError: Cannot use import statement outside a module
    at Module._compile (internal/modules/cjs/loader.js:895:18)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:995:10)
    at Module.load (internal/modules/cjs/loader.js:815:32)
    at Function.Module._load (internal/modules/cjs/loader.js:727:14)
    at Function.Module.runMain (internal/modules/cjs/loader.js:1047:10)
    at internal/main/run_main_module.js:17:11
```

2. When you rename the file to `index.mjs` it fails with another error

```
❯ node src/index.mjs
internal/modules/cjs/loader.js:1029
  throw new ERR_REQUIRE_ESM(filename);
  ^

Error [ERR_REQUIRE_ESM]: Must use import to load ES Module: /home/charlike/dev/node-native-esm/src/index.mjs
    at Object.Module._extensions..mjs (internal/modules/cjs/loader.js:1029:9)
    at Module.load (internal/modules/cjs/loader.js:815:32)
    at Function.Module._load (internal/modules/cjs/loader.js:727:14)
    at Function.Module.runMain (internal/modules/cjs/loader.js:1047:10)
    at internal/main/run_main_module.js:17:11 {
  code: 'ERR_REQUIRE_ESM'
```

which isn't that clear what that error means... hell, i use imports everywhere?!
