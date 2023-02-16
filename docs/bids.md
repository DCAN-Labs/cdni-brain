# Additional BIDS Validation 

1. Process a subject with the chosen pipeline to see if it fails right away. 

    1. This will tell you if there are any “flaws” in your data that would cause the specific pipeline you are trying to run to fail. Examples:

        1. A field is missing in your sidecar JSONs

        2. The pipeline you are running requires something specific

2. Use bash or python commands to modify the data if it ends up not being BIDS compatible and/or the pipeline you are running is unable to run successfully.

    31. Examples: rename, cp, mv

        14. For how to use, insert_command --help or man insert_command 

    32. CuBIDS or the pipeline .err / .out files will let you know what is needed to fix – the degree of BIDS-valid is determined by the pipeline.

    33. Common errors: 

        15. IntendedFor sidecar JSON fields missing

        16. BIDS flags out of order in the file names

        17. Missing parameters in sidecar jsons

        18. Subject session directories have a different naming scheme than the files
        
    34. For more information on the BIDS specification, see this [link](https://github.com/PennLINC/CuBIDS)