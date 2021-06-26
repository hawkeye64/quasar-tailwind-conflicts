# Quasar / TailwindUI conflicts

How Tailwind UI dropdown component looks on Quasar:

![100039916-3c0a6c00-2e41-11eb-9ea7-2005979180b2](https://user-images.githubusercontent.com/27774317/123427820-6124b000-d5c5-11eb-908b-1323b4c6a8fb.png)


How it looks on a Vue CLI app:

![100039908-357bf480-2e41-11eb-97b7-515d4fbdf39e](https://user-images.githubusercontent.com/27774317/123427835-671a9100-d5c5-11eb-87cb-2418bfa2e15d.png)


I've made some changes in the `.postcssrc.js` file to remove most TailwindCSS conflicts (based on [this comment from the original issue](https://github.com/quasarframework/quasar/issues/6775#issuecomment-675255237) but already tried [this one too](https://github.com/quasarframework/quasar/issues/6775#issuecomment-661891743) )
```js
// https://github.com/michael-ciniawsky/postcss-load-config

// These are the conflicting classes between Quasar and Tailwind. They may change in the future.
const conflictingClasses = [
  // flex must be treated separately
  "order-first", "order-last", "cursor-not-alowed",
  "cursor-pointer", "block", "inline-block", "text-justify",
  "hidden", "invisible", "overflow-auto", "overflow-hidden"
];
// The plugin takes an object where the keys are the selectors and the values are the properties (or list of properties) to remove or all properties with "*".
const removeObj = {
  ...Object.fromEntries(conflictingClasses.map(cc => [`.${cc}`, "*"])), // Removes all properties from conflicting classes
  ".row, .column, .flex": "flex-wrap" // Turns out rules defining multiple classes must be targetted as a whole.
};

module.exports = {
  plugins: [
    // to edit target browsers: use "browserslist" field in package.json
    require("postcss-remove-declaration")({ remove: removeObj }), // The plugin must be placed before Tailwind import!
    require("tailwindcss"),
    require("autoprefixer"),
  ]
}

```


## Install the dependencies
```bash
yarn install
```

## Comment the Quasar stylesheet file
In `node_modules/@quasar/app/templates/entry/client-entry.js`

```js
// We load Quasar stylesheet file
//import 'quasar/dist/quasar.<%= __css.quasarSrcExt %>'
```

### Start the app
```bash
quasar dev
```
