# windicss

windicss is a css compiler or css interpreter, which is based on the grammar of tailwindcss and adds other features.

The original idea of this project was to replace the tailwindcss workflow of build css then add some purgecss and autoprefixer plugins.

## How it works

### Interpret Mode

Interpret Mode is similar to the traditional tailwindcss workflow, based on the input HTML text, and parse the classes in the HTML, then build our css file based on these classes.

* Input

```html
<div class="py-8 px-8 max-w-sm mx-auto bg-white rounded-xl shadow-md space-y-2 sm:py-4 sm:flex sm:items-center sm:space-y-0 sm:space-x-6">
  <img class="block mx-auto h-24 rounded-full sm:mx-0 sm:flex-shrink-0" src="/img/erin-lindford.jpg" alt="Woman's Face">
  <div class="text-center space-y-2 sm:text-left">
    <div class="space-y-0.5">
      <p class="text-lg text-black font-semibold">
        Erin Lindford
      </p>
      <p class="text-gray-500 font-medium">
        Product Engineer
      </p>
    </div>
    <button class="px-4 py-1 text-sm text-purple-600 font-semibold rounded-full border border-purple-200 hover:text-white hover:bg-purple-600 hover:border-transparent focus:outline-none focus:ring-2 focus:ring-purple-600 focus:ring-offset-2">Message</button>
  </div>
</div>
```

* Output

```css
/* preflight... */

.bg-white {
  --tw-bg-opacity: 1;
  background-color: rgba(255, 255, 255, var(--tw-bg-opacity));
}

.block {
  display: block;
}

/* ... */

@media (min-width: 640px) {
  .sm\:flex {
    display: -webkit-box;
    display: -ms-flexbox;
    display: -webkit-flex;
    display: flex;
  }
  .sm\:flex-shrink-0 {
    -ms-flex-negative: 0;
    -webkit-flex-shrink: 0;
    flex-shrink: 0;
  }
  /* ... */
}
```

### Compile Mode

The Compile mode synthesizes all the css attributes corresponding to the className in the class attribute, which brings us back to the traditional css writing method, and includes all the great features of tailwindcss. This mode is conducive to JavaScript frameworks based on template declarations like vuejs and sveltejs. All we need is a preprocessor.

* Input

```html
<div class="py-8 px-8 max-w-sm mx-auto bg-white rounded-xl shadow-md space-y-2 sm:py-4 sm:flex sm:items-center sm:space-y-0 sm:space-x-6">
  <img class="block mx-auto h-24 rounded-full sm:mx-0 sm:flex-shrink-0" src="/img/erin-lindford.jpg" alt="Woman's Face">
  <div class="text-center space-y-2 sm:text-left">
    <div class="space-y-0.5">
      <p class="text-lg text-black font-semibold">
        Erin Lindford
      </p>
      <p class="text-gray-500 font-medium">
        Product Engineer
      </p>
    </div>
    <button class="px-4 py-1 text-sm text-purple-600 font-semibold rounded-full border border-purple-200 hover:text-white hover:bg-purple-600 hover:border-transparent focus:outline-none focus:ring-2 focus:ring-purple-600 focus:ring-offset-2">Message</button>
  </div>
</div>
```

* Ouput HTML

```html
<div class="windi-15wa4me">
<img class="windi-1q7lotv" src="/img/erin-lindford.jpg" alt="Woman's Face">
<div class="windi-7831z4">
  <div class="windi-x3f008">
    <p class="windi-2lluw6">
      Erin Lindford
    </p>
    <p class="windi-1caa1b7">
      Product Engineer
    </p>
  </div>
  <button class="windi-d2pog2">Message</button>
</div>
</div>
```

* Output CSS

