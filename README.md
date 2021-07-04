# Angular-starter-template-using-tailwindcss
# **Introduction: what is Tailwindcss ?**

> Tailwind is a utility-first CSS framework. ... Instead Tailwind CSS operates on a lower level and provides you with a set of CSS helper classes. By using this classes you can rapidly create custom design with ease. Tailwind CSS is not opinionated and let's you create you own unique design.

# Pre-requisites

Before proceeding to the guide, make sure you have the following installed in your operating system:

- Node.js v12.13.0 or higher (as part of Tailwindcss v2.0 requirements)
- Angular CLI v11

# **Create an Angular 11 project**

Using Angular CLI v11, create a new Angular 11 app with SCSS.

> ng new angular11-tailwindcss --style=scss
cd angular11-tailwindcss

## Install tailwindcss dependencies

First of all, install the latest tailwindcss, postcss, and autoprefixer packages as devDependencies:

> npm i -D tailwindcss@latest postcss@latest autoprefixer@latest

Then, install a specific major version of postcss-import, postcss-loader, and postcss-scss:

> npm i -D postcss-import@12 postcss-loader@4 postcss-scss@3

This command will install the ngx-build-plus as dependencies in package.json and automatically adjust the default angular.json to be like the following:

## Configure an additional webpack config

> After that, we’re going to install a 3rd party angular library via Angular Schematics called ngx-build-plus, which is for extending the default webpack configs without the need to eject the angular app:

> ng add ngx-build-plus

Make sure the installed ngx-build-plus version matches with the angular version. In our case, the ngx-build-plus version should be 11.x.x as we’re using Angular 11. If for some reasons the installed version does not match, use the specific version install command ng add ngx-build-plus@11.

```jsx
{
  ...
  "architect": {
    "build": {
      "builder": "ngx-build-plus:browser",
      ...
    },
    "serve": {
      "builder": "ngx-build-plus:dev-server",
      ...
    },
    "test": {
      "builder": "ngx-build-plus:karma",
      ...
    }
  }
}
```

Let’s now create a custom webpack config file called webpack.config.js and place it in the root folder with the following configurations:

```jsx
module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        loader: 'postcss-loader',
        options: {
          postcssOptions: {
            ident: 'postcss',
            syntax: 'postcss-scss',
            plugins: [
              require('postcss-import'),
              require('tailwindcss'),
              require('autoprefixer'),
            ],
          },
        },
      },
    ],
  },
};
```

