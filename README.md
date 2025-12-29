
This repository contains MATLAB scripts and data used for
3D surface mesh processing, conformal mapping, and toolpath generation.

## Contents
- `matlab/` : MATLAB Live Scripts (.mlx)
- `data/`   : Mesh node and element data exported from ANSYS
- `report/` : Project report (PDF)

## How to use

1. Copy the input files (mesh data files) into the same directory as the MATLAB scripts.
2. Open MATLAB.
3. Set the current folder to the directory containing the MATLAB codes.
4. Run the main script to execute the full workflow.

## Pseudo Code
ALGORITHM: Contour-Parallel Toolpath Generation

INPUT: nodes.txt, elements.txt
OUTPUT: 3D toolpaths

1. LOAD MESH
   - Read nodes (X, Y, Z coordinates)
   - Read elements (triangle connectivity)

2. DETECT BOUNDARIES
   - Extract all edges from triangles
   - Find edges appearing only once → boundary edges
   - Trace connected edges to form closed loops
   - Separate boundary nodes from interior nodes

3. CONFORMAL MAPPING (3D → 2D)
   - Compute cotangent weights for all edges
   - Find longest interior edge
   - Fix its two endpoints at (0,0) and (L,0)
   - Build sparse linear system:
     * Interior nodes: standard Laplacian (Equation 2)
     * Boundary nodes: free boundary equations (Equation 3)
   - Solve system → get UV coordinates for all nodes

4. GENERATE 2D TOOLPATHS
   - Create polygon from outer boundary
   - Subtract interior holes (if any)
   - LOOP until polygon disappears:
     * Offset polygon inward by distance d
     * Extract boundary as toolpath
     * Check if area < threshold → stop

5. MAP TOOLPATHS TO 3D
   - FOR each 2D toolpath:
     * FOR each point (u,v):
       - Find triangle containing point
       - Compute barycentric coordinates (w1, w2, w3)
       - Interpolate: P_3D = w1*V1 + w2*V2 + w3*V3

6. VISUALIZE
   - Display surface mesh + toolpaths
   - Export results

END
## Author
Riadh Zerkouk
