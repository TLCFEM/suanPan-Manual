# Contact Between Beam And Block

In this example, we showcase the simulation of a 2D node-line contact problem. The model can be downloaded. [contact-between-beam-and-block.supan](contact-between-beam-and-block.supan)

## The Beam

Suppose there is an elastic cantilever beam with left end at $$(0,0)$$ fully fixed, and right end at $$(4,0)$$ subjected to a displacement load.

First, we define some nodes.

```
node 1 0 0
node 2 1 0
node 3 2 0
node 4 3 0
node 5 4 0
```

We use `EB21` element to define the element. Accordingly, an elastic material is attached.

```
# E=10
material Elastic1D 1 10

# I=12 A=1
element EB21 1 1 2 12 1 1
element EB21 2 2 3 12 1 1
element EB21 3 3 4 12 1 1
element EB21 4 4 5 12 1 1

fix 1 P 1

displacement 1 0 -.2 2 5
```

## The Block

We use `CP4` to define the block.

```
node 6 3.5 -.4
node 7 4.5 -.4
node 8 4.5 -.1
node 9 3.5 -.1

# E=10 v=0
material Elastic2D 2 10 .0

# use material 2
element CP4 5 6 7 8 9 2 1.

fix 2 1 6 7
fix 3 2 6 7
```

## The Contact Interaction

The `Contact2D` element is used to define contact. It requires two node groups that define master and slave, respectively.

Note the outer norm of master line is defined by rotating the axis by $$\pi/2$$ anticlockwise, the node sequence matters.

```
# slave node group
nodegroup 1 5

# master node group, pay attention to the sequence
nodegroup 2 9 8

# define the contact
# multiplier=1E6
element Contact2D 6 2 1 1E6
```

## Other Settings and Analysis

It would be good to define a `Visualisation` recorder so that animations can be generated later by ParaView.

```
hdf5recorder 1 Visualisation U2
```

Now define the step and perform the analysis.

```
step static 1
set ini_step_size 1E-2
set fixed_step_size 1

converger RelIncreDisp 2 1E-10 10 1

analyze

exit
```

## Result

![result](contact-between-beam-and-block.gif)
