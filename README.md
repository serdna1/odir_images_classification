The first step is process the file 'ODIR-5K_Training_Annotations(Updated)_V2.xlsx' provided by ODIR to obtain a csv file ('odir_classes.csv') with 1 eye and 1 class per row (in the cited original file each row contains both eyes information and multiple classes per eye). The file 'process_annotations.ipynb' does this task and provides a second (intermediate) csv file ('odir_table.csv') that contains 1 eye per row and all the clases of that eye. To know the classes of each eye, the .ipymb file matches keywords in each diagnosis of the 'Left-Diagnostic Keywords' and 'Right-Diagnostic Keywords' fields. These are:
'normal fundus' -> N
'proliferative' -> D
'glaucoma' -> G
'cataract' -> C
'age-related macular degeneration' -> A
'hypertensive' -> H
'myopia' -> M
None of the previous match -> O