```css
/* preflight... */

.windi-15wa4me {
  --tw-bg-opacity: 1; /* bg-white */
  --tw-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06); /* shadow-md */
  -webkit-box-shadow: var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow); /* shadow-md */
  background-color: rgba(255, 255, 255, var(--tw-bg-opacity)); /* bg-white */
  border-radius: 0.75rem; /* rounded-xl */
  box-shadow: var(--tw-ring-offset-shadow, 0 0 #0000), var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow); /* shadow-md */
  margin-left: auto; /* mx-auto */
  margin-right: auto; /* mx-auto */
  max-width: 24rem; /* max-w-sm */
  padding-left: 2rem; /* px-8 */
  padding-right: 2rem; /* px-8 */
  padding-top: 2rem; /* py-8 */
  padding-bottom: 2rem; /* py-8 */
}

/* ... */

@media (min-width: 640px) {
  .windi-15wa4me {
    -ms-flex-align: center; /* items-center */
    -webkit-align-items: center; /* items-center */
    -webkit-box-align: center; /* items-center */
    align-items: center; /* items-center */
    display: -ms-flexbox; /* flex */
    display: -webkit-box; /* flex */
    display: -webkit-flex; /* flex */
    display: flex; /* flex */
    padding-top: 1rem; /* py-4 */
    padding-bottom: 1rem; /* py-4 */
  }
  
  /* ... */
}

```

## Features

* Cross browser support

    Each utility of windicss has cross-browser support, which means you don't need an autoprefixer plugin.

* Minify support

    You can simply output the minimized css.

* Preflight support

    You can generate preflights based on html tags.

* Zero dependencies

    Make it easy to use and run fast.

* Only build what you needed

    So no need to add purgecss plugin at all.

* Unrestricted build

    1. Number

        ```js
        p-${float[0,...infinite]} -> padding: (${float/4})rem
        
        p-2.5 -> padding: 0.625rem;

        p-3.2 -> padding: 0.8rem;
        ```

    2. Size

        ${size} should end up with rem|em|px|vh|vw

        ```js
        p-${size} -> padding: ${size}
        
        p-3px -> padding: 3px;

        p-4rem -> padding: 4rem;
        ```

    3. Fraction

        ```js
        w-${fraction} -> width: ${fraction -> precent}

        w-9/12 -> width: 75%;

    4. Color

        ```js
        bg-${color} -> background-color: rgba(...)

        bg-gray-300 -> background-color: rgba(209, 213, 219, var(--tw-bg-opacity);
        
        bg-hex-${hex} -> background-color: rgba(...)

        bg-hex-1c1c1e -> background-color: rgba(28, 28, 30, var(--tw-bg-opacity));
        ```


* new variants

    1. screens

        ```css
        sm: @media (min-width:640px);
        +sm: @media (min-width:640px) and (max-width:768px);
        -sm: @media (max-width:640px);
        ...
        ```

    2. themes

        ```css
        dark: .dark
        @dark: @media (prefers-color-scheme:dark);
        light: .light
        @light: @media (prefers-color-scheme:light);
        ```

    3. states

        Support all css pseudo elements and pseudo classes.

## Quick start

Go check `example/*`

## Applications && Plugins

1. Interact with JavaScript Frameworks.

    vue: https://github.com/vuejs/vue-loader

    svelte: https://github.com/sveltejs/svelte-preprocess

    react and angular: maybe some jsx or webpack plugin, not sure about this.

2. Transform tailwindcss to normal css file.

## Future work

Contribution will be very helpful.

* Push to npm.
* CLI support.
* Add some tests.
* Write documentations.
* Add tailwind.config.js support.
* svelte && vue preprocess plugin.
* Group support (eg. sm:hover:(bg-black-300 dark:text-gray-200)).
* Function support (eg. prop(font-size, 1em), bg-hsla(...), bg-raw(#fff) ...).
* Add new utility && variant && variable support
* Online playground (maybe fork from https://github.com/tailwindlabs/playtailwindcss.com).

## Special thanks

Learned a lot from [this project](https://github.com/ben-rogerson/twin.macro), if you want to use css-in-js with tailwindcss, you can check this project.