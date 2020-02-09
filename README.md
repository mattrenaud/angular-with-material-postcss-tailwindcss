#  How To Integrate PostCSS and TailwindCSS into Angular 9 and Angular Material Components.

## Part 0: Initilize an Angular CLI project.

To be sure we start with a blank slate, we'll using the CLI to generate a brand new project. Note we will use the default configuration options for a new project by bypassing interactive.

```bash
npx @angular/cli new angular-with-material-postcss-tailwindcss --interactive=false
cd angular-with-material-postcss-tailwindcss
```

After this finishes, you should end up with default project that looks something like [this](https://github.com/mattrenaud/angular-with-material-postcss-tailwindcss/tree/d6083c590f8566a142c464483155b4d26a901148).

## Part 1: Integrate PostCSS and TailwindCSS into the Angular CLI project.

### Set up a TailwindCSS test.

First I like to set up a test that confirms tailwind is working. Set the contents of `src/app/app.component.html` to the following:

```html
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  Button
</button>

```

If we start our application via `npm start`, we should see one unstyled button on the page. We'll revisit this when TailwindCSS is integrated.

### Install required dev dependencies.

```bash
npm install --save-dev postcss-loader tailwindcss @angular-builders/custom-webpack
```

### Create and generate config files.

#### Generate `tailwind.config.js` using the command line:

```bash
npx tailwind init
```
#### Create `postcss.config.js` at the root of the project with the contents:

```javascript
module.exports = {
  plugins: [
    require('tailwindcss'),
    require('autoprefixer'),
  ]
};
```
#### Create `custom-webpack.config.ts` at the root of the project with the contents:

```typescript
import { Configuration } from 'webpack';

const config: Configuration = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: 'postcss-loader',
          }
        ]
      }
    ]
  }
};

export default config;
```
You just created 3 new files in the root of your project: `tailwind.config.js`, `postcss.config.js`, and `custom-webpack.config.ts`.

### Configure the Angular CLI builder to use our custom webpack configuration.

We can use the Angular CLI to modify `angular.json` (which modifies the build targets):

```bash
npx ng config projects.'angular-with-material-postcss-tailwindcss'.architect.build.builder "@angular-builders/custom-webpack:browser"
npx ng config projects.'angular-with-material-postcss-tailwindcss'.architect.build.options.customWebpackConfig.path "custom-webpack.config.ts"
npx ng config projects.'angular-with-material-postcss-tailwindcss'.architect.serve.builder "@angular-builders/custom-webpack:dev-server"
```
### Add tailwind directives to the project CSS

Set the contents of `src/style.css` to the following:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

```

### Part 1 Verification and Review

By re-running `npm start` and opening up our browser to our running dev server (http://localhost:4200), we should see a nicely rendered tailwindcss typed button.  

In Part 1, we installed dev dependencies, created configuration files, and modified the Angular CLI to use the custom webpack builder. The result is a minimally configured Angular CLI plus postcss/tailwindcss environment. [Here is the commit with all these changes in it.](https://github.com/mattrenaud/angular-with-material-postcss-tailwindcss/commit/7454957f2fdf4a4ef3c38e1f14459bad12d7a9f5)

## Part 2: Install Material

TODO

## Part 3: Set up Material Components to be TailwindCSS friendly

TODO
