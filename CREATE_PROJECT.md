# How to Create a React App Using Rush.js

This guide will take you through the steps of setting up a React application within a Rush.js monorepo environment.

## 1. Install Rush.js

Make sure you have Node.js installed, then globally install Rush.js:

```
npm install -g @microsoft/rush
```

## 2. Initialize a Rush Monorepo

Create a new folder for your monorepo, navigate into it, and initialize Rush:

```
mkdir my-rush-monorepo
cd my-rush-monorepo
rush init
```

Rush will create the required files like `rush.json` and the `common` folder.

## 3. Configure Rush to Use pnpm

By default, Rush uses pnpm. To ensure you're using pnpm, open `common/config/rush/pnpm-config.json`. If the file doesn't exist, create it, and add the following configuration:

```json
{
  "useWorkspaces": true
}
```

## 4. Create a React App Manually

Create a folder for your React app under `apps`:

```
mkdir -p apps/my-react-app
cd apps/my-react-app
pnpm init
```

This will create a `package.json` file for your project. Install React and development dependencies:

```
pnpm add react react-dom typescript @types/react @types/react-dom
pnpm add -D webpack webpack-cli webpack-dev-server @babel/core babel-loader @babel/preset-env @babel/preset-react @babel/preset-typescript ts-loader html-webpack-plugin
```

## 5. Set up TypeScript

Create a `tsconfig.json` file in the root of `apps/my-react-app`:

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "ESNext",
    "jsx": "react-jsx",
    "moduleResolution": "node",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"]
}
```

## 6. Set up Webpack

Create a `webpack.config.js` file in the root of `apps/my-react-app`:

```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.tsx",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
  },
  resolve: {
    extensions: [".js", ".jsx", ".ts", ".tsx"],
  },
  module: {
    rules: [
      {
        test: /\.(ts|tsx)$/,
        exclude: /node_modules/,
        use: "ts-loader",
      },
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: "babel-loader",
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
  ],
  devServer: {
    static: {
      directory: path.join(__dirname, "dist"),
    },
    compress: true,
    port: 8000,
  },
};
```

## 7. Create the Project Structure

Inside `apps/my-react-app`, create the necessary directories and files:

```
mkdir src public
touch src/index.tsx public/index.html
```

Inside `public/index.html`, add the basic HTML:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My React App</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

## 8. Create a Basic React Component with TypeScript

Inside `src/index.tsx`, add the following:

```tsx
import React from "react";
import ReactDOM from "react-dom";

const App: React.FC = () => <h1>Hello, React with TypeScript and Rush.js!</h1>;

ReactDOM.render(<App />, document.getElementById("root"));
```

## 9. Add the Project to Rush

Open `rush.json` and add the React project to the "projects" section:

```json
{
  "projects": [
    {
      "packageName": "my-react-app",
      "projectFolder": "apps/my-react-app",
      "reviewCategory": "production"
    }
  ]
}
```

## 10. Run Rush Update and Install Dependencies with pnpm

```
rush update
```

This will install and link all dependencies using pnpm.

## 11. Add Build and Start Scripts to package.json

In `apps/my-react-app/package.json`, add the following scripts:

```json
"scripts": {
  "start": "webpack serve --mode development",
  "build": "webpack --mode production"
}
```

## 12. Start the Development Server

Now you can start the development server using pnpm:

```
cd apps/my-react-app
pnpm start
```

Your React app with TypeScript should now be running at <http://localhost:8000>.

## 13. Use Rush Commands

To build or manage multiple projects in your monorepo, you can use the following commands:

```
rush build – builds all projects.
rush rebuild – cleans and builds all projects from scratch.
```

This setup will give you a React app running with TypeScript, managed by Rush.js and using pnpm for dependency management.
