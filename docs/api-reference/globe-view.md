# GlobeView Class (Experimental)

The [`GlobeView`] class is a subclass of [View](/docs/api-reference/view.md). This view projects the earth into a 3D globe.

It's recommended that you read the [Views and Projections guide](/docs/developer-guide/views.md) before using this class.

## Limitations

> This class is experimental, which means it does not provide the compatibility and stability that one would typically expect from other `View` classes. Use with caution and report any issues that you find on GitHub.

The goal of `GlobeView` is to provide a generic solution to rendering and navigating data in the 3D space.
In the initial release, this class mainly addresses the need to render an overview of the entire globe. The following limitations apply, as features are still under development: 

- No support for rotation (`pitch` or `bearing`). The camera always points towards the center of the earth, with north up.
- No high-precision rendering at high zoom levels (> 12). Features at the city-block scale may not be rendered accurately.
- Only supports `COORDINATE_SYSTEM.LNGLAT`.
- These layers currently do not work in this view:
  + Aggregation layers: `HeatmapLayer`, `ContourLayer`
  + Tiled layers: `TerrainLayer`, `MVTLayer`


## Constructor

```js
import {_GlobeView as GlobeView} from '@deck.gl/core';
const view = new GlobeView({id, ...});
```

`GlobeView` takes the same parameters as the [View](/docs/api-reference/view.md) superclass constructor.

## View State

To render, `GlobeView` needs to be used together with a `viewState` with the following parameters:

- `longitude` (`Number`) - longitude at the viewport center
- `latitude` (`Number`) - latitude at the viewport center
- `zoom` (`Number`) - zoom level
- `maxZoom` (`Number`, optional) - max zoom level. Default `20`.
- `minZoom` (`Number`, optional) - min zoom level. Default `0`.
- `resolution` (`Number`, optional) - the resolution at which to turn flat features into 3D meshes, in degrees. Default `10`.


## GlobeController

By default, `GlobeView` uses the `GlobeController` to handle interactivity. To enable the controller, use:

```js
const view = new GlobeView({id: 'globe', controller: true});
```

`GlobeController` supports the following interactions:

- `dragPan`: Drag to pan
- `scrollZoom`: Mouse wheel to zoom
- `doubleClickZoom`: Double click to zoom in, with shift/ctrl down to zoom out
- `touchZoom`: Pinch zoom
- `keyboard`: Keyboard (arrow keys to pan, +/- to zoom)

You can further customize its behavior by extending the class:

```js
import {_GlobeController as GlobeController} from '@deck.gl/core';

class MyGlobeController extends GlobeController {}

const view = new GlobeView({id: 'globe', controller: MyGlobeController});
```

See the documentation of [Controller](/docs/api-reference/controller.md) for implementation details.


## Source

[modules/core/src/views/globe-view.js](https://github.com/visgl/deck.gl/blob/master/modules/core/src/views/globe-view.js)
