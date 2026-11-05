Claude


Lav1 2 · MD
Image Manipulation
Two different things can be done to an image f(x, y):

filtering : g(x, y) = T( f(x, y) ) change the VALUES, positions stay e.g. brightness, contrast, blur warping : g(x, y) = f( T(x, y) ) change the POSITIONS, values stay e.g. scale, rotate, perspective

(In filtering, T acts on the pixel value. In warping, T acts on the coordinates, and then we look up the pixel at the new location.)

example: filtering: every pixel gets +50 brightness, nothing moves. warping: the pixel that was at (10, 20) is now shown at (20, 40), its colour is unchanged.

2x2 transformations (linear warps)
matrix * vector = new vector

[[1, 0],   [px]   [px]
 [0, 1]] * [py] = [py]        identity: nothing moves
The columns of the matrix tell you where (1,0) and (0,1) land. Every other point follows: (px, py) lands at px * column1 + py * column2.

scaling: [[a, 0], [px] [apx] [0, b]] * [py] = [bpy] stretch x by a, y by b

example: a = 2, b = 1 makes the image twice as wide, same height.
rotation: a point at radius r and angle θ is x1 = r cos θ, y1 = r sin θ rotate it by φ: we want x2 = r cos(θ+φ), y2 = r sin(θ+φ)

cos(θ+φ) = cos θ cos φ - sin θ sin φ
sin(θ+φ) = sin θ cos φ + cos θ sin φ

multiply both by r and substitute x1 = r cos θ, y1 = r sin θ:

x2 = x1 cos φ - y1 sin φ
y2 = x1 sin φ + y1 cos φ

so rotation is the matrix
[[cos φ, -sin φ],
 [sin φ,  cos φ]]

(columns = where (1,0) and (0,1) land: (cos φ, sin φ) and (-sin φ, cos φ))

example: φ = 90° gives [[0, -1], [1, 0]]. The point (1, 0) goes to (0, 1).
Shear (skew)
x2 = a11 x1 + a12 y1
y2 = a21 x1 + a22 y1
(first index = row, second index = column, so a12 is row 1, column 2)

a12 ≠ 0 : x is shifted by an amount that depends on y.
          points higher up move further sideways. a square becomes a slanted box.
a21 ≠ 0 : y is shifted by an amount that depends on x.

example: [[1, 0.5], [0, 1]] sends (0, 1) to (0.5, 1).
         the bottom edge stays, the top edge slides right.
Mirror
x2 = -x1   or   y2 = -y1

[[-1, 0], [0, 1]] flips left-right.  [[1, 0], [0, -1]] flips top-bottom.
a mirror has determinant -1. that is how you detect a flip from the numbers.
Combining transformations
To rotate then stretch a vector v: first R v, then S (R v). By associativity this equals (S R) v, so the combined transformation is the single matrix M = S R. Multiply the matrices once, apply once.

Order matters: S R is NOT R S in general. S = [[2,0],[0,1]] (stretch x by 2), R = rotate 90° S R applied to (1,0): rotate -> (0,1), stretch -> (0,1) R S applied to (1,0): stretch -> (2,0), rotate -> (0,2) different results.

Read right to left: in (S R) v, R happens first because it is next to v. The vector is always on the far right.

A (B C) = (A B) C      grouping does not matter (associative)
A B ≠ B A              order does matter (not commutative)
Properties of every 2x2 (linear) transformation: origin maps to origin lines remain lines parallel lines remain parallel evenly spaced points stay evenly spaced

3x3 matrices: translation (affine)
x2 = x1 + tx
y2 = y1 + ty
A 2x2 matrix cannot do this: it always sends (0,0) to (0,0), and a translation moves the origin. Fix: add a third coordinate.

Homogeneous coordinates: write the 2D point (x, y) as (x, y, 1). To read a 3-vector (xbar, ybar, zbar) back as a 2D point, divide by zbar:

x = xbar / zbar,   y = ybar / zbar
So (2, 4, 1), (4, 8, 2) and (1, 2, 0.5) are all the same point (2, 4). A point and any scaled copy of it mean the same thing.

Translation as a matrix:

[1 0 tx]   [x1]   [x1 + tx]
[0 1 ty] * [y1] = [y1 + ty]
[0 0 1 ]   [1 ]   [   1   ]
zbar is 1, dividing by 1 changes nothing, so we read off (x1 + tx, y1 + ty).

General affine = any 2x2 in the top-left + translation in the last column:

[a b tx]
[c d ty]
[0 0 1 ]

bottom row is [0 0 1], so zbar is always 1 and the division never does anything.
6 free numbers.
Properties: origin NO LONGER maps to origin lines remain lines parallel lines remain parallel

