# Babylon.Font

Library

- Compile glyph to shapes using WebAssembly.
- Build mesh from shapes.

Tool - [Gen][1]

- Dump mesh geometry to json.
- Screenshot to png.

[1]: https://ycw.github.io/Babylon.Font/app/gen/



# Usage

```html
<script src='babylon.js'></script>
<script src='earcut.js'></script>
<script src='opentype.js'></script>
<script src='babylon.font.js'></script>
<script>
(async function() {

  // Create BabylonJS environment
  const scene = ..;
  
  // Build compiler
  const compiler = await BF.Compiler.Build('compile_wa.wasm');
  
  // Install font(s)
  const font = await BF.Font.Install('a.ttf', compiler); 

  // Build mesh for single character
  const fontSize = 1;
  const ppc = 0;
  const eps = 0;
  const shapes = Font.Compile(font, 'c', fontSize, ppc, eps);

  const depth = 1;
  const sideOrientation = BABYLON.Mesh.DOUBLESIDE;
  const mesh = Font.BuildMesh(shapes, { depth, sideOrientation }, scene);

  // Measure a character
  const metrics = Font.Measure(font, 'c', fontSize);
  metrics.advanceWidth;
  metrics.ascender;
  metrics.descender;
})();
</script>
```



# API
(TODO) 