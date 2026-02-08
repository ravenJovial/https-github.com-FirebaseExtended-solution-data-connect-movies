# https-github.com-FirebaseExtended-solution-data-connect-movies
solution with fire base 
https://studio.firebase.google.com/solution-17193156
 Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch main
# Your branch is up to date with 'origin/main'.
#
# Changes to be committed:
#	modified:   .firebaserc
#	new file:   generated/data/.guides/config.json
#	new file:   generated/data/.guides/setup.md
#	new file:   generated/data/.guides/usage.md
#	modified:   generated/data/README.md
#	modified:   generated/data/esm/index.esm.js
#	modified:   generated/data/index.cjs.js
#	modified:   generated/data/index.d.ts
#	modified:   generated/data/package.json
#	modified:   generated/data/react/README.md
#	modified:   generated/data/react/esm/index.esm.js
#	modified:   generated/data/react/index.cjs.js
#	modified:   generated/data/react/index.d.ts
#
{pkgs}: {
  channel = "stable-24.11";
  packages = [
    pkgs.nodejs_20
    pkgs.pnpm
    pkgs.jdk17
    pkgs.zip
  ];
  idx.extensions = [
    "GoogleCloudTools.firebase-dataconnect-vscode"
    "GraphQL.vscode-graphql-syntax"
  ];
  idx.workspace.onCreate = {
    extract-seed-data = "unzip seed-data.zip";
    install-dependencies = "pnpm install";
  };
  idx.workspace.onStart = {
    start-emulators = "npx -y firebase-tools@latest emulators:start";
  };
  idx.previews = {
    enable = true; 
    previews = {
      web = {
        command = [
          "pnpm"
          "run" 
          "dev:emulator" 
          "--port"
          "$PORT" 
        ];
        manager = "web";
      };
    };
  };
}
 "description": "A set of guides for interacting with the generated firebase dataconnect sdk",
  "mcpServers": {
    "firebase": {
      "command": "npx",
      "args": ["-y", "firebase-tools@latest", "experimental:mcp"]
    }
  }
}

SetUP
# Setup

If the user hasn't already installed the SDK, always run the user's node package manager of choice, and install the package in the directory ../package.json.
For more information on where the library is located, look at the connector.yaml file.

```ts
import { initializeApp } from 'firebase/app';

initializeApp({
  // fill in your project config here using the values from your Firebase project or from the `firebase_get_sdk_config` tool from the Firebase MCP server.
});
```

Then, you can run the SDK as needed.
```ts
import { ... } from '@app/data';
```




## React
### Setup

The user should make sure to install the `@tanstack/react-query` package, along with `@tanstack-query-firebase/react` and `firebase`.

Then, they should initialize Firebase:
```ts
import { initializeApp } from 'firebase/app';
initializeApp(firebaseConfig); /* your config here. To generate this, you can use the `firebase_sdk_config` MCP tool */
```

Then, they should add a `QueryClientProvider` to their root of their application.

Here's an example:

```ts
import { initializeApp } from 'firebase/app';
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const firebaseConfig = {
  /* your config here. To generate this, you can use the `firebase_sdk_config` MCP tool */
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);

// Create a TanStack Query client instance
const queryClient = new QueryClient();

function App() {
  return (
    // Provide the client to your App
    <QueryClientProvider client={queryClient}>
      <MyApplication />
    </QueryClientProvider>
  )
}

render(<App />, document.getElementById('root'));
```

# Basic Usage

Always prioritize using a supported framework over using the generated SDK
directly. Supported frameworks simplify the developer experience and help ensure
best practices are followed.




### React
For each operation, there is a wrapper hook that can be used to call the operation.