(moving the origin means an affine map is not linear in 2D.
 but on the 3-vector (x, y, 1) it is a plain matrix multiply, so it is
 linear in 3D. that is the whole trick of the extra coordinate.)
3x3 projective transformations (homography)
[h11 h12 h13]
[h21 h22 h23]
[h31 h32 h33]
Now the bottom row is free. The third coordinate becomes

zbar = h31 x + h32 y + h33
which depends on the point. Each point is divided by a different number. Far points get divided by more, so they shrink: this is perspective.

x2 = (h11 x + h12 y + h13) / (h31 x + h32 y + h33)

example: h31 = 0.5, everything else identity.
         (0, 0) -> (0, 0)        divided by 1
         (1, 0) -> (0.67, 0)     divided by 1.5
         (2, 0) -> (1, 0)        divided by 2
         points along x bunch up. as x -> infinity, x2 -> h11 / h31 = 2.
         that limit is the vanishing point.
Properties: origin not fixed lines remain lines parallel lines do NOT stay parallel (they meet at the horizon) closed under composition (two homographies in a row is a homography)

Where things live in the matrix: h11 h12 h21 h22 : scale, rotate, shear (the 2x2 part) h13 h23 : translation (third column) h31 h32 : perspective (third row)

Defined up to scale: (kH) p = k (H p) = (k xbar, k ybar, k zbar) divide: (k xbar / k zbar, k ybar / k zbar) = (xbar / zbar, ybar / zbar) the k cancels. H and kH are the SAME transformation. same reason that (x, y, 1) and (2x, 2y, 2) are the same point.

example: [[1,0,0],[0,0.6,0],[0,-0.6,1]] and [[3,0,0],[0,1.8,0],[0,-1.8,3]]
         give exactly the same picture.
Careful: this is about scaling ALL nine entries together. changing h33 alone DOES change the picture. for an affine matrix it scales the whole output; for a projective one it also moves the horizon.

8 degrees of freedom: 9 entries, but one direction (scale all nine) is invisible. 9 - 1 = 8 numbers actually matter.

picture: in the 9D space of entries, one transformation is a whole
line through the origin (all the kH). the set of transformations is
the set of such lines, and that set is 8-dimensional.
(compare: 2D points are lines through the origin in 3D homogeneous space.
 3 numbers, 2 dimensions. same idea, one level up.)
Fixing one entry: since H and kH are the same, choose one copy per transformation. usual choice: divide all entries by h33 so h33 = 1. this does not remove a real knob, it removes the duplicate copies.

any entry could be pinned instead (h11 = 1 also works), but pinning
fails when that entry is 0. h33 = 0 is rare in practice, h11 = 0
happens for a 90° rotation, so h33 is the usual choice.
safest: scale so that sum of squares of all 9 entries = 1. always works.
Finding H from points: each point pair (x, y) -> (x2, y2) gives 2 equations. 8 unknowns need 8 equations, so 4 point pairs. (4 points, not 8.) more than 4 pairs: least squares.

example: the 4 corners of a tilted document in a photo and the 4
         corners of the flat A4 page you want. that fixes H, and
         applying it un-warps the photo.
Summary table
type         bottom row         free numbers   keeps
------------------------------------------------------------------------
2x2 linear   (none)             4              origin, lines, parallels, spacing
affine       [0  0  1]          6              lines, parallels
projective   [h31 h32 h33]      8              lines only



Claude finished the response

Fixing h33 = 1 is: choosing k = 1/h33.

That’s the whole link. Do it with numbers:

H  = [2  0  0]
     [0  2  0]
     [1  0  4]        h33 = 4

pick k = 1/4:

kH = [0.5  0   0]
     [0    0.5 0]
     [0.25 0   1]     h33 = 1

kH is the same transformation as H (first fact), and it has h33 = 1 (what we wanted). We didn’t assume anything about H; we used the freedom that kH ≡ H gives us and spent it on making one entry convenient.

So the sentence “we can fix h33 = 1” means: for any H, the copy H/h33 exists, is the same transformation, and has a 1 in that corner. It’s a thing you do, not a restriction on which transformations exist.

Why this makes the count come out to 8

Every transformation is a family {kH}. In that family, exactly one member has h33 = 1 (the one with k = 1/h33). So if you only look at matrices with h33 = 1, you see each transformation exactly once, no repeats. Those matrices have 8 entries left to choose. 8 transformations’ worth of freedom, 8 numbers.

The only time this breaks is h33 = 0, because then 1/h33 doesn’t exist and no member of the family has h33 = 1. That’s the one case the convention can’t represent, and why norm = 1 is used in robust code.

