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


## General interesting informations on the framework

- In index.html the application will get initialized on domReady and with the function ```var ls = new LabelStudio("label-studio", lsfConfig);``` and the div ref ```<div id="label-studio"></div>```. It can be included in every project. Also you need to setup the event listeners ```ls.on("storageInitialized", (store) => ...)``` to enable updating annotations, drawing regions and storage initialization. Css files can be imported as usual.
- In src/env/development.js you can choose the element that you want to have rendered in your dev environment (e.g. ```const data = LayoutHorizontal;```)
- src/examples is just a copy of /examples (it will be copied on npm start). The folder src/examples can be used for development. Be careful: it is also gitignored. 
- The frontend uses Webpack as a module bundler, to enable hot module replacement and module bundling. It puts all of your assets, including Javascript, images, fonts, and CSS in a dependency graph. Webpack is used to run and build the app. It also provides dead asset elimination, so you only build elements into your dist folder that your application actually needs. Furthermore it provides easier code splitting. 
- The application uses mobx-state-tree for application state management and its overserver functions that automatically makes the wrapped components reactive to changes in its MobX observable values. These changes can be identified more fine-grained than with Reacts native approach, that can become cumbersome in deeply nested objects.
- In this stack MobX simplifies state management and reactive updates, while React efficiently renders the UI based on the changes detected by MobX. 

## Creating a new component (following [this](https://www.youtube.com/watch?v=TEtPUjBlM2M) webinar)

1. Go to src\tags\visual and create a new JS file.
1. Stick in your new implementation to the other tag files, that are in this folder (in this case we will stick to Style.js and copy it into our new file Layout.js)
1. Rename the const Model to LayoutModel, and replace it as well in all other properties. To have a more closer look at all the things that you have to change, you can checkout to the branch ```feature/youtube_tutorial``` and look at the files. You can have a look at the code at ```src\tags\visual\Layout.js```. In the HtxStyle you will return the HTML-code you want to render, when you will later use the XML Tags in your example specific config.xml files.
1. In ```src\tags\visual\index.js``` import and export your newly created LayoutModel
1. In src/examples you can copy a simple example folder, like /image_bbox and rename it to the new examples name (in this case: layout_horizontal)
1. In the copied folder open index.js open ```src\examples\layout_horizontal\index.js``` and export ```LayoutHorizontal = { config, tasks, annotation };```
1. In ```src\examples\layout_horizontal\config.xml``` insert your new Component as an XML-tag: ```<Layout>...</Layout>```
1. Go to file ```src\env\development.js```, add your new component (e.g. ```import { LayoutHorizontal } from "../examples/layout_horizontal"```) and set ```const data = LayoutHorizontal;``` to have it as an preview in development mode

Optional:

8. As we want to have our children rendered in our new component Layout we'll have to import Tree and Types from ```src\core``` to our file ```src\tags\visual\Layout.js```. We can now use these imported functionality to add the property children to the LayoutModel, which will ensure that all its provided child types will be rendered.
9. Also we can add a new porperty ```layout```, which will later define our CSS-styles that are used for our layout component. As a fallback for the layout property we use the horizontal config. To use a normal div-like behaving container one could use the component View.
10. In the end in HtxStyle we will render a div with the choosen style and inside it render its children using ```Tree.renderChildren(item, item.annotation)```

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