Here are all of the hooks that get generated:
```ts
import { useUpdateUser, useAddWatch, useAddReview, useDeleteWatch, useHomePage, useSearchMovies, useMoviePage, useWatchHistoryPage, useBrowseMovies, useGetMovies } from '@app/data/react';
// The types of these hooks are available in react/index.d.ts

const { data, isPending, isSuccess, isError, error } = useUpdateUser(updateUserVars);

const { data, isPending, isSuccess, isError, error } = useAddWatch(addWatchVars);

const { data, isPending, isSuccess, isError, error } = useAddReview(addReviewVars);

const { data, isPending, isSuccess, isError, error } = useDeleteWatch(deleteWatchVars);

const { data, isPending, isSuccess, isError, error } = useHomePage();

const { data, isPending, isSuccess, isError, error } = useSearchMovies(searchMoviesVars);

const { data, isPending, isSuccess, isError, error } = useMoviePage(moviePageVars);

const { data, isPending, isSuccess, isError, error } = useWatchHistoryPage(watchHistoryPageVars);

const { data, isPending, isSuccess, isError, error } = useBrowseMovies(browseMoviesVars);

const { data, isPending, isSuccess, isError, error } = useGetMovies(getMoviesVars);

```

Here's an example from a different generated SDK:

```ts
import { useListAllMovies } from '@dataconnect/generated/react';

function MyComponent() {
  const { isLoading, data, error } = useListAllMovies();
  if(isLoading) {
    return <div>Loading...</div>
  }
  if(error) {
    return <div> An Error Occurred: {error} </div>
  }
}

// App.tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import MyComponent from './my-component';

function App() {
  const queryClient = new QueryClient();
  return <QueryClientProvider client={queryClient}>
    <MyComponent />
  </QueryClientProvider>
}
```



## Advanced Usage
If a user is not using a supported framework, they can use the generated SDK directly.

Here's an example of how to use it with the first 5 operations:

```js
import { updateUser, addWatch, addReview, deleteWatch, homePage, searchMovies, moviePage, watchHistoryPage, browseMovies, getMovies } from '@app/data';


// Operation UpdateUser:  For variables, look at type UpdateUserVars in ../index.d.ts
const { data } = await UpdateUser(dataConnect, updateUserVars);

// Operation AddWatch:  For variables, look at type AddWatchVars in ../index.d.ts
const { data } = await AddWatch(dataConnect, addWatchVars);

// Operation AddReview:  For variables, look at type AddReviewVars in ../index.d.ts
const { data } = await AddReview(dataConnect, addReviewVars);

// Operation DeleteWatch:  For variables, look at type DeleteWatchVars in ../index.d.ts
const { data } = await DeleteWatch(dataConnect, deleteWatchVars);

// Operation HomePage: 
const { data } = await HomePage(dataConnect);

// Operation SearchMovies:  For variables, look at type SearchMoviesVars in ../index.d.ts
const { data } = await SearchMovies(dataConnect, searchMoviesVars);

// Operation MoviePage:  For variables, look at type MoviePageVars in ../index.d.ts
const { data } = await MoviePage(dataConnect, moviePageVars);

// Operation WatchHistoryPage:  For variables, look at type WatchHistoryPageVars in ../index.d.ts
const { data } = await WatchHistoryPage(dataConnect, watchHistoryPageVars);

// Operation BrowseMovies:  For variables, look at type BrowseMoviesVars in ../index.d.ts
const { data } = await BrowseMovies(dataConnect, browseMoviesVars);

// Operation GetMovies:  For variables, look at type GetMoviesVars in ../index.d.ts
const { data } = await GetMovies(dataConnect, getMoviesVars);


```
Index.esm.js
import { queryRef, executeQuery, mutationRef, executeMutation, validateArgs } from 'firebase/data-connect';

export const connectorConfig = {
  connector: 'connector',
  service: 'app',
  location: 'us-central1'
};

export const updateUserRef = (dcOrVars, vars) => {
  const { dc: dcInstance, vars: inputVars} = validateArgs(connectorConfig, dcOrVars, vars, true);
  dcInstance._useGeneratedSdk();
  return mutationRef(dcInstance, 'UpdateUser', inputVars);
}
updateUserRef.operationName = 'UpdateUser';

export function updateUser(dcOrVars, vars) {
  return executeMutation(updateUserRef(dcOrVars, vars));
}

export const addWatchRef = (dcOrVars, vars) => {
  const { dc: dcInstance, vars: inputVars} = validateArgs(connectorConfig, dcOrVars, vars, true);
  dcInstance._useGeneratedSdk();
  return mutationRef(dcInstance, 'AddWatch', inputVars);
}
addWatchRef.operationName = 'AddWatch';

