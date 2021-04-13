# NZ Strong Motion

The **original** NZ strong motion database can be downloaded from this [page](https://www.geonet.org.nz/data/supplementary/nzsmdb).

The **accelerograms** are **processed** and **compiled** into binary files. Please [download](NZStrongMotion.7z) the archive first before using any recordings. Users may need [7-Zip](https://www.7-zip.org/download.html) to unpack the archive.

## Syntax

```
amplitude NZStrongMotion (1) (2)
# (1) int, unique amplitude tag
# (2) string, accelerogram name
```

It must be noted that the amplitudes are normalized so that the maximum absolute magnitude is **unity**. Hence to achieve a target PGA, it shall be assigned to the magnitude of the acceleration load.

## Usage

For example, to load the accelerogram `20110221_235142_CSHS`, which is one of the 2011 Christchurch earthquake recordings, user shall first extract the file `20110221_235142_CSHS` from `NZStrongMotion.7z` and place it where the program can locate, for example, the current working folder.

To define such an amplitude, one can use

```
amplitude NZStrongMotion 1 20110221_235142_CSHS
```

## Explanation of File Format

The format of original file can be seen [here](https://www.geonet.org.nz/data/supplementary/strong_motion_file_formats).

The processed archive first extract accelerogram name from the file, then read how many acceleration values shall be read and the interval between two records, then read all values and scale them by multiplying each by $$1000$$ and store them into an integer array. The array has the following layout.

| index | value ($$\times1000$$)                  |
|:-----:|:---------------------------------------:|
| 0     | time interval of the record in seconds  |
| 1     | absolute maximum acceleration value     |
| 2     | acceleration value at first time point  |
| 3     | acceleration value at second time point |
| 4     | acceleration value at third time point  |
| 5     | ...                                     |