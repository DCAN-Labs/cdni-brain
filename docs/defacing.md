## Defacing and De-Identification - UNDER CONSTRUCTION !!!!

*Please note that different studies as well as institutions have different policies around de-identification requirements, so always check the specific requirements for your data first in order to ensure that you thoroughly de-identify your data to those standards.*

### Defacing

Defacing is performed in order to mask out the eyes, nose, and mouth from MRI images. These features could hypothetically be used to reconstruct someone's facial structure, which is considered indirect identifiable information. Skull-stripping can also be used for defacing, however this limits what kind of preprocessing can be performed with the data in the event that head images are required.

There are several tools available for defacing, but we recommend using [Pydeface](https://pypi.org/project/pydeface/). Pydeface registers a default atlas to the subject head and applies the resulting linear transform to an accompanying atlas face mask to create a subject-specific mask. Pydeface takes one input, the subject head file (see usage for additional options), so simply `pip install pydeface` and run:

```
pydeface subject_head.nii.gz
```

Defacing infant data is more difficult compared to adults due to contrast inversion (and variable or lower contrast depending on the age and modality) in T1w and T2w images over the first 6 months of age, so it's best to use age-specific atlases in order to get good results. 


### De-Identification
De-identification involves stripping MRI images data of all PHI. 
