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
