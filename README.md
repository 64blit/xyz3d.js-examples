# xyz3d.js examples

## How To Run

Use any development server in one of the following subdirectories, such as the vscode extension [FiveServer](https://github.com/yandeu/five-server).
For example:
`python -m http.server 8000`
or
`nodemon --ext html,js,glb,json --verbose --exec http-server -p 8081`


see https://glitch.com/@64blit for examples running live



## Getting Start

[xyz3d.js-examples](https://github.com/64blit/xyz3d.js-examples)

The following is the minimum requirements for running an xyz3d framework instance.

1. A canvas and dom element for the popup html content. This is presented as a child iframe under this div. 

```html
<canvas id="main-canvas" class="fullscreen-canvas"></canvas>
<div id="popup-content" class="XYZ3d-hidden"></div>
```

2. CSS

```css

/*   The iframe which is dynamically added and parented 
to the popup-content div */

iframe #XYZ3d-fullscreen {
  position: absolute;
  width: 100%;
  height: 100%;
  border: none;
}

/* A dynamic button, used to close the popup, is created and 
parented to the popup-content div as follows: 
`<div id="XYZ3d-close-btn" class="XYZ3d-close-btn"><span>✕</span></div>` */
.XYZ3d-close-btn {
  color: #fff;
  font-size: 1.4rem;
  text-align: center;
  position: fixed;
  background-color: rgb(31, 31, 31);
  width: 5rem;
  height: 4rem;
  right: 0rem;
  top: 0rem;
  z-index: 2;
}

.XYZ3d-close-btn span {
  position: absolute;
  bottom: 50%;
  right: 50%;
  transform: translate(50%, 50%);
}

.XYZ3d-close-btn:hover {
  cursor: pointer;
  transition: background-color 100ms linear;
  background-color: #898989;
}

/* The hide and show selectors for the popup dom element */
.XYZ3d-visible {
  z-index: 2 !important;
  opacity: 1;
  transition: all 400ms linear;
  visibility: visible;
}

.XYZ3d-hidden {
  z-index: -2 !important;
  overflow: hidden;
  opacity: 0;
  transition: all 20ms 0ms;
}
```

3. Add a json file:

```json
{
  "models": [
    {
      "name": "bio template",
      "path": "assets/scene.glb",
      "enabled": true,
      "position": {
        "x": 0,
        "y": 0,
        "z": 0
      },
      "scale": {
        "x": 1,
        "y": 1,
        "z": 1
      },
      "rotation": {
        "x": 0,
        "y": 0,
        "z": 0,
        "w": 0
      },
      "interactable": [
        {
          "type": "link",
          "modelName": "Icon_IG",
          "content": "https://www.instagram.com"
        },
        {
          "type": "popup",
          "modelName": "Button_1_Mesh",
          "content": "pages/untree.co-minimal/index.html"
        }
      ],
      "shadow": false,
      "shadowBias": 0.005,
      "shadowNormalBias": 0.01,
      "shadowRadius": 1.0,
      "frustumCulled": true
    }
  ],
  "lights": [
    {
      "id": "ambient light 1",
      "type": "ambientLight",
      "enabled": true,
      "color": "#FFFFFF",
      "intensity": 1.0
    }
  ]
}
```

4. Add a GLB file created with the proper custom properties added via the blender plugin. See [xyz3d.js-blender-plugin](https://github.com/64blit/xyz3d.js-blender-plugin)

5. Create the xyz3d typescript instance, linking the proper dom elements.

```js
import { THREE, XYZ3d } from './xyz3d.es.js'

const xyzed = new XYZ3d({
  jsonPath: 'scene.json',
  domElements: {
    canvas: document.getElementById('main-canvas'),
    popup: document.getElementById('popup-content')
  }
})

let threeComponents = {
  camera: null,
  scene: null,
  renderer: null,
  sceneWrapper: null
}

xyzed.setup().then(result => {
  threeComponents = result
})
```
