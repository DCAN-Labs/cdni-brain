# Cifti Connectivity Matrix 

One crucial way that we explore brain function is to explore the functional connectivity of the brain. The way we do that is by determining the correlation of the fluctuations in the BOLD signal over time. That’s another way of saying that we are interested in the voxel-to-voxel (or grayordinate-to-grayordinate or parcel to parcel) correlation. Exactly how this is done can greatly affect the correlation structure of the data. 

This code takes the BOLD time series data for a single voxel and correlates it with every over voxels time series. It does this for every voxel in the brain. The final result is a matrix of correlations. Workbench can also provide a correlation matrix but this code allows for more customization. This allows the user to play around with how the correlation is calculated and how that affects the resulting matrix. For example, a correlation matrix that does not use any motion censoring will almost certainly look different that one that does use motion censoring. A correlation matrix with many time points will be much more reliable than one with few time points.

This code can be found on MSI at `/home/faird/shared/code/internal/utilities/cifti_connectivity`

Version-controlled code and documentation can be found on [GitHub.](https://github.com/DCAN-Labs/cifti-connectivity)

For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).