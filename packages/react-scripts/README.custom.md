# Custom Changes

- Add `tailwindcss` so it is supported by default
- Add `focus-visible` package with `postcss-focus-visible` so we can use `:focus-visible` pseudoselector.
- Add ability to override the files based on parameter you pass in the command, which will be described below.

## Overriding File to be Used

Assuming you want to use custom `index.html` file or `.env` file for different environments. Default `react-scripts` does not support this use case as it always use `public/index.html` for HTML and it always use `.env` or `.env.production` during build.

1. Create a file called `react.config.js` at the root of your project with the following contents:

   ```js
   module.exports = {
     overrides: {
       preprod: {
         // assuming you have a `index.preprod.html` file in `public` folder that should be used when your build target is `preprod`.
         appHtml: path.resolve(__dirname, 'public', 'index.preprod.html'),
         // assuming you have a `.env.preprod` file in an `env` folder that should be used when your build target is `preprod`.
         dotenv: path.resolve(__dirname, 'env', '.env.preprod'),
       },
       testing: {
         appHtml: path.resolve(__dirname, 'public', 'index.testing.html'),
       },
     },
   };
   ```

   The file must export an object like the example above. `overrides` property is an object whose properties are the type of configuration you want to support (usually it will be your environment, like `staging`, `preprod`, `prod` etc.).

1. Each object can define two properties: `appHtml`, and `dotenv`, which must be the absolute path that you want to override.

1. Then, when you run the command, you can pass `--config <env>` to override them, e.g.

   ```bash
   npx react-scripts build --config preprod
   ```

   or just define the commands in your `package.json`:

   ```json
   {
     "scripts": {
       "build:preprod": "react-scripts build --config preprod",
       "build:testing": "react-scripts build --config testing"
     }
   }
   ```
