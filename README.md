# Label Studio Frontend &middot; ![GitHub](https://img.shields.io/github/license/heartexlabs/label-studio?logo=heartex) ![build](https://github.com/heartexlabs/label-studio-frontend/workflows/Build%20and%20Test/badge.svg) ![npm audit](https://github.com/heartexlabs/label-studio-frontend/actions/workflows/npm_audit.yml/badge.svg)
 ![GitHub release](https://img.shields.io/github/v/release/heartexlabs/label-studio-frontend?include_prereleases) &middot; :sunny:

[Website](https://labelstud.io/) • [Docs](https://labelstud.io/guide) • [Twitter](https://twitter.com/heartexlabs) • [Join Slack Community <img src="https://go.heartex.net/docs/images/slack-mini.png" width="18px"/>](https://slack.labelstudio.heartex.com)

<br/>

**Label Studio is an open-source, configurable data annotation tool. :v:**

Frontend, as its name suggests, is the frontend library developed using React and mobx-state-tree, distributed as an NPM package. You can include it in your applications and provide data annotation support to your users. It can be granularly customized and extended.

<br/>



## Development

DISCLAIMER: This project was forked from the [original one](https://github.com/heartexlabs/label-studio-frontend).

1. Clone the repository
   ```bash
   git clone https://github.com/fraunhoferkiku98/label-studio-frontend.git
   cd label-studio-frontend
   ```

2. Install required dependencies
   ```bash
   yarn install
   ```

3. Start the development server
   ```bash
   yarn start
   ```

4. Check different ways to initiate the development server config & task data in `src/env/development.js`, changing the `data` variable is a good place to start.

5. After you make changes and ready to use it in production, you need to create a production build
   ```bash
   npm run build-bundle
   ```
   Now you have one .js file and one .css file in the `build/static/` directory

<br/>

## Creating a new component (follwing [this](https://www.youtube.com/watch?v=TEtPUjBlM2M) webinar)

### General interesting informations on the framework

- In index.html the application will get initialized on domReady and with the function ```var ls = new LabelStudio("label-studio", lsfConfig);``` and the div ref ```<div id="label-studio"></div>``` it can be included in every project. Also you need to setup the event listeners ```ls.on("storageInitialized", (store) => ...)``` to enable updating annotations, drawing regions and storage initialization. Css files can be imported normally in the css files.
- in src/env/development.js you can choose the element that you want to have rendered in your dev environment (e.g. ```const data = LayoutHorizontal;```)
- src/examples is just a copy of /examples (it will be copied on npm start). The folder src/examples can be used for development. Be careful: it is also gitignored. 
- The frontend uses Webpack as a module bundler, to enable hot module replacement and module bundling. It puts all of your assets, including Javascript, images, fonts, and CSS in a dependency graph. Webpack is used to run and build the app. It also provides dead asset elimination, so you only build elements into your dist/ folder that your application actually needs. Furthermore it provides easier code splitting. 

### Process to create a new component
TODO: genaue anleitung aus notizen (label-studio setup.txt)



<br/>

## Usage

**With Webpack**

```js
import LabelStudio from '@heartexlabs/label-studio';
import 'label-studio/build/static/css/main.css';
```

**With UNPKG.com**

```xhtml
<!-- Include Label Studio stylesheet -->
<link href="https://unpkg.com/@heartexlabs/label-studio@1.8.0/build/static/css/main.css" rel="stylesheet">

<!-- Create the Label Studio container -->
<div id="label-studio"></div>

<!-- Include the Label Studio library -->
<script src="https://unpkg.com/@heartexlabs/label-studio@1.8.0/build/static/js/main.js"></script>
```

**Initialization**

```xhtml
<!-- Initialize Label Studio -->
<script>
  var labelStudio = new LabelStudio('label-studio', {
    config: `
      <View>
        <Image name="img" value="$image"></Image>
        <RectangleLabels name="tag" toName="img">
          <Label value="Hello"></Label>
          <Label value="World"></Label>
        </RectangleLabels>
      </View>
    `,

    interfaces: [
      "panel",
      "update",
      "submit",
      "controls",
      "side-column",
      "annotations:menu",
      "annotations:add-new",
      "annotations:delete",
      "predictions:menu",
    ],

    user: {
      pk: 1,
      firstName: "James",
      lastName: "Dean"
    },

    task: {
      annotations: [],
      predictions: [],
      id: 1,
      data: {
        image: "https://htx-misc.s3.amazonaws.com/opensource/label-studio/examples/images/nick-owuor-astro-nic-visuals-wDifg5xc9Z4-unsplash.jpg"
      }
    },

    onLabelStudioLoad: function(LS) {
      var c = LS.annotationStore.addAnnotation({
        userGenerate: true
      });
      LS.annotationStore.selectAnnotation(c.id);
    }
  });
</script>
```
## Label Studio for Teams, Startups, and Enterprises :office:

Label Studio for Teams is our enterprise edition (cloud & on-prem), that includes a data manager, high-quality baseline models, active learning, collaborators support, and more. Please visit the [website](https://www.heartex.com/) to learn more.

## Ecosystem

| Project | Description |
|-|-|
| [label-studio](https://github.com/heartexlabs/label-studio) | Server part, distributed as a pip package |
| label-studio-frontend | Frontend part, written in JavaScript and React, can be embedded into your application |
| [label-studio-converter](https://github.com/heartexlabs/label-studio-converter) | Encode labels into the format of your favorite machine learning library |
| [label-studio-transformers](https://github.com/heartexlabs/label-studio-transformers) | Transformers library connected and configured for use with label studio |

## License

This software is licensed under the [Apache 2.0 LICENSE](/LICENSE) © [Heartex](https://www.heartex.com/). 2020

<img src="https://github.com/heartexlabs/label-studio/blob/master/images/opossum_looking.png?raw=true" title="Hey everyone!" height="140" width="140" />
