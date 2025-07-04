# @knobik/bpmn-js-properties-panel

Fork of bpmn-js-properties-panel, but includes a custom script editor and some love for camunda platform.

[![CI](https://github.com/bpmn-io/bpmn-js-properties-panel/workflows/CI/badge.svg)](https://github.com/bpmn-io/bpmn-js-properties-panel/actions?query=workflow%3ACI)

A properties panel extension for [bpmn-js](https://github.com/bpmn-io/bpmn-js) that adds the ability to edit technical properties (generic and [Camunda](https://camunda.com)).

[![bpmn-js-properties-panel screenshot](./docs/screenshot.png "Screenshot of the bpmn-js modeler with properties panel")](https://github.com/bpmn-io/bpmn-js-examples/tree/master/properties-panel)


## Features

The properties panel allows users to edit invisible BPMN properties in a convenient way.

Some of the features are:

* Edit element ids, multi-instance details and more
* Edit execution related [Camunda 7](https://docs.camunda.org) and [Camunda 8](https://docs.camunda.io/) properties
* Redo and undo (plugs into the [bpmn-js](https://github.com/bpmn-io/bpmn-js) editing cycle)

## Usage

Provide two HTML elements, one for the properties panel and one for the BPMN diagram:

```html
<div class="modeler">
  <div id="canvas"></div>
  <div id="properties"></div>
</div>
```

Bootstrap [bpmn-js](https://github.com/bpmn-io/bpmn-js) with the properties panel and a [properties provider](https://github.com/bpmn-io/bpmn-js-properties-panel/tree/master/src/provider):


```javascript
import BpmnModeler from 'bpmn-js/lib/Modeler';
import {
  BpmnPropertiesPanelModule,
  BpmnPropertiesProviderModule,
} from 'bpmn-js-properties-panel';

const modeler = new BpmnModeler({
  container: '#canvas',
  propertiesPanel: {
    parent: '#properties'
  },
  additionalModules: [
    BpmnPropertiesPanelModule,
    BpmnPropertiesProviderModule
  ]
});
```

### Styling

For proper styling include the necessary stylesheets:

```html
<link rel="stylesheet" href="https://unpkg.com/@bpmn-io/properties-panel/dist/assets/properties-panel.css">
```


### Dynamic Attach/Detach

You may attach or detach the properties panel dynamically to any element on the page, too:

```javascript
const propertiesPanel = bpmnJS.get('propertiesPanel');

// detach the panel
propertiesPanel.detach();

// attach it to some other element
propertiesPanel.attachTo('#other-properties');
```


### Edit Camunda Properties

To edit [Camunda](https://camunda.com) properties, you have to use a [moddle extension](https://github.com/bpmn-io/moddle) so bpmn-js is can read and write Camunda properties and use a provider so these properties are shown in the properties panel.

For example, to edit [Camunda 8](https://camunda.com/platform/) properties, use the [Camunda 8 moddle extension](https://github.com/camunda/zeebe-bpmn-moddle) and the [Camunda 8 provider](https://github.com/bpmn-io/bpmn-js-properties-panel/tree/master/src/provider/zeebe). Additionally, you should use [behaviors specific to Camunda 8](https://github.com/camunda/camunda-bpmn-js-behaviors?tab=readme-ov-file#camunda-8) to ensure parts of the model that are specific to Camunda 8 are maintained correctly.

```javascript
import BpmnModeler from 'bpmn-js/lib/Modeler';
import {
  BpmnPropertiesPanelModule,
  BpmnPropertiesProviderModule,
  ZeebePropertiesProviderModule // Camunda 8 provider
} from 'bpmn-js-properties-panel';

// Camunda 8 moddle extension
import zeebeModdle from 'zeebe-bpmn-moddle/resources/zeebe';

// Camunda 8 behaviors
import ZeebeBehaviorsModule from 'camunda-bpmn-js-behaviors/lib/camunda-cloud';

const modeler = new BpmnModeler({
  container: '#canvas',
  propertiesPanel: {
    parent: '#properties'
  },
  additionalModules: [
    BpmnPropertiesPanelModule,
    BpmnPropertiesProviderModule,
    ZeebePropertiesProviderModule,
    ZeebeBehaviorsModule
  ],
  moddleExtensions: {
    zeebe: zeebeModdle
  }
});
```

### API

#### `BpmnPropertiesPanelRenderer#attachTo(container: HTMLElement) => void`

Attach the properties panel to a parent node.

```javascript
const propertiesPanel = modeler.get('propertiesPanel');

propertiesPanel.attachTo('#other-properties');
```

#### `BpmnPropertiesPanelRenderer#detach() => void`

Detach the properties panel from its parent node.

```javascript
const propertiesPanel = modeler.get('propertiesPanel');

propertiesPanel.detach();
```

#### `BpmnPropertiesPanelRenderer#registerProvider(priority: Number, provider: PropertiesProvider) => void`

Register a new properties provider to the properties panel.

```javascript
class ExamplePropertiesProvider {
  constructor(propertiesPanel) {
    propertiesPanel.registerProvider(500, this);
  }

  getGroups(element) {
    return (groups) => {

      // add, modify or remove groups
      // ...

      return groups;
    };
  }
}

ExamplePropertiesProvider.$inject = [ 'propertiesPanel' ];
```

## Additional Resources

* [Issue tracker](https://github.com/bpmn-io/bpmn-js-properties-panel)
* [Forum](https://forum.bpmn.io)
* [Example Project](https://github.com/bpmn-io/bpmn-js-examples/tree/master/properties-panel)


## Development

Prepare the project by installing all dependencies:

```sh
npm install
```

Then, depending on your use-case, you may run any of the following commands:

```sh
# build the library and run all tests
npm run all

# spin up a single local modeler instance
npm start

# run the full development setup
npm run dev
```

## License

MIT
