# React MicrofrontEnd 
> Udemy Course __Microfrontends with React: A Complete Developers Guide__. Author, Stephen Grinder

## Module Federation 

> We've built lightweight versions of the `container` and `product` MFE, now we need to integrate the two. To execute the integration of these MFEs we will follow the steps here ... 

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/123636867-96561b80-d7da-11eb-8565-aed99fa4a239.png" width="450">
</p>

- [ ] `HOST`: container
- [ ] `REMOTE`: products

-------

Steps: 
1. Nav to `products` directory and open the `webpack.config.js` file. Require into that file `ModuleFederationPlugin`. 

```javascript 
const HtmlWebpackPlugin = require('html-webpack-plugin')
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin')
```

2. Now within the same file, configure your plugin: 

```javascript 
    plugins: [
        new ModuleFederationPlugin({
          name: 'products', 
          filename: 'remoteEntry.js', 
          exposes: {
            './ProductsIndex': './src/index'
          }
        }),
        new HtmlWebpackPlugin({
          template: './public/index.html'
        })
    ]
```

3. Nav to the `container` directory and in the `webpack.config.js` file, require in `ModuleFederationPlugin`

```javascript 
const HtmlWebpackPlugin = require('html-webpack-plugin')
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin')
```

4. Again configure the `container` config file for the `ModuleFederationPlugin` with options: 

```javascript 
 plugins: [
        new ModuleFederationPlugin({
          name: 'container', 
          remotes: {
            products: 'products@http://localhost:8081/remoteEntry.js'
          }
        }),
        new HtmlWebpackPlugin({
          template: './public/index.html'
        })
    ]
```

5. In the `contaier/src` directory we are going to rename the file `index.js` to `bootstrap.js`. 

6. Create a new file within `container/src` called `index.js`

```javascript 
code container/src/index.js
```

7. Add an `import function call` to the index.js file, which will import the `booststrap.js` file we just renamed in _Step 7_.

```javascript 
import('./bootstrap');
```

8. Import `products.js` to our `container/src/boostrap.js` file. 

```javascript 
import 'products/ProductsIndex'; 

console.log('Container!');
```

9. If `container` MFE and/or `products` MFE are currently running, stop and restart these services to implement the changes to the config files. 

```javascript 
// Open a terminal session for container
npm run start
```

```javascript 
// Open a terminal session for products
npm run start
```

> NOTE: Changed the `container` `webpack.config.js` file to listen on port `8082` vs. `8080` that was originally configured because there is a Jenkins application running locally on port `8080`

> RESULTS: Terminal Sessions

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124175673-1eeaeb00-da6b-11eb-8c4c-5274b0c5e570.png" width="450">
</p>

> RESULTS: Browser Sessions

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124175930-725d3900-da6b-11eb-8ffe-865105031c9d.png" width="450">
</p>

> NOTE: The error in the `container` browser session `...no innnerHTML...` is because we are attempting to federate the `products` view in the `container` view, but the `container` view doesn't have an HTML element with an id of `dev-products`. 

10. Add a reference element in the `container` html to render the `products` list. Nav to the `container/public/index.html` file and add an element that the `products` javascript file is attempting to reference which is `<div id="dev-products"></div>`. The HTML update should be recognized by the Browser without a restart of the container MFE. 

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Container MFE</title>
  </head>
  <body>
    <h1>Inside the Container</h1>
    <div id="dev-products"></div>
  </body>