> The above webpack config is mainly for setting up PostCSS. In this case, we’re using postcss-import, tailwindcss, and autoprefixer PostCSS plugins. To customise or learn more about other possible configurations, please visit [https://tailwindcss.com/docs/using-with-preprocessors](https://tailwindcss.com/docs/using-with-preprocessors).

### Now, we need to add an additional config to enable angular to read our additional webpack.config.js file. Copy the following new line to angular.json under "build", "serve", and "test" options :

```jsx
{
  ...
  "architect": {
    "build": {
      "builder": "ngx-build-plus:browser",
      "options": {
        "extraWebpackConfig": "webpack.config.js", // add this line
        ...
      },
      ...
    },
    "serve": {
      "builder": "ngx-build-plus:dev-server",
      "options": {
        "extraWebpackConfig": "webpack.config.js", // add this line
        ...
      },
      ...
    },
    "test": {
      "builder": "ngx-build-plus:karma",
      "options": {
        "extraWebpackConfig": "webpack.config.js", // add this line
        ...
      },
      ...
    },
  }
}
```

> Make sure the property name is called "extraWebpackConfig" and the value matches your additional webpack config file name, which in our case should be "webpack.config.js". Additionally, ensure the additional webpack config file is placed in the root folder.

### 

# Configure and add tailwindcss

To start using tailwind in the project, let’s first generate a tailwindcss config file in the root folder using the command:

npx tailwindcss init

This will generate a file called tailwind.config.js with the following generated configs:

```jsx
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

```jsx
/* You can add global styles to this file, and also import other style files */
@import 'tailwindcss/base';
@import 'tailwindcss/components';
@import 'tailwindcss/utilities';
```

## Test out tailwind utility classes

```html
<nav class="nav flex bg-black text-2 text-white dark:text-gray-200 flex-wrap items-center justify-between px-4 shadow-lg">
    <div class="flex flex-no-shrink items-center mr-6 py-3 text-grey-darkest">
        <span class="font-semibold text-xl tracking-tight">Nishikanta Ray</span>
    </div>

    <input class="menu-btn hidden" type="checkbox" id="menu-btn">
    <label class="menu-icon block cursor-pointer md:hidden px-2 py-4 relative select-none" for="menu-btn">
        <span class="navicon bg-grey-darkest flex items-center relative"></span>
    </label>

    <ul class="menu border-b md:border-none flex justify-end list-reset m-0 w-full md:w-auto">
        <li class="border-t md:border-none">
            <a routerLink="/home"
                class="block md:inline-block px-4 py-3 no-underline text-grey-darkest hover:text-grey-darker font-bold">Home</a>
        </li>

        <li class="border-t md:border-none">
            <a routerLink="/about"
                class="block md:inline-block px-4 py-3 no-underline text-grey-darkest hover:text-grey-darker">About</a>
        </li>

        <li class="border-t md:border-none">
            <a routerLink="/gallery"
                class="block md:inline-block px-4 py-3 no-underline text-grey-darkest hover:text-grey-darker">Gallery</a>
        </li>
        <li class="border-t md:border-none">
            <a routerLink="/price"
                class="block md:inline-block px-4 py-3 no-underline text-grey-darkest hover:text-grey-darker">Pricing</a>
        </li>
        <li class="border-t md:border-none pl-2">
            <button
            routerLink="/login" class="w-auto px-8 py-2 my-2 text-base font-medium text-white transition duration-500 ease-in-out transform bg-green-600 border-blue-600 rounded-md focus:shadow-outline focus:outline-none focus:ring-2 ring-offset-current ring-offset-2 hover:b-gblue-700 ">Login
            </button>
        </li>
        <li class="border-t md:border-none pl-2">
            <button
            routerLink="/signup" class="w-auto px-8 py-2 my-2 text-base font-medium text-white transition duration-500 ease-in-out transform bg-red-600 border-blue-600 rounded-md focus:shadow-outline focus:outline-none focus:ring-2 ring-offset-current ring-offset-2 hover:b-gblue-700 ">Signup
            </button>
        </li>
    </ul>
</nav>
<section class="text-gray-600 body-font">
    <div class="container mx-auto flex px-5 py-24 md:flex-row flex-col items-center">
      <div class="lg:flex-grow md:w-1/2 lg:pr-24 md:pr-16 flex flex-col md:items-start md:text-left mb-16 md:mb-0 items-center text-center">
        <h1 class="title-font sm:text-4xl text-3xl mb-4 font-medium text-gray-900">Before they sold out
          <br class="hidden lg:inline-block">readymade gluten
        </h1>
        <p class="mb-8 leading-relaxed">Copper mug try-hard pitchfork pour-over freegan heirloom neutra air plant cold-pressed tacos poke beard tote bag. Heirloom echo park mlkshk tote bag selvage hot chicken authentic tumeric truffaut hexagon try-hard chambray.</p>
        <div class="flex justify-center">
          <button class="inline-flex text-white bg-yellow-500 border-0 py-2 px-6 focus:outline-none hover:bg-yellow-600 rounded text-lg">Button</button>
          <button class="ml-4 inline-flex text-gray-700 bg-gray-100 border-0 py-2 px-6 focus:outline-none hover:bg-gray-200 rounded text-lg">Button</button>
        </div>
      </div>
      <div class="lg:max-w-lg lg:w-full md:w-1/2 w-5/6">
        <img class="object-cover object-center rounded" height="300px" alt="hero" src="https://images.unsplash.com/photo-1621189378573-fead8ffee049?ixid=MnwxMjA3fDB8MHx0b3BpYy1mZWVkfDd8NnNNVmpUTFNrZVF8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60">
      </div>
    </div>
</section>
<br><br>
<div class="shadow-lg">
    <footer class="px-3 shadow-inner py-8 bg-black text-2 text-white dark:text-gray-200 transition-colors duration-200">
        <div class="flex flex-col">
            <div class="md:hidden mt-7 mx-auto w-11 h-px rounded-full">
            </div>
            <div class="mt-4 md:mt-0 flex flex-col md:flex-row">
                <nav class="flex-1 flex flex-col items-center justify-center md:items-end md:border-r border-gray-100 md:pr-5">
                    <a aria-current="page" href="#" class="hover:text-gray-700 dark:hover:text-white">
                        Components
                    </a>
                    <a aria-current="page" href="#" class="hover:text-gray-700 dark:hover:text-white">
                        Contacts
                    </a>
                    <a aria-current="page" href="#" class="hover:text-gray-700 dark:hover:text-white">
                        Customization
                    </a>
                </nav>
                <div class="md:hidden mt-4 mx-auto w-11 h-px rounded-full">
                </div>
                <div class="mt-4 md:mt-0 flex-1 flex items-center justify-center md:border-r border-gray-100">
                    <a class="hover:text-primary-gray-20" href="https://github.com/Charlie85270/tail-kit">
                        <span class="sr-only">
                            View on GitHub
                        </span>
                        <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" fill="currentColor" class="text-xl hover:text-gray-800 dark:hover:text-white transition-colors duration-200" viewBox="0 0 1792 1792">
                            <path d="M896 128q209 0 385.5 103t279.5 279.5 103 385.5q0 251-146.5 451.5t-378.5 277.5q-27 5-40-7t-13-30q0-3 .5-76.5t.5-134.5q0-97-52-142 57-6 102.5-18t94-39 81-66.5 53-105 20.5-150.5q0-119-79-206 37-91-8-204-28-9-81 11t-92 44l-38 24q-93-26-192-26t-192 26q-16-11-42.5-27t-83.5-38.5-85-13.5q-45 113-8 204-79 87-79 206 0 85 20.5 150t52.5 105 80.5 67 94 39 102.5 18q-39 36-49 103-21 10-45 15t-57 5-65.5-21.5-55.5-62.5q-19-32-48.5-52t-49.5-24l-20-3q-21 0-29 4.5t-5 11.5 9 14 13 12l7 5q22 10 43.5 38t31.5 51l10 23q13 38 44 61.5t67 30 69.5 7 55.5-3.5l23-4q0 38 .5 88.5t.5 54.5q0 18-13 30t-40 7q-232-77-378.5-277.5t-146.5-451.5q0-209 103-385.5t279.5-279.5 385.5-103zm-477 1103q3-7-7-12-10-3-13 2-3 7 7 12 9 6 13-2zm31 34q7-5-2-16-10-9-16-3-7 5 2 16 10 10 16 3zm30 45q9-7 0-19-8-13-17-6-9 5 0 18t17 7zm42 42q8-8-4-19-12-12-20-3-9 8 4 19 12 12 20 3zm57 25q3-11-13-16-15-4-19 7t13 15q15 6 19-6zm63 5q0-13-17-11-16 0-16 11 0 13 17 11 16 0 16-11zm58-10q-2-11-18-9-16 3-14 15t18 8 14-14z">
                            </path>
                        </svg>
                    </a>
                    <a class="ml-4 hover:text-primary-gray-20" href="#">
                        <span class="sr-only">
                            Settings
                        </span>
                        <svg width="30" height="30" fill="currentColor" class="text-xl hover:text-gray-800 dark:hover:text-white transition-colors duration-200" viewBox="0 0 2048 1792" xmlns="http://www.w3.org/2000/svg">
                            <path d="M960 896q0-106-75-181t-181-75-181 75-75 181 75 181 181 75 181-75 75-181zm768 512q0-52-38-90t-90-38-90 38-38 90q0 53 37.5 90.5t90.5 37.5 90.5-37.5 37.5-90.5zm0-1024q0-52-38-90t-90-38-90 38-38 90q0 53 37.5 90.5t90.5 37.5 90.5-37.5 37.5-90.5zm-384 421v185q0 10-7 19.5t-16 10.5l-155 24q-11 35-32 76 34 48 90 115 7 11 7 20 0 12-7 19-23 30-82.5 89.5t-78.5 59.5q-11 0-21-7l-115-90q-37 19-77 31-11 108-23 155-7 24-30 24h-186q-11 0-20-7.5t-10-17.5l-23-153q-34-10-75-31l-118 89q-7 7-20 7-11 0-21-8-144-133-144-160 0-9 7-19 10-14 41-53t47-61q-23-44-35-82l-152-24q-10-1-17-9.5t-7-19.5v-185q0-10 7-19.5t16-10.5l155-24q11-35 32-76-34-48-90-115-7-11-7-20 0-12 7-20 22-30 82-89t79-59q11 0 21 7l115 90q34-18 77-32 11-108 23-154 7-24 30-24h186q11 0 20 7.5t10 17.5l23 153q34 10 75 31l118-89q8-7 20-7 11 0 21 8 144 133 144 160 0 8-7 19-12 16-42 54t-45 60q23 48 34 82l152 23q10 2 17 10.5t7 19.5zm640 533v140q0 16-149 31-12 27-30 52 51 113 51 138 0 4-4 7-122 71-124 71-8 0-46-47t-52-68q-20 2-30 2t-30-2q-14 21-52 68t-46 47q-2 0-124-71-4-3-4-7 0-25 51-138-18-25-30-52-149-15-149-31v-140q0-16 149-31 13-29 30-52-51-113-51-138 0-4 4-7 4-2 35-20t59-34 30-16q8 0 46 46.5t52 67.5q20-2 30-2t30 2q51-71 92-112l6-2q4 0 124 70 4 3 4 7 0 25-51 138 17 23 30 52 149 15 149 31zm0-1024v140q0 16-149 31-12 27-30 52 51 113 51 138 0 4-4 7-122 71-124 71-8 0-46-47t-52-68q-20 2-30 2t-30-2q-14 21-52 68t-46 47q-2 0-124-71-4-3-4-7 0-25 51-138-18-25-30-52-149-15-149-31v-140q0-16 149-31 13-29 30-52-51-113-51-138 0-4 4-7 4-2 35-20t59-34 30-16q8 0 46 46.5t52 67.5q20-2 30-2t30 2q51-71 92-112l6-2q4 0 124 70 4 3 4 7 0 25-51 138 17 23 30 52 149 15 149 31z">
                            </path>
                        </svg>
                    </a>
                </div>
                <div class="md:hidden mt-4 mx-auto w-11 h-px rounded-full ">
                </div>
                <div class="mt-7 md:mt-0 flex-1 flex flex-col items-center justify-center md:items-start md:pl-5">
                    <span class="">
                        © 2021
                    </span>
                    <span class="mt-7 md:mt-1">
                        Created by
                        <a class="underline hover:text-primary-gray-20" >
                            Nishikanta
                        </a>
                    </span>
                </div>
            </div>
        </div>
    </footer>
</div>
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35e9ce4e-9627-4761-9762-44604aca49b2/Screenshot_(89).png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35e9ce4e-9627-4761-9762-44604aca49b2/Screenshot_(89).png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f816feaf-e6f4-4710-b280-b9049ab7d254/Screenshot_(90).png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f816feaf-e6f4-4710-b280-b9049ab7d254/Screenshot_(90).png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d6a10f9-18db-4400-9d2d-64e2cc0d2ddc/Screenshot_(91).png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d6a10f9-18db-4400-9d2d-64e2cc0d2ddc/Screenshot_(91).png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/72844ae2-efb2-4cc9-908d-f64b81fb9bc8/Screenshot_(92).png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/72844ae2-efb2-4cc9-908d-f64b81fb9bc8/Screenshot_(92).png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/af824593-871d-4462-beb5-669c67345f97/Screenshot_(93).png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/af824593-871d-4462-beb5-669c67345f97/Screenshot_(93).png)

### Source code and full project refer my github repo.

[https://github.com/NishikantaRay](https://github.com/NishikantaRay)

# References:

- Ngx-build-plus repository:  [https://github.com/manfredsteyer/ngx-build-plus](https://github.com/manfredsteyer/ngx-build-plus)
- Angular 10 with Tailwind CSS by notiz.dev:  [https://notiz.dev/blog/angular-10-with-tailwindcss](https://notiz.dev/blog/angular-10-with-tailwindcss)
- TailWindkit [https://www.tailwind-kit.com/](https://www.tailwind-kit.com/)
- Tailblocks [https://tailblocks.cc/](https://tailblocks.cc/)
- Github repo-[https://github.com/NishikantaRay](https://github.com/NishikantaRay)