export function addWatch(dcOrVars, vars) {
  return executeMutation(addWatchRef(dcOrVars, vars));
}

export const addReviewRef = (dcOrVars, vars) => {
  const { dc: dcInstance, vars: inputVars} = validateArgs(connectorConfig, dcOrVars, vars, true);
  dcInstance._useGeneratedSdk();
  return mutationRef(dcInstance, 'AddReview', inputVars);
}
addReviewRef.operationName = 'AddReview';

export function addReview(dcOrVars, vars) {
  return executeMutation(addReviewRef(dcOrVars, vars));
}

export const deleteWatchRef = (dcOrVars, vars) => {
  const { dc: dcInstance, vars: inputVars} = validateArgs(connectorConfig, dcOrVars, vars, true);
  dcInstance._useGeneratedSdk();
  return mutationRef(dcInstance, 'DeleteWatch', inputVars);
}
deleteWatchRef.operationName = 'DeleteWatch';

export function deleteWatch(dcOrVars, vars) {
  return executeMutation(deleteWatchRef(dcOrVars, vars));
}

export const homePageRef = (dc) => {
  const { dc: dcInstance} = validateArgs(connectorConfig, dc, undefined);
  dcInstance._useGeneratedSdk();
  return queryRef(dcInstance, 'HomePage');
}
homePageRef.operationName = 'HomePage';

export function homePage(dc) {
  return executeQuery(homePageRef(dc));
}

export const searchMoviesRef = (dcOrVars, vars) => {
  const { dc: dcInstance, vars: inputVars} = validateArgs(connectorConfig, dcOrVars, vars, true);
  dcInstance._useGeneratedSdk();
  return queryRef(dcInstance, 'SearchMovies', inputVars);
}
searchMoviesRef.operationName = 'SearchMovies';

export function searchMovies(dcOrVars, vars) {
  return executeQuery(searchMoviesRef(dcOrVars, vars));
}

export const moviePageRef = (dcOrVars, vars) => {
  const { dc: dcInstance, vars: inputVars} = validateArgs(connectorConfig, dcOrVars, vars, true);
  dcInstance._useGeneratedSdk();
  return queryRef(dcInstance, 'MoviePage', inputVars);
}
moviePageRef.operationName = 'MoviePage';

export function moviePage(dcOrVars, vars) {
  return executeQuery(moviePageRef(dcOrVars, vars));
}

export const watchHistoryPageRef = (dcOrVars, vars) => {
  const { dc: dcInstance, vars: inputVars} = validateArgs(connectorConfig, dcOrVars, vars);
  dcInstance._useGeneratedSdk();
  return queryRef(dcInstance, 'WatchHistoryPage', inputVars);
}
watchHistoryPageRef.operationName = 'WatchHistoryPage';

export function watchHistoryPage(dcOrVars, vars) {
  return executeQuery(watchHistoryPageRef(dcOrVars, vars));
}

export const browseMoviesRef = (dcOrVars, vars) => {
  const { dc: dcInstance, vars: inputVars} = validateArgs(connectorConfig, dcOrVars, vars);
  dcInstance._useGeneratedSdk();
  return queryRef(dcInstance, 'BrowseMovies', inputVars);
}
browseMoviesRef.operationName = 'BrowseMovies';

export function browseMovies(dcOrVars, vars) {
  return executeQuery(browseMoviesRef(dcOrVars, vars));
}

export const getMoviesRef = (dcOrVars, vars) => {
  const { dc: dcInstance, vars: inputVars} = validateArgs(connectorConfig, dcOrVars, vars);
  dcInstance._useGeneratedSdk();
  return queryRef(dcInstance, 'GetMovies', inputVars);
}
getMoviesRef.operationName = 'GetMovies';

export function getMovies(dcOrVars, vars) {
  return executeQuery(getMoviesRef(dcOrVars, vars));
}

export const detailedWatchHistoryRef = (dc) => {
  const { dc: dcInstance} = validateArgs(connectorConfig, dc, undefined);
  dcInstance._useGeneratedSdk();
  return queryRef(dcInstance, 'DetailedWatchHistory');
}
detailedWatchHistoryRef.operationName = 'DetailedWatchHistory';

export function detailedWatchHistory(dc) {
  return executeQuery(detailedWatchHistoryRef(dc));
}

