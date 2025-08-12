# dconn Shrinker

This page explains a tool developed by Martin Gell to convert a .dconn file into an hdf5 file, which shrinks the size of the dconn by almost 80% (from ~33 GB to ~7GB). This can be very helpful when your analysis requires loading many dconns at once, something that is very resource intensive. 

Turning a .dconn into a hdf5 file involves vectorizing the upper-triangle of the connectivity matrix and saving that data to an hdf5 file. It also saves the number of grayordinates and the diagonal values so that it can be turned back into a .dconn. This file format also allows you to only pull certain chunks of the data at a time. 

Below is an example code block that uses python to load the hdf5 file and pull rows 1000-2000:

```
# Open the HDF5 file and load only the desired slice
 import h5py
        with h5py.File(path_to_file.h5, 'r') as f:
            dataset = f['data']
            chunk_data = dataset[1000:2000]
```

Right now, there is not an easy way to pull specific row/column of the original dconn. This is something Martin or Eric Feczko can help figure out if you need it.

The code can be found on MSI at `/home/faird/shared/code/internal/utilities/dconn_shrinker` and on GitHub at the [dconn_shrinker repo](https://github.com/DCAN-Labs/dconn_shrinker). 

For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
