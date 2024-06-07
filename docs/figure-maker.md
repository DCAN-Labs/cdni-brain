# Figure Maker 

It can often be difficult to standardize the way figures are made in the connectome workbench view.  Furthermore, workbench does not have an easy way to generate brain images automatically from the command line.  Nobody wants to manually create thousands of images.  The figure maker is a python wrapper for a bash script (make_dscalar_pics_vX.X.sh).  The script uses a template scene file and uses a(n exhausting) list of sed commands to replace the file names and parameters within the scene file to generate an image file.

On MSI, the code can be found at `/home/faird/shared/code/internal/utilities/figure_maker`. There is a README in this directory that includes more information about installation and dependencies. 

You can find the version-controlled code on [gitlab](https://gitlab.com/Fair_lab/figure-maker). 

Here is an example of a valid call to this script:
```
python3 ./figure_maker_wrapper.py --subject-scalar ./group1_dscalar_AVG.dlabel.nii --output ./output/figure.png --label-lower -1 --label-upper 1 --threshold-image --threshold-lower 0 --threshold-upper 10 
```

## Basic Arguments

- These arguments can be given in any order.
- The `--subject-scalar` argument is the only argument that is always required.
- If the `--label` flag is included, its `lower` and `upper` bound arguments must also be included. Otherwise, the bound arguments are unneeded. The same applies to the required arguments with the word `threshold`.

| Flag | Default | Description of Argument |
|-:|:-:|:-|
| `--subject-scalar` | N/A | Required. Path to subject `dscalar` or `dlabel` file. |
|  `--output` | `./figure.png` | Path to output image file. By default, it will be a `.png` file.<sup>1</sup> |
| `--label-lower` | Depends on `dscalar` vs. `dlabel` | Lower scale limit for color pallet. |
| `--label-upper` | Depends on `dscalar` vs. `dlabel` | Upper scale limit for color pallet. |
| `--palette-name`    | `ROY-BIG-BL` | Supply the name of the color palette. All of the color palette options are listed below.<sup>2</sup> |
| `--threshold-image` | N/A (`FALSE`) | Include this flag if you want to exclude data inside or outside of a specific threshold.
| `--threshold-lower` | Depends on `dscalar` vs. `dlabel` | Lower threshold for color pallet. |
| `--threshold-upper` | Depends on `dscalar` vs. `dlabel` | Upper threshold for color pallet. |
| `--threshold-inside` | `THRESHOLD_TEST_SHOW_OUTSIDE` | Include this flag to set data exclusion threshold to `THRESHOLD_TEST_SHOW_INSIDE` instead of `THRESHOLD_TEST_SHOW_OUTSIDE`. |
| `--make-quad` | N/A (`FALSE`) | Include this flag to make DV view. By default, it won't. |
| `--make-subcorticals` | `FALSE` | Include this flag to create new images that show parasaggital, coronal, and axial views. |
| `--width-in-cm` | `8` | Width in centimeters of the final image. |
| `--dots-per-cm` | `118` | Dots per centimeter (dpcm) in the final image. The default value, 118, will give 300 dots per inch (dpi). |
| `--save-scene-file` | N/A (`FALSE`) | Include this flag to save the scene file. Helpful for debugging. |
| `--wb-command` | (Depends on server) | Path to workbench command file (`wb_command`). Default depends on which server the script is run from. |

For more advanced arguments, see the README in `/home/faird/shared/code/internal/utilities/figure_maker`


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).