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