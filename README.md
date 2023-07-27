# webpack-config
## Installation & Configuration

> 1. init **npm config**:
```bash
npm init -y
```
> 2. install **webpack** and cli :
```bash
npm install webpack webpack-cli --save-dev
```
> 3. nest all scripts in **src** folder
> 4. create in root **webpack.config.js**
> 5. write configurations in **webpack.config.js**
```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const mode = process.env.NODE_ENV || 'development';
const target = mode === 'development' ? 'web' : 'browserslist';
const devtool = mode === 'development' ? 'source-map' : undefined;

module.exports = {
  mode,
  target,
  devtool,
  devServer: {
    open: {
      app: {
        name: 'msedge',
      },
    },
    hot: true,
  },
  entry: './src/script.js',
  output: {
    filename: '[name][contenthash].js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
    assetModuleFilename: 'assets/[hash][ext][query]',
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
    new MiniCssExtractPlugin({
      filename: '[name][contenthash].css',
    }),
  ],
  module: {
    rules: [
      {
        test: /\.html$/i,
        loader: 'html-loader',
      },
      {
        test: /\.(sa|sc|c)ss$/i,
        use: [
          MiniCssExtractPlugin.loader,
          'css-loader',
          'postcss-loader',
          'sass-loader',
        ],
      },
      {
        test: /\.(png|svg|jpg|jpeg|gif|avif|webp)$/i,
        type: 'asset/resource',
      },
      {
        test: /\.(woff|woff2|eot|ttf|otf)$/i,
        type: 'asset/resource',
      },
    ],
  },
};

```
> 6. write scripts in **package.json**:
```json 
 "scripts": {
    "start": "set NODE_ENV=development&&webpack serve",
    "dev": "set NODE_ENV=development&&webpack",
    "build": "set NODE_ENV=production&&webpack",
    "clear": "rd /s /q dist",
    "serve": "webpack serve"
  },
```
> 7. install **Dev server** (aka live-server) :
```bash
npm install webpack-dev-server -D
```
> 8. install **HtmlWebpackPlugin**:
```bash
npm i -D html-webpack-plugin
```
> 9. install other plugins for **styles**:
```bash
npm i -D mini-css-extract-plugin css-loader sass-loader sass postcss postcss-preset-env postcss-loader
```
> 10. in src folder **remove imports of styles** from index.html file and add them as import in index.js instead (do not forget about order).
```js
import './css/normalize.min.css';
import './css/bootstrap.min.css';
import './css/style.css';
// or: 
import './scss/index.scss';
```
> 11.  **Autorefixer** setup
> 	add in **webpack.config.js**
```js
const target = mode === 'development' ? 'web' : 'browserslist';

module.exports = {
target,
```
>	**webpack.config.js** add in => module: {
>		 rules: 
```js
{
test: /\.(png|svg|jpg|jpeg|gif|avif|webp)$/i,
type: 'asset/resource',
},
{
test: /\.(woff|woff2|eot|ttf|otf)$/i,
type: 'asset/resource',
},
```
> 	**webpack.config.js** add in => plugins: 
```js
new MiniCssExtractPlugin({
filename: '[name][contenthash].css',
}),
```
> 	**webpack.config.js** add in  outputs: 
```js
assetModuleFilename: 'assets/[hash][ext][query]',
```
>	create file       **.browserslistrc**        in repo's root folder. add:
```js
last 2 versions
not dead
> 0.5%
```
>	create file       **postcss.config.js**        in repo's root folder.
```js
module.exports = {
  plugins: ['postcss-preset-env'],
};
```
> 12. install **html-loader** for supporting assets in html file:
```bash
npm i -D html-loader
```
> 13. managing **fonts**
>    in src make @import in index.scss (test by running npm build)
```scss
@import "fonts";
```
> 14. install **Babel**:
```bash
npm install -D babel-loader @babel/core @babel/preset-env
```
> 	1. in **webpack.config.js**:
> 		modules:
> 			rules:
```js
{
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            cacheDirectory: true,
          },
        },
      },
```
>	in index.js and other **JS** modules:
>	2. adjust paths for avoiding conflicts:
>		for example:
>			**from :** 
>				`import {getStorage} from './phonebook/script/serviceStorage.js'
>			**to:**
>				`import {getStorage} from './phonebook/script/serviceStorage'`
>	3. for modules rename filenames:
>		example: 
>			from:
>				`createElements.js` 
>			to: 
>				`createElements.mjs`
>	4. create file in root: **babel.config.js**
```js
module.exports = {

  presets: ['@babel/preset-env'],

};
```
>	5. setup polyfills:
```bash
npm install @babel/polyfill
```
>		add in webpack.config.js :
```
entry: [
    'core-js/stable',
    '@babel/polyfill',
    ],
```
### That's all for now