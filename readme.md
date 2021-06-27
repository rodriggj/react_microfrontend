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

- [ ] When we have to include dependencies in our application build, we don't want to have to load a series of files to the browser. 
- [ ] Webpack handeles this problem for us by taking all our dependency files and bundling the files into a single file, in this case the file called `main.js`. Sometimes this `main.js` file may also be named, `bundle.js`. 
- [ ] So in this example the `main.js` _bundled_ our `index.js` file for products, the `faker` files, and all our supporting files (e.g package.json) and combined them to a single file to be provided to the browser called, `main.js`.

10. Now that that single file has been produced, we need some means of reading the file and all the combined code from our webpack bundle. This will be done using `webpack-server`. To configure this server to read the file, we will need to make 2 changes: 

1. A small modification to our `webpack.config.js` file to add a `devServer` node with the port specified

```javascript
// webpack.config.js file
module.exports = {
    mode: "development",
    devServer: {
        port: 8081,
    }
};
```

2. You will also need to modify the `package.json` file to modify the `start` script. Add the `serve` term to the previous `start` command from _Step 8_ above.

```javascript
   "scripts": {
    "start": "webpack serve"
  },
```

> __NOTE:__ This is a graphical depiction of what is going on ... 
<p align="center">
    <img src="https://user-images.githubusercontent.com/8760590/123556087-9278b980-d746-11eb-8f35-7152678c590b.png" width="450">
</p>


