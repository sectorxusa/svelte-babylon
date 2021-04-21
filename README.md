# svelte-babylon

Use BabylonJS like A-Frame through reactive Svelte Components.

## demo

https://svelte.dev/repl/3aa641515a99443c9f1036712f62fe90?version=3.37.0

## code example

```
<BabylonEngine>
  <BabylonScene>
    <BabylonCamera position={new BABYLON.Vector3(0, 5, -10)} target={BABYLON.Vector3.Zero()} />
    <BabylonHemisphericLight direction={new BABYLON.Vector3(0, 1, 0)} intensity={0.7} />
    <!-- Below is two-way binding between spherePosition and position -->
    <BabylonSphere bind:position={spherePosition} options={{ diameter: 2, segments: 32 }} />
    <BabylonGround options={{ width: 6, height: 6 }} />
  </BabylonScene>
</BabylonEngine>
```

## development

[![Contributors](https://badgen.net/github/contributors/sectorxusa/svelte-babylon)](https://github.com/SectorXUSA/svelte-babylon/graphs/contributors)
![GitHub repo size](https://img.shields.io/github/repo-size/SectorXUSA/svelte-babylon)
[![devDependencies Status](https://status.david-dm.org/gh/sectorxusa/svelte-babylon.svg?type=dev)](https://david-dm.org/sectorxusa/svelte-babylon?type=dev)
[![Total Alerts](https://img.shields.io/lgtm/alerts/g/SectorXUSA/svelte-babylon.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/SectorXUSA/svelte-babylon/alerts/)
[![Language Grade: JavaScript](https://img.shields.io/lgtm/grade/javascript/g/SectorXUSA/svelte-babylon.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/SectorXUSA/svelte-babylon/context:javascript)
[![Maintainability](https://api.codeclimate.com/v1/badges/1317568f8b75107655d6/maintainability)](https://codeclimate.com/github/SectorXUSA/svelte-babylon/maintainability)
[![Netlify Status](https://api.netlify.com/api/v1/badges/fad4c0b9-f89e-4f0a-be93-26007e8b2fe5/deploy-status)](https://app.netlify.com/sites/sx-svelte-babylon/deploys)

1. `$ npm i`
2. `$ npm run dev`
3. http://localhost:3000/
