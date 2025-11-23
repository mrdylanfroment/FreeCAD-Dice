# FreeCAD Face Numbering Demo - Dice Example

A FreeCAD macro that demonstrates how to programmatically reference and work with faces by creating a dice with numbered faces.

## Purpose

This macro is a learning example that shows how to:
- Reference specific faces on a 3D object
- Attach sketches to faces programmatically
- Center sketches on faces using geometric analysis
- Understand FreeCAD's face numbering system
- Work with face properties (center of mass, normals, local coordinate systems)

## What It Does

The macro creates a 20mm cube (dice) centered at the origin with a sketch on each of the 6 faces. Each sketch displays a dot pattern (1-6 dots) to help visualize which face is which and how FreeCAD numbers faces.

## Features

- **Automatic face detection**: Iterates through all faces of the cube
- **Geometric analysis**: Calculates proper sketch positioning using the sketch's actual coordinate system
- **Visual feedback**: Displays dice-like dot patterns on each face
- **Educational output**: Prints face centers, normals, and coordinate information to the console

## Installation

1. Open FreeCAD
2. Go to **Macro → Macros...**
3. Click **Create**
4. Paste the macro code
5. Save with a meaningful name (e.g., `dice_face_demo.FCMacro`)

## Usage

1. Open FreeCAD
2. Go to **Macro → Macros...**
3. Select the macro
4. Click **Execute**

The macro will:
- Create a new document (if none is active)
- Generate a 20mm cube centered at the origin
- Attach 6 sketches to the cube faces
- Display dot patterns (1-6) on each face
- Print detailed face information to the Report View

## Understanding the Output

### Console Output
Check the **Report View** (View → Panels → Report view) to see:
- Total number of faces
- For each face:
  - Face index (0-5)
  - Center coordinates
  - Normal vector (direction the face points)
  - Sketch X and Y axes in global coordinates
  - Calculated offset values

### 3D View
- Each face shows a circular pattern with dots
- Face 0: 1 dot
- Face 1: 2 dots
- Face 2: 3 dots
- Face 3: 4 dots
- Face 4: 5 dots
- Face 5: 6 dots

## Key Concepts Demonstrated

### Face Referencing
```python
# FreeCAD uses 1-based indexing for face names in the GUI
sketch.AttachmentSupport = [(cube, f'Face{i+1}')]
```

### Centering Sketches on Faces
The macro uses geometric analysis to center sketches:
1. Attach sketch to face
2. Query the sketch's placement (position and orientation)
3. Extract the sketch's local X and Y axes
4. Calculate vector from sketch origin to face center
5. Project onto local axes to get required offset

```python
sketch_x_axis = sketch_placement.Rotation.multVec(App.Vector(1, 0, 0))
sketch_y_axis = sketch_placement.Rotation.multVec(App.Vector(0, 1, 0))
origin_to_center = center - sketch_placement.Base
offset_x = origin_to_center.dot(sketch_x_axis)
offset_y = origin_to_center.dot(sketch_y_axis)
```

### Why Center the Cube at Origin?
The cube is positioned with its center at (0,0,0) to:
- Simplify coordinate calculations
- Make face positions predictable (±10mm on each axis)
- Follow CAD best practices
- Make the geometric relationships clearer

## Customization

### Change Dice Size
```python
dice_size = 30.0  # Change from 20.0 to any value
```

### Modify Dot Patterns
Edit the circle geometry in each `if i == X:` block to create different patterns.

### Add More Features
- Pocket the circles to create indentations
- Add fillets to round the edges
- Change colors for different faces
- Add text labels instead of dots

## Learning Points

This macro teaches:
- **Face iteration**: How to loop through all faces of a shape
- **Sketch attachment**: Using `AttachmentSupport` and `MapMode`
- **Coordinate systems**: Understanding local vs global coordinates
- **Geometric queries**: Using `CenterOfMass`, `normalAt()`, face vertices
- **Placement mathematics**: Working with rotations and projections
- **CAD workflow**: Centering geometry, using parametric values

## Technical Notes

- FreeCAD uses 0-based indexing in Python (Face 0-5) but 1-based in the GUI (Face1-6)
- Each face has its own local coordinate system that depends on its orientation
- The sketch's coordinate system is established by FreeCAD's attachment engine
- `AttachmentOffset` moves the sketch in its local coordinate space

## Next Steps

After understanding this example, you could:
1. Apply these techniques to more complex shapes
2. Create functions to automate sketch positioning
3. Build tools that work with arbitrary face selections
4. Explore different `MapMode` options for sketch attachment
5. Work with non-planar (curved) surfaces

## Requirements

- FreeCAD 0.19 or later
- No additional dependencies

## License

This is an educational example. Feel free to use and modify for learning purposes.

## Contributing

This is a learning example. Suggestions for improvements or additional educational features are welcome!