.package.json

{"type":"module"}
INDEXesm.js
import { updateUserRef, addWatchRef, addReviewRef, deleteWatchRef, homePageRef, searchMoviesRef, moviePageRef, watchHistoryPageRef, browseMoviesRef, getMoviesRef, detailedWatchHistoryRef, connectorConfig } from '../../esm/index.esm.js';
import { validateArgs, CallerSdkTypeEnum } from 'firebase/data-connect';
import { useDataConnectQuery, useDataConnectMutation, validateReactArgs } from '@tanstack-query-firebase/react/data-connect';

export function useUpdateUser(dcOrOptions, options) {
  const { dc: dcInstance, vars: inputOpts } = validateArgs(connectorConfig, dcOrOptions, options);
  function refFactory(vars) {
    return updateUserRef(dcInstance, vars);
  }
  return useDataConnectMutation(refFactory, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}

export function useAddWatch(dcOrOptions, options) {
  const { dc: dcInstance, vars: inputOpts } = validateArgs(connectorConfig, dcOrOptions, options);
  function refFactory(vars) {
    return addWatchRef(dcInstance, vars);
  }
  return useDataConnectMutation(refFactory, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}

export function useAddReview(dcOrOptions, options) {
  const { dc: dcInstance, vars: inputOpts } = validateArgs(connectorConfig, dcOrOptions, options);
  function refFactory(vars) {
    return addReviewRef(dcInstance, vars);
  }
  return useDataConnectMutation(refFactory, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}

export function useDeleteWatch(dcOrOptions, options) {
  const { dc: dcInstance, vars: inputOpts } = validateArgs(connectorConfig, dcOrOptions, options);
  function refFactory(vars) {
    return deleteWatchRef(dcInstance, vars);
  }
  return useDataConnectMutation(refFactory, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}


export function useHomePage(dcOrOptions, options) {
  const { dc: dcInstance, options: inputOpts } = validateReactArgs(connectorConfig, dcOrOptions, options);
  const ref = homePageRef(dcInstance);
  return useDataConnectQuery(ref, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}

export function useSearchMovies(dcOrVars, varsOrOptions, options) {
  const { dc: dcInstance, vars: inputVars, options: inputOpts } = validateReactArgs(connectorConfig, dcOrVars, varsOrOptions, options, true, true);
  const ref = searchMoviesRef(dcInstance, inputVars);
  return useDataConnectQuery(ref, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}

export function useMoviePage(dcOrVars, varsOrOptions, options) {
  const { dc: dcInstance, vars: inputVars, options: inputOpts } = validateReactArgs(connectorConfig, dcOrVars, varsOrOptions, options, true, true);
  const ref = moviePageRef(dcInstance, inputVars);
  return useDataConnectQuery(ref, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}

export function useWatchHistoryPage(dcOrVars, varsOrOptions, options) {
  const { dc: dcInstance, vars: inputVars, options: inputOpts } = validateReactArgs(connectorConfig, dcOrVars, varsOrOptions, options, true, false);
  const ref = watchHistoryPageRef(dcInstance, inputVars);
  return useDataConnectQuery(ref, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}

export function useBrowseMovies(dcOrVars, varsOrOptions, options) {
  const { dc: dcInstance, vars: inputVars, options: inputOpts } = validateReactArgs(connectorConfig, dcOrVars, varsOrOptions, options, true, false);
  const ref = browseMoviesRef(dcInstance, inputVars);
  return useDataConnectQuery(ref, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}

export function useGetMovies(dcOrVars, varsOrOptions, options) {
  const { dc: dcInstance, vars: inputVars, options: inputOpts } = validateReactArgs(connectorConfig, dcOrVars, varsOrOptions, options, true, false);
  const ref = getMoviesRef(dcInstance, inputVars);
  return useDataConnectQuery(ref, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}

export function useDetailedWatchHistory(dcOrOptions, options) {
  const { dc: dcInstance, options: inputOpts } = validateReactArgs(connectorConfig, dcOrOptions, options);
  const ref = detailedWatchHistoryRef(dcInstance);
  return useDataConnectQuery(ref, inputOpts, CallerSdkTypeEnum.GeneratedReact);
}
