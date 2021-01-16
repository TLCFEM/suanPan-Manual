# Recorder

Record Model Response

The available output types are listed in [OutputType.md](OutputType.md) file.

Not all quantities at every time step need to be stored. Only those explicitly specified will be stored as history output.

Currently, there are several different types of recorders. Analysts can choose to record either **element response** at integration points (strain, stress, etc., some elements support response at elemental nodes) or **nodal response** at model nodes (displacement, velocity, acceleration, reaction, etc.). The third type supports the output of the summation of the same quantity of several nodes/elements. Alternatively, analyst can recorder global quantities such as global stiffness/mass, global kinetic energy, etc.

## Syntax

Two types of format are supported. The output file can be either plain text or HDF5 file if the program is compiled with HDF5 support. The general form to define a record is as follows.

```
recorder (1) plain (2) (3) [every 4] [5...]
recorder (1) hdf5 (2) (3) [every 4] [5...]
# (1) int, unique recorder tag
# (2) string, recorder type
# (3) string, object type that needs to be recorded
# [4] int, optional argument with leading keyword "every" to indicate the interval of recording, for example: "every 10" means the recorder will record the response every ten converged substeps.
# [5...] int, tags of elements/nodes, etc.
```

Alternatively, it is possible to use the following two equivalences.

```
plainrecorder (1) (2) (3) [every 4] [5...]
hdf5recorder (1) (2) (3) [every 4] [5...]
```

There are some recorder types that do not need tags of elements/nodes as they record the response of the whole model.

## Eigen Recorder

```
# to record eigenvalues and eigenvectors if current step is an eigenanalysis step
recorder (1) plain Eigen
recorder (1) hdf5 Eigen
# (1) int, unique recorder tag
```

## Frame Recorder

```
# to record response of the whole model, this one only supports HDF5
recorder (1) hdf5 Frame (3) [every 4]
# (1) int, unique recorder tag
# (3) string, object type that needs to be recorded
# [4] int, optional argument with leading keyword "every" to indicate the interval of recording, for example: "every 10" means the recorder will record the response every ten converged substeps.
```

## Visualization Recorder

```
# to record response of the whole model and write to VTK files for visualisation, this one is enabled only with VTK support.
recorder (1) plain Visualisation (3) [every 4]
recorder (1) hdf5 Visualisation (3) [every 4]
# (1) int, unique recorder tag
# (3) string, object type that needs to be recorded
# [4] int, optional argument with leading keyword "every" to indicate the interval of recording, for example: "every 10" means the recorder will record the response every ten converged substeps.
```

The `Visualisation` will immediately write the disk with incremental file names.

## Node Recorder

To record nodal response,

```
recorder (1) plain Node (3) [every 4] [5...]
recorder (1) hdf5 GroupNode (3) [every 4] [5...]
# (1) int, unique recorder tag
# (3) string, object type that needs to be recorded
# [4] int, optional argument with leading keyword "every" to indicate the interval of recording, for example: "every 10" means the recorder will record the response every ten converged substeps.
# [5...] int, tags of nodes or node groups, etc.
```

If `GroupNode` is used, then the tags of node groups shall be used.

## Element Recorder

To record element response,

```
recorder (1) plain Element (3) [every 4] [5...]
recorder (1) hdf5 GroupElement (3) [every 4] [5...]
# (1) int, unique recorder tag
# (3) string, object type that needs to be recorded
# [4] int, optional argument with leading keyword "every" to indicate the interval of recording, for example: "every 10" means the recorder will record the response every ten converged substeps.
# [5...] int, tags of elements or element groups, etc.
```

If `GroupElement` is used, then the tags of element groups shall be used.
