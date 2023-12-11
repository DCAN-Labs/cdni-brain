# Template Matching

Template matching is a method of network mapping that leverages commonly observed networks that have been previously observed in a group average to accelerate the community detection process. Individual networks are identified in a 3-step process:

* **Run a community detection of your choosing on an average connectivity matrix**: In Hermosillo et al. 2022, we used networks defined from an average connectivity matrix created from 120 healthy adults. Infomap community detection was conducted on the average matrix (See Gordon et al. 2017 for details).

* **Generate a template of networks**: Using the networks commonly observed in a group, we can create a set of network templates that are specific to the training group.

* **Match connectivity of each voxel**: Using a dense connectivity matrix from a subject in an independent test group, we correlate each voxel, or seed, to each of the network templates. Voxels are assigned to the network with the highest spatial similarity to the network template.

The code can be found on MSI at `/home/faird/shared/code/internal/analytics/compare_matrices_to_assign_networks`

Version-controlled code and documentation can be found on [Github.](https://github.com/DCAN-Labs/compare_matrices_to_assign_networks)