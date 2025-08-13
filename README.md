CURRENT VERSION HAS MY VIZUALIZATION OMITTED - I will be producing a version with an STL of the Viz soon

Dependencies 
pathLib, numpy, trimesh, tqdm

External File/Variable Naming:

Script will look for this target:
SCALED_HEAD.STL 
(Compatiable with Yishai's Skin-Only MNI, potentially with other similar)

^If Unable to find, the script will print that it did not and then look for another file MNI_Head.stl 
MNI_Head.stl is simply my reference mesh (unique to my folder/computer)

Script will look for these 4 variables - which should be defined in numbers as X, Y, Z coordinates separated by commas.
FPZ
INION
CZ
DLPFC (referred in readme as target)

^if unable to find, the script will print that it did, then, default to non-SCALED MNI Head Coorindates (these are stored in script currently)

---- 

What the script does (more detail to come later).

| Load and reducing thickness of Mesh to 0:
-> Loads Mesh (Skin Only)
-> reduces mesh to a single layer about the outer most edge (i.e., the layer that defines the edge of scalp no longer has width)

| Plotting points and snapping to new mesh.

| Calculating 1 Additional and Necessary Point (Arbitrarily named "Inter BC"): 

Next, it calculates one point that is necessary for defining the horizontal scalp measurement in the script as Inter_BC
In vivo - this point is established by extending a measuring tape from Cz to Target (i.e. ldlPFC) and then past it until
it intersects a previously defined circumference of the head about FpZ and Inion.

In the script....
1. A plane is defined that includes Inion and Fpz, which in of itself has insufficient information to be defined in a single way.
So, we define the plane such that is it normal to Cz | that is we drew a line segment between Fpz and Inion, and then drew a line from Cz
to intersect the Fpz-Inion line orthogonally (where it's perendicular, all right angles at crossing)... The plane is defined in a manner
where if we added a 3rd dimension about intersection point, that it would for another set of right angles.  (in english, the plane
needs to be anchored "flat")

2. now that we have a reference of "flatness" relative to mesh (i.e. self referential, rather than assumption of space required)
we can now draw another plane that includes Cz and Target that is perpendicular to the first plane.

3. There are now two points where the plane intersects each other AND the scalp. We take the "front" one by taking the coordinate
   closest to FpZ

| Measuring about scalp in a straight path (but not relying on geodesic distnace as it can be clunky / potentially not actually straight)

A reference point is created inside the headmesh (in this case by using the midpoint of FpZ and Inion)
Two rays are then created starting from this midpoint to the points we'd like to measure between.
A plane is defined about the rays (ensuring it extends past the scalp)

All points Where this new "pizza slice" area pass through a line segment defining parts of the mesh (or a point that is defining segments [rare]) is noted.

This allows us to trace a straight path (in this case using greedy nearest neighbor rule) along the two points, while accouting for the curve of the scalp (like a measuring tape).


















