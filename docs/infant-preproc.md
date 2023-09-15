# Infant Preprocessing 

Defacing is an important preprocessing step to ensure the anonymity of the subjects. Without defacing, a subjects facial structure could theoretically be reconstructed from a MRI image. Although skull-stripping could also be done to anonymize an image, this can limit what processing can be done to those images and this is especially hard to do in infants, which is why defacing is the standard method of anonymization. 

Defacing is done by creating a rough boxy mask that cuts off the eyes/nose/mouth so that only the brain is left in the image. This mask is created by using an existing atlas that has a mask and registering the subject head to that atlas. A linear transform is then applied to the template mask to create a mask that is in the subject space. 

Usage for a directory that contains a subject folder, and the subject folder contains a T1w image, T2w image, or both:

`python3 /home/feczk001/shared/projects/BCP_deface/code/deface.py /path/to/directory/`