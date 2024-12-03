# CS405-Project2


---

#### Project Overview

This project involves enhancing the functionality of a WebGL-based 3D rendering application. Specifically, the tasks focus on texture handling and lighting implementation. All modifications are made in the `project2.js` file.

---

### Task 1: Non-Power-of-Two Textures

#### Objective:
Modify the `setTexture` function to allow non-power-of-two (NPOT) textures to be used in rendering.

#### Implementation Steps:
1. **Texture Initialization**:
   - Bind the texture using `gl.bindTexture(gl.TEXTURE_2D, texture);`.
   - Set the texture image data using `gl.texImage2D`.

2. **Texture Parameter Updates**:
   - For NPOT textures:
     - Set the wrapping mode to `CLAMP_TO_EDGE` using:
       ```javascript
       gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_S, gl.CLAMP_TO_EDGE);
       gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_T, gl.CLAMP_TO_EDGE);
       ```
     - Set the filtering to `LINEAR`:
       ```javascript
       gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.LINEAR);
       gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.LINEAR);
       ```

3. **Testing**:
   - Use `leaves.jpg` from the resources folder to test the implementation.

#### Expected Outcome:
The application should successfully render NPOT textures without errors, ensuring broader texture compatibility.

---

### Task 2: Lighting Implementation


#### Implementation Steps:

1. **Constructor Modifications**:
   - Initialize uniforms for:
     - Light position (`lightPosLoc`).
     - Ambient light intensity (`ambientLoc`).
     - Lighting toggle (`enableLightingLoc`).

2. **`setMesh` Method Updates**:
   - Handle normal vectors for lighting calculations:
     ```javascript
     gl.bindBuffer(gl.ARRAY_BUFFER, this.normBuffer);
     gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(normalCoords), gl.STATIC_DRAW);
     ```

3. **`draw` Method Updates**:
   - Pass lighting uniforms to the shader:
     ```javascript
     gl.uniform3f(this.lightPosLoc, lightX, lightY, 1.0);
     gl.uniform1f(this.ambientLoc, this.ambient);
     gl.uniform1i(this.enableLightingLoc, this.enableLight ? 1 : 0);
     ```

4. **Fragment Shader Enhancements**:
   - Add lighting calculations:
     ```glsl
     vec3 normalizedNormal = normalize(v_normal);
     vec3 normalizedLightDir = normalize(lightPos);
     float diffuse = max(dot(normalizedNormal, normalizedLightDir), 0.0);
     vec4 diffuseColor = diffuse * vec4(1.0, 1.0, 1.0, 1.0);
     vec4 ambientLight = ambient * vec4(1.0, 1.0, 1.0, 1.0);
     vec4 finalColor = diffuseColor + ambientLight;
     gl_FragColor = finalColor * texture2D(tex, v_texCoord);
     ```

5. **Lighting Control**:
   - Implement `enableLighting` and `setAmbientLight` methods for dynamic lighting control via user interface sliders.

6. **Dynamic Light Movement**:
   - Allow users to reposition the light source using arrow keys.
  
   
The scene should exhibit realistic lighting effects with adjustable ambient intensity. Users should observe changes in light location and density dynamically.

### How to Run the Project

1. Open `project2.html` in a browser.
2. Upload a 3D model and texture file.
3. Test:
   - Non-power-of-two textures using `leaves.jpg`.
   - Lighting effects by interacting with the ambient light slider and arrow keys.

### Notes

- All modifications are in the `project2.js` file.
- Tasks 3 and 4 are not included.