</html>
```

> RESULTS: 

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124176670-6f167d00-da6c-11eb-9b82-210ea9fdab66.png" width="450">
</p>

## Explanation 

#### Products MFE (aka REMOTE)
- [ ] This is what `webpack` is doing in the `products` MFE

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124177202-21e6db00-da6d-11eb-8dcc-4e0f3b07ae6c.png" width="450">
</p>


#### Container MFE (aka HOST)
- [ ] This is what `webpack` is doing in the `container` MFE

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124177802-f31d3480-da6d-11eb-8229-ef338f06709f.png" width="450">
</p>

#### Combined Activity

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124178739-0f6da100-da6f-11eb-8d2b-9567be24151f.png" width="450">
</p>

You can validate this flow in the Google Dev Tools 
<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124178943-5360a600-da6f-11eb-9b9b-a4d42c7ba776.png" width="450">
</p>


## Understanding the `webpack.config.js` input 

#### Container `webpack.config.js` file

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124179143-9de22280-da6f-11eb-9e29-d15fa0c9568e.png" width="450">
</p>

##### Products `webpack.config.js` file

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124179434-18ab3d80-da70-11eb-8b5f-7263743bab9f.png" width="450">
</p>

##### How the 2 files relate 

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124179716-73dd3000-da70-11eb-96be-16a72cf8ea58.png" width="450">
</p>

11. Now we'll add our last piece to this initial build which will be the `cart` MFE. Here we can create a directory called cart and to save a little time we can copy/paste the `package.json` file from the `products` MFE to the `cart` MFE. In the `package.json` file change the name from `products` to `cart`.

```javascript
mkdir ecomm/cart
cp ecomm/products/package.json ecomm/cart
```

```javascript
{
  "name": "cart",
  "version": "1.0.0",
  "description": "",
  ...
}
```

12. Now nav to the `cart` dir and run `npm install` to install the `package.json` dependencies. 

```javascript
cd ecomm/cart
npm i
```

13. In the `cart` dir, create a `public` dir and an `index.html` page with a div and an id called `dev-cart`.

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Cart MFE</title>
  </head>
  <body>
    <h1>You are inside the Cart MFE</h1>
    <div id="dev-cart"></div>
  </body>
</html>
```

14. Inside of the `cart` dir, create a sub dir called `src` and and `index.js` file. Input the following code to the index.js file. 

```javascript
import  faker from 'faker';

const cartText = `<div>You have ${faker.datatype.number()`} items in your cart.</div>`;

document.querySelector('#dev-cart').innerHTML = cartText;
```

15. Finally we need to create a `webpack.config.js` file.

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin');

module.exports = {
    mode: "development", 
    devServer: {
        port: 8083
    },
    plugins: [
        new ModuleFederationPlugin({
            name: 'cart',
            filename: 'remoteEntry.js', 
            exposes: {
                './CartShow':'./src/index'
            }
        }),
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
}
```

16. Now open a new terminal and run `npm run start` for your `cart` MFE. 

> RESULTS: 

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124354559-d5fd7880-dbc9-11eb-9710-9f660136880a.png" width="450">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124354638-32609800-dbca-11eb-9982-ddef9b7f89cb.png" width="450">
</p>

17. Now let's integrate our `cart` to the `container` host. To do this we need to modify the `cart` `webpack.config.js` file. Here in the `ModuleFederationPlugin` / `remotes` section of our config we need to add `cart`. 

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin')

module.exports = {
    mode: 'development',
    devServer: {
        port: 8082
    },
    plugins: [
        new ModuleFederationPlugin({
            name: 'container', 
            remotes: {
              products: 'products@http://localhost:8081/remoteEntry.js', 
              cart: 'cart@http://localhost:8083/remoteEntry.js'
            }
        }),
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
}
```

18. Then in our `container/src/bootstrap` file we also need to add an import statement. 

```javascript
import 'products/ProductsIndex'; 
import 'cart/CartShow';

console.log('Container!');
```

> NOTE: Recall what is going on here.... 
1. First webpack is going to read the boostrap.js file, from `src` and see that there is an import statement for `cart`. 
2. Webpack is then going to look into the `container/webpack.config.js` file and realize that the import statement for `cart` is aligned to the `remotes` section, and see that `cart` is at the URL location, which designates a `remoteEntry.js` file. Because the URL is pointing at the `cart` service webpack will recognize the `webpack.config.js` file fromt the `cart` service. 
3. In the `cart` MFE is a webpack.config.js file where the `ModuleFederationPlugin` has a config attribute `name` which equals `cart`, with a fileName of `remoteEntry.js`. The config indicates to expose the `./CartShow` (which is what is referenced in the `container` bootstrap file), and when you reference `./CartShow` render the file at the path `./src/index` which happens to be our JS file that utilzes `faker` to render a div with a random number. 

19. The last portion of this process is to make sure that the `container` html has a reference to the javascript query selector so it can render the content in the DOM. So in the `container/public/index.html` file, insert one more div with a ID to reference the `cart` javascript. 

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Container MFE</title>
  </head>
  <body>
    <h1>Inside the Container</h1>
    <div id="dev-products"></div>
    <hr>
    <div id="dev-cart"></div>
  </body>
</html>
```

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124355068-8e2c2080-dbcc-11eb-9319-f1d4ea0e5a8e.png" width="450">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124355096-ba47a180-dbcc-11eb-9bcf-2d6375bbbe8a.png" width="450">
</p>