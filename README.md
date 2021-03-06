## VirSorter2_to_Anvio
Draft script to generate Anvi'o-compatible annotation files from VirSorter2 output, for visualization of VS2 results in Anvi'o. Building on https://merenlab.org/2018/02/08/importing-virsorter-annotations/ but now adjusted for VirSorter2.

## Steps
# VirSorter2
First, one needs to run VirSorter2, using something like:
```
virsorter run -i input_file.fna -w VirSorter2_results/ --keep-original-seq --prep-for-dramv --hallmark-required-on-short -j 4
```

The important option here is "prep-for-dramv", otherwise hallmark genes annotation can not be imported

# Create Anvi'o input files
Now we create the input files for Anvi'o, using this new script
```
./virsorter_to_anvio.py -i VirSorter2_results/ -s splits_basic_info.txt -n all_gene_calls.txt -d /path/to/databases/VirSorter2/db/  -A virsorter_additional_info.txt -C virsorter_collection.txt -F virsorter_annotations.txt
```
VirSorter2_results/ is the output directory from VirSorter2
splits_basic_info.txt and all_gene_calls.txt are files generated by/from Anvi'o using anvi-export-table and anvi-export-gene-calls
path/to/databases/VirSorter2/db/ is the path to the VirSorter2 database on your system, used to identify hallmark genes
virsorter_additional_info.txt, virsorter_collection.txt, and virsorter_annotations.txt are output files, which will be created by the script

If you do not care for annotation of the hallmark genes, a simplified command line would be:
```
./virsorter_to_anvio.py -i VirSorter2_results/ -s splits_basic_info.txt -A virsorter_additional_info.txt -C virsorter_collection.txt
```

# Import information into Anvi'o
If everything worked as expected, you should be able to import the contigs collection, information, and hallmark gene annotation in your Anvi'o db and profile:
```
anvi-import-collection virsorter_collection.txt -c CONTIGS.db -p My_profile/PROFILE.db -C VirSorter2
anvi-import-misc-data virsorter_additional_info.txt -p My_profile/PROFILE.db --target-data-table items
anvi-import-functions -c CONTIGS.db -i virsorter_annotations.txt
```

# Some additional notes
- If you start from scratch, you should clean up your fasta file for Anvi'o (https://merenlab.org/software/anvio/help/7/programs/anvi-script-reformat-fasta/) before running VirSorter2, otherwise the contig names won't match, and the script won't work, and  everyone will be sad :-(
