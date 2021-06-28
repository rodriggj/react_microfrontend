# React MicrofrontEnd 
> Udemy Course __Microfrontends with React: A Complete Developers Guide__. Author, Stephen Grinder

## Initial Build 

Steps: 
1. We will start by building the `Products` Micro-Front End (MFE). 

2. Create a directory structure to house the project
```javascript
cd ~/Desktop
mkdir ecomm
cd ecomm
mkdir products 
cd products
```

3. Initialize the `Products` MFE

```javascript 
npm init -y 
```

4. Install dependencies
```javascript
npm install webpack@5.4.0 webpack-cli@4.2.0 webpack-dev-server@3.11.0 faker@5.1.0 html-webpack-plugin@4.5.0 --save
```

> NOTE: The versions of these dependencies are subject to change.


5. Create a `src` folder and create an `index.js` file

```javascript
mkdir src
code index.js
```

6. Create a script within `index.js` that will produce a Product name from `faker`. 

```javascript
import faker from 'faker';

let products = '';

for (let i = 0; i < 3; i++) {
    const name = faker.commerce.productName();
    products += <div>${name}</div>
}

console.log(products)
```

> NOTE: You cannot run the `index.js` file using `node index.js`. If you try you will get an error indicating that `Cannot use import statement outside a module`. This is one of the reasons you need `webpack`. Webpack will bundle your code an pull in the appropriate dependencies. 

7. Create a file called `webpack.config.js` and populate it as follows...

```javascript
module.exports = {
    mode: 'development'
};
```

8. Now open the `package.json` file and replace the "test" script with "start" script as follows...

```javascript
"scripts": {
    "start": "webpack"
}  
```

9. Navigate to your `products` directory and run 

```javascript
npm run start
```

>__CONSOLE OUTPUT:__ If your webpack complied correctly you will see console output similar to the following: 
<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123555680-7247fb00-d744-11eb-9bc1-c60448068ff3.png" width="450">
</p>

>__DIRECTORY STRUCTURE:__ Your webpack build will have produced an additional directory in your directory structure called `dist`, with a file called `main.js`.

9. What is happening with this output on the console and the directory structure addition? 

<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123555787-1e89e180-d745-11eb-835d-9480fd1fc2c1.png" width="450">
</p>

+ When we have to include dependencies in our application build, we don't want to have to load a series of files to the browser. 
+ Webpack handeles this problem for us by taking all our dependency files and bundling the files into a single file, in this case the file called `main.js`. Sometimes this `main.js` file may also be named, `bundle.js`. 
+ So in this example the `main.js` _bundled_ our `index.js` file for products, the `faker` files, and all our supporting files (e.g package.json) and combined them to a single file to be provided to the browser called, `main.js`.

10. Now that that single file has been produced, we need some means of reading the file and all the combined code from our webpack bundle. This will be done using `webpack-server`. To configure this server to read the file, we will need to make 2 changes: 

+ 1. A small modification to our `webpack.config.js` file to add a `devServer` node with the port specified

```javascript
// webpack.config.js file
module.exports = {
    mode: "development",
    devServer: {
        port: 8081,
    }
};
```

+ 2. You will also need to modify the `package.json` file to modify the `start` script. Add the `serve` term to the previous `start` command from _Step 8_ above.

```javascript
   "scripts": {
    "start": "webpack serve"
  },
```

> __NOTE:__ This is a graphical depiction of what is going on ... 
<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123556087-9278b980-d746-11eb-8f35-7152678c590b.png" width="450">
</p>

11. Now try to run the products application again. Nav to `products` directory and run the `start` script 

```javascript 
npm run start
```

>__CONSOLE OUTPUT:__ If your webpack complied correctly you will see console output similar to the following: 
<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123556455-94dc1300-d748-11eb-9cfe-e54ebee95033.png" width="450">
</p>

> __NOTE:__ If you navigate to the URL listed in the console `http:localhost:8081`, and you navigate the directory structure to the bundled file, `main.js` in the `dist` directory, you will see text of the file, but not an `html` rendering of our `<div>{productName}</div>` that we programmed on our javascript file. To see the html rendering of this file we need to create an `html` view and reference the `dist/main.js` file in a script tag. 

