## aframe-look-at-component

Components for [A-Frame](https://aframe.io) to tell an entity to face towards
another entity or position (look-at), or face the active camera (billboard). Uses three.js's
[Object3D](http://threejs.org/docs/#Reference/Core/Object3D) `.lookAt()`

The look-at component defines the behavior for an entity to dynamically rotate
or face towards another entity or position. The rotation of the entity will be
updated on every tick. The `look-at` component takes a src parameter containing
either a vec3 position or a [query selector][mdn-queryselector] to another entity in it's src parameter, and flags to control it's runtime behavior.  The `checkSrcEveryFrame` flag controls whether the component re-evaluates the `query selector` each frame, useful if the result of the selector may change over time. This is false by default. The `updateWorldTransform` flag controls whether the component updates the `worldTransform` on the underlying `THREE.Object3D` each frame, or uses the already computed value. This is false by default, since the worst case is that the value from the previous frame will be used (which in many cases is not noticable), and extra updates may be computationally non-trivial (depending on the size of the scene graph).  However, if you want to guarantee the current, correct value is used, set this flag.

The billboard component behaves similarly to look-at, but always faces the entity toward the current camera.  It takes no parameters.

## Values 

The attribute values of the look-at component are as follows.

| Name |  Type     | Description                                                                                                                                   |
|-------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| src | selector | A query selector indicating another entity to track. If the other entity is moving then the `look-at` component will track the moving entity. |
| | vec3     | An XYZ coordinate. The entity will face towards a static position.    |
| checkSrcEveryFrame | boolean | whether to re-evaluate the src selector each frame |
| updateWorldTransform | boolean | whether to re-compute the underlying Object3D worldTransform |

### Usage

#### Browser Installation

Install and use by directly including the [browser files](dist):

```html
<head>
  <title>My A-Frame Scene</title>
  <script src="https://aframe.io/releases/0.2.0/aframe.min.js"></script>
  <script src="https://rawgit.com/blairmacintyre/aframe-look-at-billboard-component/master/dist/aframe-look-at-billboard-component.min.js"></script>
</head>

<body>
  <a-scene>
    <a-entity id="monster" geometry="primitive: box" material="src: url(monster.png)"
              billboard></a-entity>
    <a-entity id="player" camera></a-entity>

    <a-entity id="dog" geometry="primitive: box" material="src: url(dog.png)"
              look-at="src: #squirrel"></a-entity>
    <a-entity id="squirrel">
      <a-animation id="running" attribute="position" to="100 0 0"></a-animation>
    </a-entity>
  </a-scene>
</body>
```

[mdn-queryselector]: https://developer.mozilla.org/docs/Web/API/Document/querySelector
