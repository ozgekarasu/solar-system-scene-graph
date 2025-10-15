# Solar System Scene Graph & Illumination

This project was developed as part of **CS 405 – Computer Graphics** coursework.  
It demonstrates the implementation of a **scene graph hierarchy** and **Phong illumination model** using WebGL to simulate a simple **solar system** with the Sun, Earth, Moon, and Mars.

---

## Objectives

1. Implement hierarchical transformations in a **scene graph**.
2. Extend the fragment shader to include **diffuse** and **specular** lighting.
3. Add a new celestial body (Mars) to the solar system using the scene graph structure.

---

## Project Structure

| File | Description |
|------|--------------|
| `CameraController.js` | Calculates view (lookAt) matrices for the camera. |
| `LightingAndMeshRenderer.js` | Handles mesh rendering and fragment shader logic (ambient, diffuse, and specular light). |
| `MeshParser.js` | Helper class for parsing `.obj` files. |
| `ProjectionMatrix.js` | Computes the projection (perspective) matrix. |
| `SceneGraphNode.js` | Implements scene graph nodes and recursive transformation propagation. |
| `SphereGeometry.js` | Defines the sphere mesh used for all celestial bodies. |
| `Transformations.js` | Handles translation, rotation, and scaling (TRS transformations). |
| `MathUtils.js` | Provides general mathematical utilities such as matrix multiplication and normalization. |
| `SolarSystemSimulation.html` | Entry point of the project. Initializes WebGL, loads textures, sets up the scene graph, and manages the render loop. |
| `SceneGraph_Report.docx` | Detailed project report explaining implementation methodology and results. |
| `Assignment_Instructions.pdf` | Original project requirements and guidelines. |

---

## Implementation Overview

### **Task 1 – Scene Graph (Transformation Hierarchy)**
Implemented the `draw()` function inside `SceneGraphNode.js` to correctly propagate transformations from parent to child nodes.  
Each node applies its own TRS matrix and recursively calls its children's draw functions.

```js
draw(mvp, modelView, normalMatrix, modelMatrix) {
    var transformedMvp = MatrixMult(mvp, this.trs.getTransformationMatrix());
    var transformedModelView = MatrixMult(modelView, this.trs.getTransformationMatrix());
    var transformedNormals = getNormalMatrix(transformedModelView);
    var transformedModel = MatrixMult(transformedMvp, this.trs.getTransformationMatrix());

    if (this.meshDrawer) {
        this.meshDrawer.draw(transformedMvp, transformedModelView, transformedNormals, transformedModel);
    }

    for (var i = 0; i < this.children.length; i++) {
        this.children[i].draw(transformedMvp, transformedModelView, transformedNormals, transformedModel);
    }
}
