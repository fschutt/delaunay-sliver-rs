# delaunay-sliver-rs

Delaunay Refinement with Sliver Exudation

#### Algorithm Overview

Combines Ruppert's Delaunay refinement with Cheng et al.'s sliver exudation technique to generate 
high-quality 3D tetrahedral meshes by eliminating poorly-shaped tetrahedra.

#### Key Implementation Components

**Geometric Predicates:**
```rust
pub trait GeometricPredicate<T: Float> {
    fn orient3d(pa: &[T; 3], pb: &[T; 3], pc: &[T; 3], pd: &[T; 3]) -> T;
    fn insphere(pa: &[T; 3], pb: &[T; 3], pc: &[T; 3], pd: &[T; 3], pe: &[T; 3]) -> T;
}
```

**Ruppert's Refinement Algorithm:**
1. Split encroached segments first (prioritized over quality)
2. Insert circumcenters of poor-quality tetrahedra
3. Remove vertices in diametral balls to maintain conformity
4. Iterate until quality bound achieved

**Sliver Exudation Process:**
1. **Identify slivers**: Tetrahedra with radius-edge ratio > threshold or dihedral angles < 5°
2. **Weight assignment**: Solve optimization problem to assign vertex weights
3. **Weighted Delaunay**: Reconstruct triangulation with assigned weights
4. **Verification**: Ensure sliver removal while maintaining mesh quality

**Advanced Data Structures:**

```rust
pub struct Tetrahedron {
    pub vertices: [usize; 4],
    pub neighbors: [Option<usize>; 4],
    pub circumcenter: [f64; 3],
    pub circumradius: f64,
    pub radius_edge_ratio: f64,
}
```

**Robust Implementation:**

- Adaptive arithmetic predicates (Shewchuk's approach)
- Fast floating-point filter with exact arithmetic fallback
- Spatial indexing (octree) for fast point location
- SVD-based numerical stability for degenerate cases
