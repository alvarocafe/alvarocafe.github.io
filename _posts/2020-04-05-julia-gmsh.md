---
title: "Using Julia and Gmsh for topology optmization"
published: false
---

In this post, I'll show you how to use Julia alongside Gmsh to perform a topology optmization. This might sound scary, but it'll actually be a simple 2D shape which will change according to some rule. This rule will be related to the sound field on the interior of the shape.

From the git page of the Gmsh.jl module, an example is shown:

`
import Gmsh: gmsh
gmsh.initialize()
gmsh.option.setNumber("General.Terminal", 1)
gmsh.model.add("t1")
lc = 1e-2
gmsh.model.geo.addPoint(0, 0, 0, lc, 1)
gmsh.model.geo.addPoint(.1, 0,  0, lc, 2)
gmsh.model.geo.addPoint(.1, .3, 0, lc, 3)
gmsh.model.geo.addPoint(0, .3, 0, lc, 4)
gmsh.model.geo.addLine(1, 2, 1)
gmsh.model.geo.addLine(3, 2, 2)
gmsh.model.geo.addLine(3, 4, 3)
gmsh.model.geo.addLine(4, 1, 4)
gmsh.model.geo.addCurveLoop([4, 1, -2, 3], 1)
gmsh.model.geo.addPlaneSurface([1], 1)
gmsh.model.addPhysicalGroup(0, [1, 2], 1)
gmsh.model.addPhysicalGroup(1, [1, 2], 2)
gmsh.model.addPhysicalGroup(2, [1], 6)
gmsh.model.setPhysicalName(2, 6, "My surface")
gmsh.model.geo.synchronize()
gmsh.model.mesh.generate(2)
gmsh.write("t1.msh")
gmsh.finalize()
`

Using this example, only a .msh file is created. To run a BEM simulation using this mesh, we can use BB. 