12. To create an html file that will render our `dist/main.js` webpack bundle in the browser we need to create an html file and reference `main.js` via a script tag. To do so, complete the following: 

+ 1. Create a new directory withing `products` called `public`. 

```javascript
mkdir public
```

+ 2. In the `public` directory, create a new file called `index.html` and enter boilerplate html code. 

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Product MFE</title>
  </head>
  <body>
    <h1>Hello, world!</h1>
  </body>
</html>
```

> NOTE: As we previously stated we want to reference the `main.js` file via a script tag.A straight forward way to do this would be something like this....

<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123556734-12ece980-d74a-11eb-93b8-a281d6af1adb.png" width="450">
</p>

> But we don't want to do this. Why? As we make modifications to our webpack bundle by adding additional dependencies, the file name created by `webpack` will have unpredicatable naming covnetions associated with it, looking something like this ... 

<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123556713-023c7380-d74a-11eb-84ca-ae38b6bc7f16.png" width="450">
</p>

> Therefore, what we want to do is let `webpack` create the script tag for us that will be placed in the html file for us. This inserton will be completed by a webpack plugin module called `html-webpack-plugin`.

<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123556925-28164800-d74b-11eb-9e5f-d1097f023d78.png" width="450">
</p>

13. To add the `html-webpack-plugin` navigate back to the `webpack.config.js` file and add the following...

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    mode: "development",
    devServer: {
        port: 8081,
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
};
```

14. Now if we restart the server with `npm run start` you should see in the html ouput that a script tag was added to the `index.html` file (using Google Developer Tools) to idenfity a `script` tag with a `src` attribute that identifies our `dist/main.js` file. You can also see that the `faker` output for the product names displaying to the console. 

<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123557114-4fb9e000-d74c-11eb-921c-10675042659a.png" width="450">
</p>



15. Now that we have `webpack` handling all our dependencies, creating a bundle.js file that will be passed to the browser, and that file executing our code that we placed in products -> index.js file, the final thing we have to do is render our faker output to the html page instead of the console. To do this we simply utilize standard javascript to insert the faker names to the html file. 

```javascript 
// ecomm/products/src/index.js file
import faker from 'faker';

let products = '';

for (let i = 0; i < 3; i++) {
    const name = faker.commerce.productName();
    products += `<div>${name}</div>`;
}

document.querySelector('#dev-products').innerHTML = products
```

```html
<!-- ecomm/public/index.html file -->
<html lang="en">
  <head>
    <title>Product MFE</title>
  </head>
  <body>
    <div id="dev-products"></div>
  </body>
</html>
```

16. Restart the sever and validate in the browser that the html now renders the faker info for 3 products

<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123557540-9f99a680-d74e-11eb-9bce-143317e4c639.png" width="450">
</p>

17. To complete this initial build we will complete the scafolding for the `container` MFE. Let's begin by creating a directory structure to house the `container` MFE and the `npm init` process and dependencies just like Steps 1 - 5 of the `products` MFE. Nav to the `ecom` app and execute the following. 

```javascript 
mkdir container
cd container
npm init -y 
npm install webpack webpack-cli webpack-dev-server html-webpack-plugin nodemon --save
mkdir src
cd src && code index.js
```

```javascript
// on index.js file 
console.log('Container!');
```

> __NOTE:__ There is no `faker` package. We added `nodemon` package

18. Create a directory for the html view, just like we did in _Step 12_ above. Populate the index.html with boiler plate html code. 

```javascript 
mkdir ecomm/container/public
code ecomm/container/public/index.html
```

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Container MFE</title>
  </head>
  <body>
    <h1>Inside the Container</h1>
  </body>
</html>
```

19. Create and configure the `webpack.config.js` file just like we did on _Step 13_. Within the file add the boilerplate code.

```javscript 
cd ecomm
code webpack.config.js
```

```javascript 
// webpack.config.js file 
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    mode: 'development', 
    devServer: {
        port: 8080
    }, 
    plugins: [
        new HtmlWebpackPlugin ({
            template: './public/index.html'
        })
    ]
}
```

20. Finally we need to modify the `package.json` to configure the scripts. 

```javascript 
  "scripts": {
    "start": "webpack serve"
  }
```

