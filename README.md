# create-pwa-react-vite
**My steps to create a functional pwa with react on vite using gh-pages and vite-plugin-pwa**

## _First Step_

1. Create a React Web App with Vite
```shell
npm create vite@latest
```
> We have to choose the react-ts template

2. We enter or new proyect

```shell
cd my-project

npm install
npm run dev
```

3. Once the proyect start we add the modules to make it PWA

```shell
npm install gh-pages --save-dev

npm i vite-plugin-pwa -D   
```

4. Now to make it deployable on gh-pages, we add the following in to our `vite.config.ts`
```shell
base: "/{repo-name}/"
````
5. Now add the folowing into `package.json` scripts section

```shell
"deploy": "gh-pages -d dist"
```

6. Configure Git

```shell
git init

git remote add origin https://github.com/{username}/{repo-name}.git
```

> Now the gh-pages conf is ready, if we want to deploy it, we can be executing the comand `npm run build` and `npm run deploy`

> Also for the deployment make sure the ico for your app is in the `public` folder and is referenced on `index.html`. And any resource used on yours `.tsx` should by inside your `src` folder, if not your GitHub Page will be missing those resources 

> Considering that we can deploy our app on Github Pages

## _PWA_

1. Now to configure VitePWA we add the following in to `vite.config.ts` on the `plugins` section

```shell
VitePWA({
      registerType: "autoUpdate",
      workbox: {                                            // File types that will be saved to make it function offline
        globPatterns: ["**/*.{js,css,html,ico,png,svg}"], //you can add other file types 
                            
      },
      manifest: {
        name: "vite-project",
        short_name: "vite-project",
        start_url: "/{repo-name}/", ///pending to try it with routes  //      './' ??? or './
        display: "standalone",
        background_color: "#ffffff",
        lang: "en",
        scope: "/{repo-name}/",     ///pending to try it with routes
        icons: [    //all of this icons should be on public folder
                    // All necesari to make the Web App installable
          {
            "src": "favicon.ico",
            "sizes": "64x64 32x32 24x24 16x16",
            "type": "image/x-icon"
          },
          {
            "src": "logo192.png",
            "type": "image/png",
            "sizes": "192x192"
          },
          {
            "src": "logo512.png",
            "type": "image/png",
            "sizes": "512x512"
          }
        ],
        theme_color: "#000000",//Should be the same into index.html
      },
    }),
```
2. Now we have to modify `index.html`

```shell
<meta name="theme-color" content="#000000"/>
```
3. Now we are ready to buildt and deploy it


## _Routes_

1. Add the folowing to `vite.config.ts`

```shell
resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
```


2. Now for the routes we use react-router and for the import of components we use `React.lazy()` example:

```shell
const About = React.lazy(() => import("./pages/About"));

<Route
            path="about"
            element={
              <React.Suspense fallback={<>...</>}>
                <About />
              </React.Suspense>
            }
/>
```

< We well need a function when we go to not existing route in our app

```shell
<Route path="*" element={<NoMatch />} />


function NoMatch() {
  return (
    <div>
      <h2>Nothing to see here!</h2>
      <p>
        <Link to="">Go to the home page</Link>
      </p>
    </div>
  );
}
```

3. We need to modify our ``main.tsx` with `HashRouter`

```shell
 <HashRouter>
    <App />
  </HashRouter>

```
> Pending to check 

```shell
export default defineConfig({
  plugins: [
    react(),
    VitePWA({
      manifest,
      includeAssets: ['favicon.svg', 'favicon.ico', 'robots.txt', 'apple-touch-icon.png'],
      // switch to "true" to enable sw on development
      devOptions: {
        enabled: false,
      },
      workbox: {
        globPatterns: ['**/*.{js,css,html}', '**/*.{svg,png,jpg,gif}'],
      },
    }),
  ],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
});

```

```shell
<head>
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>My Awesome App</title>
  <meta name="description" content="My Awesome App description">
  <link rel="icon" href="/favicon.ico">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png" sizes="180x180">
  <link rel="mask-icon" href="/mask-icon.svg" color="#FFFFFF">
  <meta name="theme-color" content="#ffffff">
</head>
```



