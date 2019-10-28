# Babylon.Font
Create text in BabylonJS

# Usage

```html
<script src='babylon.js'></script>
<script src='earcut.js'></script>
<script src='opentype.js'></script>
<script src='font.js'></script>
<script>
  
  // Install compiler
  const compiler = await Font.InstallCompiler('compile_wa.wasm');
  
  // Install font(s)
  const font = await Font.InstallFont('x.ttf', compiler);

  // Create Char
  const char = font.char('B', 1, 10, 0.003);

  // Create TransformNode 
  char.node({ depth: 1 });

</script>
```



# Examples

https://ycw.github.io/Babylon.Font/

(source: https://github.com/ycw/Babylon.Font/tree/gh-pages) 



# Font 

```js
const compiler = await Font.InstallCompiler('compile_wa.wasm');

// The only way to create a Font{} 
const font = await Font.InstallFont('x.ttf', compiler);
```

It has two methods 

```js
// 1. Create Char{} (see Char section)
const char = font.char(ch, sz, ppc, eps)

// Then use it to create a TransformNode{} (see Node section) 
const node = char.node()

// 2. Create ShapeXZ[] (see ShapeXZ section) 
const shapes = font.compile(ch, sz, ppc, eps) 
```


# Char

```js
const char = font.char(
  ch,  // char
  sz,  // fontsize
  ppc, // number of points used to interp for single bezier curve
  eps  // threshold used to dedup nearby vertices, e.g. 0.001 
);
```

It exposes useful properties for layout. All of them are normalized
and proportional to font size. 

```js
char.ascender       // number; top to baseline
char.descender      // number; baseline to bottom; negative
char.advanceWidth   // number; +L/R bearings
char.sTypoAscender  // number; top to CJK_baseline
char.sTypoDescender // number; CJK_baseline to bottom
```
 


# Node 

A helper - creating a `TransformNode{}` wrapping a char mesh (if any)   

```js
const char = font.char(..);
const node = char.node(
  opt,            // optional; used as MeshBuilder.CreatePolygon() option
  scene,          // optional; 
  isPivotAtOrigin // optional; centering the char mesh; default false;
);                // -> TransformNode{}

```



# OpentypeJS Font 

```js
const font = await Font.InstallFont(..);
font.otFont    // opentype.Font{}
```



# ShapeXZ 

```js
const shapesXZ = font.compile(ch, sz, ppc, eps);

shapesXZ
//- is array of {fill, holes}
//- where fill  : Vector3[]     # on XZplane
//-       holes : Vector3[][]   # on XZplane 
```

Note: 
- it may contain 0 shapes, e.g. space
- it may contain more than 1 shapes, e.g. "i" 
- a shape may contain more than 1 holes, e.g. "8" 
