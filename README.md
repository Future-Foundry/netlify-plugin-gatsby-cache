# Netlify Build Plugin: Persist the Gatsby Cache Between Builds

Persist the Gatsby cache between Netlify builds for huge build speed improvements! ⚡️

> NOTE: Netlify Build Plugins are in beta. [To use this plugin, request an invite!](https://www.netlify.com/build/plugins-beta/?utm_source=github&utm_medium=netlify-plugin-gatsby-cache-jl&utm_campaign=devex)

## Usage

First, install the plugin in your repo:

```bash
# using npm
npm install netlify-plugin-gatsby-cache

# or using Yarn
yarn add netlify-plugin-gatsby-cache
```

Add the plugin in your `netlify.yml`:

```diff
  build:
    command: yarn build
    publish: public
    functions: functions

+ plugins:
+   - type: netlify-plugin-gatsby-cache
```

This plugin determines the location of your `.cache` folder by looking a the `publish` folder configured for Netlify deployment (this is typically set in your `netlify.yml`/`netlify.toml` in the `build` section). This means that if your Gatsby site successfully deploys, it will be cached as well with no config required! 🎉

## How much of a difference does this plugin make in build times?

Each Gatsby site is different, so build times vary widely between them, but one common slowdown in Gatsby builds is processing and transforming images. Gatsby is smart enough to check if these transformations have already been done and skip them, but in order to get that benefit in a build pipeline (e.g. Netlify) the `public` and `.cache` directories need to be preserved between builds. That’s what this plugin does!

On a site with 231 GraphQL queries, 1,871 images, and 224 pages, build times dropped by 75%:

- no cache: ✔  Netlify Build completed in 293207ms ([build log](https://app.netlify.com/sites/lengstorf/deploys/5dceed27d58a580008daaccc))
cache: ✔  Netlify Build completed in 72835ms ([build log](https://app.netlify.com/sites/lengstorf/deploys/5dcef2463da4810008d48aaa))

On a small site (5 GraphQL queries, no image processing, 32 pages), build times dropped by 30% when using this plugin:

 no cache: :heavy_check_mark:  Netlify Build completed in 22072ms ([build log](https://app.netlify.com/sites/build-plugin-test/deploys/5dceed49e746a200091c76fe))
- cache: :heavy_check_mark:  Netlify Build completed in 15543ms ([build log](https://app.netlify.com/sites/build-plugin-test/deploys/5dceedbfad95d0000bcd46d1))

tl;dr: Repeat builds with lots of images will be _much_ faster. With few or no images, the difference will be there, but it won’t be as pronounced.

## Want to learn how to create your own Netlify Build Plugins?

Check out [Sarah Drasner’s excellent tutorial](https://www.netlify.com/blog/2019/10/16/creating-and-using-your-first-netlify-build-plugin/?utm_source=github&utm_medium=netlify-plugin-gatsby-cache-jl&utm_campaign=devex)!
