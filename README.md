# Vitessce Setup

## Setup React app

1. Create the base react app
  
```bash
npm init react-app shrestha_jci2021 --use-npm
npm install vitessce --save
```

2. In the react app, change the `src/App.js` to return the Vitessce component

```js
import React from 'react';
import { Vitessce } from 'vitessce';
import { myViewConfig } from './my-view-config';

export default function App() {
  return (
    <Vitessce
      config={myViewConfig}
      theme="light"
    />
  );
}
```

3. Create a config file ->  `src/my-view-config.js` file
    - The `url` section points to a file hosted in AWS S3

```js
export const myViewConfig = {
    "version": "1.0.15", 
    "name": "Shrestha JCI 2021 Test Viewer", 
    "description": "", 
    "datasets": [
        {
            "uid": "A", 
            "name": "Shrestha JCI 2021", 
            "files": [
                {
                    "fileType": "anndata.zarr", 
                    "url": "https://cds-pancreatlas-public.s3.amazonaws.com/Shrestha_jci2021_new.zarr/", 
                    "options": {
                        "obsEmbedding": [
                            {
                                "path": "obsm/X_umap", 
                                "dims": [0, 1], 
                                "embeddingType": "UMAP"
                            }, 
                            {
                                "path": "obsm/X_pca", 
                                "dims": [0, 1],
                                "embeddingType": "PCA"
                            }
                        ], 
                        "obsSets": [
                            {
                                "name": "CellTypes", 
                                "path": "obs/CellTypes"
                            }
                        ],
                        "obsFeatureMatrix": {
                            "path": "X"
                        }
                    }
                }
            ]
        }
    ], 
    "coordinationSpace": {
        "dataset": {"A": "A"}, 
        "embeddingType": {"A": "UMAP"}
    }, 
    "layout": [
        {
            "component": "scatterplot", 
            "coordinationScopes": {
                "dataset": "A", 
                "embeddingType": "A"
            }, 
            "x": 0.0, 
            "y": 0.0, 
            "w": 8.0, 
            "h": 8.0
        }, 
        {
            "component": "obsSets", 
            "coordinationScopes": {
                "dataset": "A"
            }, 
            "x": 8.0, 
            "y": 0.0, 
            "w": 4.0, 
            "h": 4.0
        }, 
        {
            "component": "featureList", 
            "coordinationScopes": {
                "dataset": "A"
            }, 
            "x": 8.0, 
            "y": 4.0, 
            "w": 4.0, 
            "h": 4.0
        }, 
        {
            "component": "obsSetFeatureValueDistribution", 
            "coordinationScopes": {
                "dataset": "A"
            }, 
            "x": 6.0, 
            "y": 8.0, 
            "w": 6.0, 
            "h": 4.0
        }, 
        {
            "component": "obsSetSizes", 
            "coordinationScopes": {
                "dataset": "A"
            }, 
            "x": 0.0, 
            "y": 8.0, 
            "w": 6.0, 
            "h": 4.0
        }
    ], 
    "initStrategy": "auto"
}
```

1. Install the gh-pages npm package

```bash
npm install gh-pages --save-dev
```

5. Update the homepage info in `src/package.json`
 
```json
// this is just an example
{
  "name": "shrestha_jci2021", 
  "homepage": "https://vandydata.github.io/vitessce", //change this
  ...
}
```

6. Modify `src/index.css` to include information Vitessce uses for positioning modules.

```css
html {
  height: 100%;
}

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
  flex: 1;
  height: 100%;
  margin: 0;
}

.vitessce-container {
  height: max(100%, 100vh);
  width: 100%;
}
```

7. In `src/package.sjon`, add pre-deploy and deploy to the scripts:

```json
{
    ...
    "scripts": {
        "predeploy": "npm run build",
        "deploy": "gh-pages -d build",
        "start": "react-scripts start",
        "build": "react-scripts build",
        "test": "react-scripts test",
        "eject": "react-scripts eject"
    },
    ...
}
```

8. Deploy to github pages
    - This step takes a few minutes
```bash
# example
npm run deploy -- -m "Uploading Shrestha JCI2021 complete dataset in Vitessce viewer to Github pages"
```

9. Configure Github Pages
   - Go to the Github repo in the browser
   - Click on `Settings`
   - In the `Code and automation` section, click on `Pages`
   - Configure the `Build and deployment` settings like this:
     - Source: Deploy from a branch
     - Branch:
       - Branch: `gh-pages`
       - Folder `/ (root)`
   - Click on the `Save` button.