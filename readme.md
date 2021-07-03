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

#### Development Process
<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124355303-9afd4400-dbcd-11eb-9d29-6297c147c7c4.png" width="450">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124355350-e152a300-dbcd-11eb-9d4c-e963ae5471fe.png" width="450">
</p>

- [ ] Benefits of using this pattern are that 3 different teams can all be working on functionality independent of one another. They can choose their own stack, own design process, and otherwise work entirely independent of the other two teams so long as they 1. use the `webpack` and 2. utilize the `ModuleFederationPlugin`

- [ ] Realize also that the `index.html` files for the _Cart_ and _Products_ MFEs are not used in production. When we deploy the app, the _Container_ index.html file is the only file being rendered from a _View_ perspective. The _Cart_ and _Product_ .html file are really there for the independent teams to test and dev with. Therefore, these .html files are usually very light in CSS and other Markup. They are simply trying to render the functionatily. The only files that are going to prod deployment are the `index.js` files within the MFEs. 

## Refactoring 

#### Shared Module Federation 

1. You may have noticed that both the `cart` and `products` MFE are using the `faker` module to create data. But each MFE is bringing in the same `faker` module which is 1. inefficient & 2. impacting performance as the file size for `faker` is 2.9M. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124362633-613f3400-dbf3-11eb-9ba0-0d8185f95cd5.png" width="450">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124362736-04904900-dbf4-11eb-9f8d-7f2f4bff2b68.png" width="450">
</p>

So the question is can we federate the modules that are being used between 2 different MFES and share the Faker module between the `cart` and `products` MFEs? It turns out we __CAN__ configure webpack to do this via the `ModuleFederationPlugin`. The steps to complete this Module Sharing are as follows...

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124362787-5c2eb480-dbf4-11eb-8bb8-ff2d560164ee.png" width="450">
</p>

2. So navigate to the `products` MFE, and within the webpack.config.js file lets update the file, to inlcude an additional attribute `shared` in the `ModuleFederationPlugin` node. We will do the same in the `cart` webpack.config.js file as well. 

```javascript 
// products webpack.config.js file
// truncated ... 
    plugins: [
        new ModuleFederationPlugin({
            name: 'products', 
            filename: 'remoteEntry.js', 
            exposes: {
              './ProductsIndex': './src/index'
            }, 
            shared: ['faker']
          }),
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
// truncated ... 
```

```javascript 
// cart webpack.config.js file
// truncated ... 
    plugins: [
        new ModuleFederationPlugin({
            name: 'cart',
            filename: 'remoteEntry.js', 
            exposes: {
                './CartShow':'./src/index'
            }, 
            shared: ['faker']
        }),
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ]
// truncated ... 
```

3. Because we made a chage to the webpack.config.js files of both `cart` and `products` we will need to restart these services for the config changes to take affect.

```javascript 
// ecomm/cart
npm run start
```

```javascript 
// ecomm/products
npm run start
```

4. Now if you nav to your browser and look at the network tab, you can see there is only 1 instance of faker being shared by both cart and products MFES. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124363076-59cd5a00-dbf6-11eb-94ee-4c315223617b.png" width="450">
</p>


> NOTE: That while we were able to share the `faker` module while runing container, we __break__ the products MFE from running by itself. Why? Because if webpack runs asynchronously... and when we load `products` our first statement in the javascript file is to `import faker` ... but when we share the file running from container, `products` hasn't had `faker` loaded and we therefore receive an error. So when sharing ... the container will run fine ... but will break the rendering of the individual MFE. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124363149-db24ec80-dbf6-11eb-9b21-d2440d598102.png" width="450">
</p>

5. To fix this error, and make the `products` and `container` MFE still render the import statements in an ascync manner, we need to use the `import()` function method that we used in our pattern in container. Note in the `container` MFE, we don't use an `import faker` statement ... instead we use an `import(_something_)` function. The use of the function versus the statement allows JS to manage the async rendering of the `products` MFE. 

6. Nav to the `products/src` folder and create a file called `bootstrap.js`

7. Copy all the content of the `products/src/index.js` and paste the code to the newly created `products/src/bootstap.js`. 

8. Now in the `products/src/index.js` file, simply add 

```javascript
import('./bootstrap');
```

> NOTE: This may all seem administrative and pointless, but what we are trying to do is allow webpack to asyncronously load the code that requires the faker module. 

9. Now you can see in the browser that both the `container` and the `products` MFEs both work correctly. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/8760590/124363539-06104000-dbf9-11eb-8e2c-c8237d8ee545.png" width="450">
</p>

> NOTE: You'll see this same behaviour with the async loading happening in the `cart` MFE ... so go to the `cart/src` file and repeat the `import()` function pattern. 

#### Verison impact with shared modules

1. So as already has been stated, one of the benefits of MFEs is that 2 different teams can work independently. Well what happens if 2 teams are sharing a faker module, but using different version of the module? 
