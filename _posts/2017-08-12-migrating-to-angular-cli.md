---
author: Joe Kampschmidt
comments: true
layout: post
slug: migrating-to-angular-cli-build
title: Migrating a project to Angular CLI build system
desc: 'A real example of migrating an existing Angular client app and backend typescript Express server to an Angular CLI build system. Including in-depth scripts, lessons learned, pain points and build server implementation.'
tags:
- devops
- angular-cli
- javascript
---

I recently migrated an Angular app from a gulp build system to an [Angular CLI](https://cli.angular.io/) (version 1.2.6). I am enjoying the new build system and the improved Angular performance. I wanted to share the process, some pain points and lessons learned.

A good starting point for migrating is the angular-cli [migration guide](https://github.com/angular/angular-cli/wiki/stories-1.0-update) and [moving guide](https://github.com/angular/angular-cli/wiki/stories-moving-into-the-cli). Also check out the [list of general stories](https://github.com/angular/angular-cli/wiki/stories)

Our migration plan was a little more involved since our project consists of **an Angular client app and a backend node typescript server with Express.js to serve our client app and proxy an external API**.

<a name="migration"></a>
## [Migration Outline](#migration)

- Create a new angular-cli project
- Copy over (move) your existing application into the new project
- Merge package.json files and resolve package versions
- Iterate over the app, build and tests
- Switch over build and run scripts

<a name="goals"></a>
## [Goals](#goals)

- Replace current gulp commands and code with angular-cli and npm scripts
- Work on both windows and mac development machines
- Be able to run a local server to host an API and client ap
- Create a shell script for build and packaging for the build server

<a name="preparation"></a>
## [Migration Preparation](#preparation)

- Move all static resources into the `assets` folder and ensure relative paths
- Ensure `node` and `npm` prereqs for `angular-cli`. I prefer `nvm`.
- Find and fix any private variables being referenced in components. Make those vars public.

<a name="lessons-learned"></a>
## [Lessons Learned and Pain Points](#lessons-learned)

- I only had one global `tsconfig.json` file before but angular-cli makes use of multiple `tsconfig.json` files and for a new `tsconfig.app.json` file. Also, as of right now some editors ( [VS Code #24362](https://github.com/Microsoft/vscode/issues/24362)) don't support custom tsconfig filenames yet. In some cases I had to create a `tsconfig.json` for VS Code alongside the `tsconfig.app.json` file in order for the editor to recognize specific paths. Not an ideal solution.

- I used to be to run `tsc` and compile both server and client apps at the same time. Unfortunately, with the cli I now have to compile the client and server apps separately and with different commands and config.

- If you are going to ditch gulp entirely you will need to embrace `npm scripts` to augment `angular-cli`. I found this resource ["Why I Left Gulp and Grunt for npm Scripts"](<https://medium.freecodecamp.org/why-i-left-gulp-and-grunt-for-npm-scripts-3d6853dd22b8>) by Cory House a must read before the migration.

- It is important to realize that `ng serve` and `ng test` compile your typescript into memory and NOT physical files.

- If you are clueless as to why `ng test` doesn't work with cryptic errors look into the `pollyfills.ts` file and start uncommenting lines.

- I would recommend locking package.json versions with exact version numbers, shrinkwrap, lockfile or even yarn. There is no joy in debugging `@types`, `tsc` versions and incompatible modules.

- Building with `ng build --prod` which uses AOT compilation is a lot more strict than the default typescript rules with `ng serve`.

- You may run into libraries that don't support AOT compilation yet. For example, we had to switch from [Alberplz/angular2-color-picker](https://github.com/Alberplz/angular2-color-picker) to [zefoy/ngx-color-picker](https://github.com/zefoy/ngx-color-picker)

- In cases where you want to (not really necessary to remove just for the sake) but you can use plain old js node processes and wrap them in `npm scripts`.

- Using `sass` for css is straightforward by setting `"styleExt": "scss"` in `.angular-cli.json`. See [Angular2 - Angular-CLI SASS options](https://stackoverflow.com/questions/36220256/angular2-angular-cli-sass-options)

- Using `font awesome` was straightforward by simply adding `"../node_modules/font-awesome/css/font-awesome.css"` to the `styles` array in `.angular-cli.json` file.

<a name="scripts"></a>
## [Serve and Build Scripts](#scripts)

**Serving for development work**

Just use `npm start`.

This compiles and serves the client app from memory.

Underneath the hood, serving our client and server requires some extra steps since we also need to compile a typescript server application. This gets tricky since `ng serve` is running its own lite http server. We can actually proxy some routes in the `ng serve` to our backend server running on a different port. This is available with a `--proxy-config` switch.

When we compile our client app with `ng build` we can actually just serve our node server and the compiled client since our app handles all the routing by design.

This is why there are some port number and proxy switching between the **serve** and **build** tasks.

Below are the baisc npm scripts we run in parallel in order to reduce the command into a simple `npm start`

```js
"compile-server": "tsc -p ./src/server",

"server": "npm run compile-server && cross-env NODE_CONFIG_DIR=./src/server/config node dist/server/index.js",

"client": "ng serve --base-href=/ --proxy-config dev.proxy.conf.json -p 3000 --sourcemap=true --watch ",

"start": "cross-env NODE_PORT=1337 npm-run-all --parallel client server"
```

 I make use of the cross platform `cross-env` and `npm-run-all` libraries to support macs and window machines.


**Buliding for prod (Bundling and AOT)**

The below table explains the difference between `ng build --prod` and `ng serve` (aka `ng build`)
<table>
<thead>
<tr>
<th>Flag</th>
<th><code>--dev</code></th>
<th><code>--prod</code></th>
</tr>
</thead>
<tbody>
<tr>
<td><code>--aot</code></td>
<td><code>false</code></td>
<td><code>true</code></td>
</tr>
<tr>
<td><code>--environment</code></td>
<td><code>dev</code></td>
<td><code>prod</code></td>
</tr>
<tr>
<td><code>--output-hashing</code></td>
<td><code>media</code></td>
<td><code>all</code></td>
</tr>
<tr>
<td><code>--sourcemaps</code></td>
<td><code>true</code></td>
<td><code>false</code></td>
</tr>
<tr>
<td><code>--extract-css</code></td>
<td><code>false</code></td>
<td><code>true</code></td>
</tr>
</tbody>
</table>

Run `npm run start-build` in order to compile the client and server apps into the `dist` directory and run a node server for that directoy. This is actually how we will in a deploy (production) mode.

Below are the `package.json` scripts:

```js
"build": "ng build --prod --base-href=/app/ --watch",
"start-build": "cross-env NODE_ENV=dev NODE_PORT=3000 npm-run-all --parallel build server",
```
**Running Tests**

Use `ng test` or `ng test --single-run --browsers PhantomJS` for the build server and PhantomJS.

<a name="build-script"></a>
## [Build Script and Deployment Server](#build-script)

I won't go into great depth regarding our build script but I will include it for the big picture. We use this on our Bamboo build server. It works but is rather inefficient.

In the end we want a my-client-x-x-x.zip with the following:

   1. compiled client bundles
   2. compiled server code
   3. node_modules required for server app

```sh
#!/bin/sh

# get build number and branch provided in bamboo task
build=$1
branch=$2

echo "build $build and branch $branch"

. ~/.nvm/nvm.sh
nvm use 7.2.0

echo 'node -v'
node  -v

npm install -g npm@5.2.0 --no-progress

echo 'npm -v'
npm -v

echo 'npm install'
npm install --no-progress
echo 'npm install finished'

echo 'showing versions with npm ls'
npm ls

# Dump versions for logs
ng version

npm run clean

npm run compile-server
npm run compile-lib

npm run test-build-server

# remove current dist folder files
echo 'Removing current ./dist folder'
rm -rf ./dist
mkdir ./dist

rm -rf artifacts
mkdir -p artifacts

# Creates dist/app files
echo 'Running ng build --prod'
ng build --prod --progress=false
echo 'ng build finished'

echo 'optomize build images'
node ./devops/scripts/min-images.js

# Creates dist/server files. we have to compile server tsc separately
cd src/server

# run tsc from bin modules to control our tsc version with package.json
../../node_modules/.bin/tsc

# back to root proj directory
cd ../../

echo 'copy server config directory'
cp -rf ./src/server/config ./dist/server

echo 'Run devops/version.it.js to get version and bamboo build'
# Create version.json and bamboo vars text file
version=$(node devops/version-it.js $build $branch)

echo 'npm prune --production'
npm prune --production

# copy over to dist
cp -rf ./node_modules ./dist/node_modules

zipFileName=my-client-$version.zip
zip -r "./artifacts/$zipFileName" dist devops
echo "Created ZIP file for dist at ${zipFileName}"
```

<a name="questions"></a>
## [Questions and Next Steps](#questions)

- How to force devs to verify `ng build --prod` before committing?
- [How to use AOT compilation when using `ng test`?](https://stackoverflow.com/questions/45642545/how-to-run-angular-cli-ng-test-with-production-build-settings-and-aot-enabled)






