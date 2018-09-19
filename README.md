﻿﻿﻿﻿﻿
# Unix Assignment

# Zea mays
## Data Inspection
Inspection of data to figure out the header and number of rows


`head -n 5 snp_position.txt`

`tail -n 5 snp_position.txt`

`head -n 5 fang_et_al_genotypes.txt`

`head -n 5 fang_et_al_genotypes.txt`

`wc fang_et_al_genotypes.txt`

`wc snp_position.txt`

## Sorting files before joining them
#### We need the header intact for further processing, so first line saves the header to file and the second adds the sorted data to the file other wise the header will be sorted as well
`head -n 1 snp_position.txt > snp_sorted.txt`

`tail -n+2 snp_position.txt | sort  -k1,1 >>snp_sorted.txt`

## Rearranging the columns in the file according to final output
`awk 'BEGIN {FS="\t"; OFS="\t"} {print $1, $3, $4, $2, $5,$6,$7,$8,$9,$10,$11,$12,$13,$14,$15}' snp_sorted.txt >snp_sorted_col.txt`

## Filtering only maize data from genotype dataset
 
`head -n 1 fang_et_al_genotypes.txt > maize_genotypes.txt`

`grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt >>maize_genotypes.txt`

## Transposing the filtered data

`awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt`

## Sorting the data before join 
#### The second and third row do not contain genotype data, so the second command skips them while retaining the header
`head -n 1 transposed_maize_genotypes.txt > maize_sorted.txt`

`tail -n+4 transposed_maize_genotypes.txt | sort  -k1,1 >>maize_sorted.txt`

## Joining the two files
#### -t "[Ctrl+V, tab ]" = -t "<tab>" in Unix to  retain tab separated format of the output
`join -1 1 -2 1 --header -t " [Ctrl+V, tab ]       " snp_sorted_col.txt maize_sorted.txt > maize_joined.txt`

## Creating 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
 `head -n 1 maize_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 >"Chr_"i"_asc.txt"}'`
#### - Creates files with name Chr\_(Chromosome number)\_asc.txt for each chromosome with just the header row
`tail -n +2 maize_joined.txt | sort -k2,2 -k3,3n | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {  print  >>"Chr_"$2"_asc.txt"}}'`
#### - Sorts the data from the joined file (excluding header) based of Chromosome number followed by ascending order of position, then pipes it into awk which saves data for each chromosome number into respective files.

## Creating 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -
 `head -n 1 maize_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 >"Chr_"i"_des.txt"}'`
#### - Creates files with name Chr\_(Chromosome number)\_des.txt for each chromosome with just the header row
`tail -n +2 maize_joined.txt | sort -k2,2 -k3,3rn | sed 's/?/-/g' | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {  print  >>"Chr_"$2"_des.txt"}}'`
#### - Sorts the data from the joined file (excluding header) based of Chromosome number followed by descending order of position, sed replaces ? with -,and  then pipes it into awk which saves data for each chromosome number into respective files.

## Creating  file for unknown position
 `head -n 1 maize_joined.txt >Chr_unknown.txt`
#### - Creates files with name Chr\_unknown.txt 
`tail -n +2 maize_joined.txt |  awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "unknown") {  print  >>"Chr_unknown.txt"}}'`
#### - Saves all records with unknown position into the file

## Creating  file for multiple positions
 `head -n 1 maize_joined.txt >Chr_multiple.txt`
#### - Creates files with name Chr\_multiple.txt 
`tail -n +2 maize_joined.txt |  awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "multiple") {  print  >>"Chr_multiple.txt"}}'`
#### - Saves all records with multiple position into the file

# Teosinte

## Filtering only Teosinte data from genotype dataset
 
`head -n 1 fang_et_al_genotypes.txt > teosinte_genotypes.txt`

`grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt >>teosinte_genotypes.txt`

## Transposing the filtered data

`awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt`

## Sorting the data before join 
#### The second and third row do not contain genotype data, so the second command skips them while retaining the header
`head -n 1 transposed_teosinte_genotypes.txt > teosinte_sorted.txt`

`tail -n+4 transposed_teosinte_genotypes.txt | sort  -k1,1 >>teosinte_sorted.txt`

## Joining the two files
#### -t "[Ctrl+V, tab ]" = -t "<tab>" in Unix to  retain tab separated format of the output
`join -1 1 -2 1 --header -t " [Ctrl+V, tab ]       " snp_sorted_col.txt teosinte_sorted.txt > teosinte_joined.txt`

## Creating 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
 `head -n 1 teosinte_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 >"Chr_"i"_asc_Teosinte.txt"}'`
#### - Creates files with name Chr\_(Chromosome number)\_asc\_Teosinte.txt for each chromosome with just the header row
`tail -n +2 teosinte_joined.txt | sort -k2,2 -k3,3n | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {  print  >>"Chr_"$2"_asc_Teosinte.txt"}}'`
#### - Sorts the data from the joined file (excluding header) based of Chromosome number followed by ascending order of position, then pipes it into awk which saves data for each chromosome number into respective files.

## Creating 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -
 `head -n 1 teosinte_joined.txt | awk 'BEGIN {FS="\t"; OFS="\t"}{ for (i=1; i <= 10; i++) print $0 >"Chr_"i"_des_Teosinte.txt"}'`
#### - Creates files with name Chr\_(Chromosome number)\_des_Teosinte.txt for each chromosome with just the header row
`tail -n +2 teosinte_joined.txt | sort -k2,2 -k3,3rn | sed 's/?/-/g' | awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 >= 1 && $2 <= 10) {  print  >>"Chr_"$2"_des_Teosinte.txt"}}'`
#### - Sorts the data from the joined file (excluding header) based of Chromosome number followed by descending order of position, sed replaces ? with -, and  then pipes it into awk which saves data for each chromosome number into respective files.

## Creating  file for unknown position
 `head -n 1 teosinte_joined.txt >Chr_unknown_Teosinte.txt`
#### - Creates files with name Chr\_unknown\_Teosinte..txt 
`tail -n +2 teosinte_joined.txt |  awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "unknown") {  print  >>"Chr_unknown_Teosinte.txt"}}'`
#### - Saves all records with unknown position into the file

## Creating  file for multiple positions
 `head -n 1 teosinte_joined.txt >Chr_multiple_Teosinte.txt`
#### - Creates files with name Chr\_multiple\_Teosinte.txt 
`tail -n +2 teosinte_joined.txt |  awk 'BEGIN {FS="\t"; OFS="\t"}{ if($2 == "multiple") {  print  >>"Chr_multiple_Teosinte.txt"}}'`
#### - Saves all records with multiple position into the file





















