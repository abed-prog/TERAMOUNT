# 3D Structure Generator - User Guide

## Overview
This script creates a 3D model consisting of a house with a triangular roof and an external box, generates a 2D top-view projection, and documents all measurements in CSV format.

## Requirements

### For Google Colab (Recommended)
No setup required - the script automatically installs dependencies.

### For Local Installation
```bash
pip install cadquery trimesh rtree matplotlib pandas numpy
```

## How to Run

### Google Colab
1. Upload `ex.py` to Google Colab
2. Run the script
3. Enter dimensions when prompted
4. Files will auto-download

### Local Machine
```bash
python ex.py
```
Note: Remove lines 18 and 26-27 (Colab-specific imports) and line 199 (auto-download).

## User Parameters

When running the script, you'll be prompted to enter three values:

```
Enter house base dimensions (width depth height): 
```

### Input Format
- Enter three numbers separated by spaces
- Example: `10 8 6` or `10,8,6`
- Units are arbitrary but must be consistent

### Parameters Explained

| Parameter | Description | Example | Effect |
|-----------|-------------|---------|---------|
| **width** | House base width | 10 | Determines house and roof width |
| **depth** | House base depth | 8 | Determines house depth |
| **height** | House base height | 6 | Determines wall height (roof adds 50% more) |

## Derived Dimensions

The script automatically calculates:

| Element | Formula | Example (w=10, d=8, h=6) |
|---------|---------|--------------------------|
| Roof height | 0.5 × height | 3 |
| Roof depth | 1.5 × depth | 12 |
| Roof overhang | 0.25 × width | 2.5 |
| Total roof width | width + 2 × overhang | 15 |
| Box width/depth | 0.25 × width | 2.5 |
| Box height | 2 × height | 12 |
| Box X position | 2 × width | 20 |

## Output Files

### 1. `3D_house_and_box.stl`
- Complete 3D model
- Can be viewed in any STL viewer
- Ready for 3D printing

### 2. `Topography.png`
- Top-view projection showing heights
- Color-coded elevation map
- 300 DPI resolution

### 3. `distances_and_positions_of_elements.csv`
Contains:
- Element positions (X, Y, Z centers)
- Bounding box coordinates
- All corner positions
- Distance relationships between elements

## Customizing the Model

### Changing Proportions
Edit these ratios in lines 34-42:

```python
roof_h = 0.5 * h         # Change to 0.7 for taller roof
roof_d = 1.5 * d         # Change to 2.0 for longer roof
roof_oh = 0.25 * w       # Change to 0.3 for more overhang
box_w = 0.25 * w         # Change to 0.4 for wider box
box_h = 2 * h            # Change to 1.5 for shorter box
```

### Changing Box Position
Edit line 45:
```python
box_xmin = 2 * w         # Change to 3*w for more separation
```

### Changing Grid Resolution
Edit line 89 for higher/lower quality projection:
```python
grid_size = 400          # Increase to 800 for higher detail
```

### Changing Colors
Edit line 109:
```python
cmap="viridis"           # Try "plasma", "coolwarm", "terrain"
```

## Troubleshooting

### Common Issues

1. **Import Error**: Install missing packages
   ```bash
   pip install cadquery trimesh matplotlib pandas
   ```

2. **Memory Error**: Reduce grid_size to 200

3. **STL Not Loading**: Ensure CadQuery is properly installed

4. **No File Download**: For local use, files are saved in current directory

## Example Usage

```
Enter house base dimensions (width depth height): 12 10 8

Output:
✅ STL saved as: 3D_house_and_box.stl
✅ Topography saved as: Topography.png
✅ CSV saved as: distances_and_positions_of_elements.csv
```

This creates:
- House: 12×10×8 units with 4-unit tall roof
- Box: 3×3×16 units, positioned 24 units from origin
- Total model height: 16 units