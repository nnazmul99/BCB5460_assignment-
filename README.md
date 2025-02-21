### Data Inspection of ```fang_et_al_genotypes.txt```

Here are the codes used for inspecting ```fang_et_al_genotypes.txt``` 
```sh
1. head -n 10 fang_et_al_genotypes.txt
```

```sh
2. tail -n 10 fang_et_al_genotypes.txt
```

```sh
3. cat fang_et_al_genotypes.txt 
```
```sh
4. awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
```
```sh
5. wc fang_et_al_genotypes.txt
```

### Attributes of ```fang_et_al_genotypes.txt```
1. Displayed the first 10 lines of the file
2. Displayed the last 10 lines of the file
3. Displayed the file
4. Displayed the no of column in the file which is 986
5. Displayed the no of rows , words and characters. According to the result, there was 2783 rows, 2744038 words and 11051939 characters.

### Data Inspection of ```snp_position.txt ```

```sh 
1. head -n 10 snp_position.txt
```
```sh
2. tail -n 10 snp_position.txt
```
```sh
3. cat snp_position.txt
```
```sh
4. awk -F "\t" '{print NF; exit}' snp_position.txt
```
```sh
5. wc snp_position.txt
```
### Attributes of ```snp_position.txt```
1. Displayed the first 10 lines of the file
2. Displayed the last 10 lines of the file
3. Displayed the file
4. Displayed the no of column in the file which is 15
5. Displayed the no of rows , words and characters. According to the result, there was 984 rows, 13198 words and 82763 characters.

### Data Processing
1. Extracting data from ```fang_et_al_genotypes.txt```
```sh
grep -E -w "(Sample_ID|ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt
```
2. Extracting data from ```snp_position.txt```
```sh
cut -f 1,3,4 snp_position.txt > Extracted_snp_position.txt
```
3. Deleting the header and sorting ```Extracted_snp_position.txt```
```sh
grep -v "SNP_ID" Extracted_snp_position.txt | sort -k1,1 Extracted_snp_position.txt > Extracted_snp_position_sorted.txt
```
4. Transposing ```maize_genotypes.txt```
```sh
awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt
```
5. Deleting the header and sorting ```transposed_maize_genotypes.txt```
```sh
grep -v "Sample_ID" transposed_maize_genotypes.txt | sort -k1,1 > transposed_maize_genotypes_sorted.txt
```
6. Joining two files (```Extracted_snp_position_sorted.txt```,```transposed_maize_genotypes_sorted.txt```)
```sh
join -1 1 -2 1 -t $'\t' Extracted_snp_position_sorted.txt transposed_maize_genotypes_sorted.txt > maize_SNP_joined.txt
```
7. 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
```sh
sed 's/unknown/?/g' maize_SNP_joined.txt | sort -k3,3n > maize_sorted.txt
```

```sh
awk '$2~ /1/' maize_sorted.txt > maize_chr1_increase.txt
awk '$2~ /2/' maize_sorted.txt > maize_chr2_increase.txt
awk '$2~ /3/' maize_sorted.txt > maize_chr3_increase.txt
awk '$2~ /4/' maize_sorted.txt > maize_chr4_increase.txt
awk '$2~ /5/' maize_sorted.txt > maize_chr5_increase.txt
awk '$2~ /6/' maize_sorted.txt > maize_chr6_increase.txt
awk '$2~ /7/' maize_sorted.txt > maize_chr7_increase.txt
awk '$2~ /8/' maize_sorted.txt > maize_chr8_increase.txt
awk '$2~ /9/' maize_sorted.txt > maize_chr9_increase.txt
awk '$2~ /10/' maize_sorted.txt > maize_chr10_increase.txt
```
8. 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -
```sh
sed 's/unknown/-/g' maize_SNP_joined.txt | sort -k 3 -r -n > maize_sorted2.txt
```
```sh
awk '$2~ /1/' maize_sorted2.txt > maize_chr1_decrease.txt
awk '$2~ /2/' maize_sorted2.txt > maize_chr2_decrease.txt
awk '$2~ /3/' maize_sorted2.txt > maize_chr3_decrease.txt
awk '$2~ /4/' maize_sorted2.txt > maize_chr4_decrease.txt
awk '$2~ /5/' maize_sorted2.txt > maize_chr5_decrease.txt
awk '$2~ /6/' maize_sorted2.txt > maize_chr6_decrease.txt
awk '$2~ /7/' maize_sorted2.txt > maize_chr7_decrease.txt
awk '$2~ /8/' maize_sorted2.txt > maize_chr8_decrease.txt
awk '$2~ /9/' maize_sorted2.txt > maize_chr9_decrease.txt
awk '$2~ /10/' maize_sorted2.txt > maize_chr10_decrease.txt
```
9. 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)

```sh
grep -w "unknown" maize_SNP_joined.txt > maize_unknown.txt
```
10. 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)
```sh
grep -w "multiple" maize_SNP_joined.txt > maize_multiple.txt
```
### For teosinte
1. Extracting data from ```snp_position.txt```
```sh
cut -f 1,3,4 snp_position.txt > Extracted_snp_position.txt
```
2. Deleting the header and sorting ```Extracted_snp_position.txt```
```sh
grep -v "SNP_ID" Extracted_snp_position.txt | sort -k1,1 Extracted_snp_position.txt > Extracted_snp_position_sorted.txt
```
3. Extracting data from ```fang_et_al_genotypes.txt```
```sh
grep -E -w "(Sample_ID|ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > teosinte_genotypes.txt
```
4. Transposing ```teosinte_genotypes.txt```
```sh
awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt
```
5. Deleting the header and sorting ```transposed_teosinte_genotypes.txt```
```sh
grep -v "Sample_ID" transposed_teosinte_genotypes.txt | sort -k1,1 > transposed_teosinte_genotypes_sorted.txt
```
6. Joining the files
```sh
join -1 1 -2 1 -t $'\t' Extracted_snp_position_sorted.txt transposed_teosinte_genotypes_sorted.txt > teosinte_SNP_joined.txt
```
7. 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?
```sh
sed 's/unknown/?/g' teosinte_SNP_joined.txt | sort -k3,3n > teosinte_sorted.txt
awk '$2~ /1/' teosinte_sorted.txt > teosinte_chr1_increase.txt
awk '$2~ /2/' teosinte_sorted.txt > teosinte_chr2_increase.txt
awk '$2~ /3/' teosinte_sorted.txt > teosinte_chr3_increase.txt
awk '$2~ /4/' teosinte_sorted.txt > teosinte_chr4_increase.txt
awk '$2~ /5/' teosinte_sorted.txt > teosinte_chr5_increase.txt
awk '$2~ /6/' teosinte_sorted.txt > teosinte_chr6_increase.txt
awk '$2~ /7/' teosinte_sorted.txt > teosinte_chr7_increase.txt
awk '$2~ /8/' teosinte_sorted.txt > teosinte_chr8_increase.txt
awk '$2~ /9/' teosinte_sorted.txt > teosinte_chr9_increase.txt
awk '$2~ /10/' teosinte_sorted.txt > teosinte_chr10_increase.txt
```
8. 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -
```sh
sed 's/unknown/-/g' teosinte_SNP_joined.txt | sort -k 3 -r -n > teosinte_sorted2.txt
awk '$2~ /1/' teosinte_sorted2.txt > teosinte_chr1_decrease.txt
awk '$2~ /2/' teosinte_sorted2.txt > teosinte_chr2_decrease.txt
awk '$2~ /3/' teosinte_sorted2.txt > teosinte_chr3_decrease.txt
awk '$2~ /4/' teosinte_sorted2.txt > teosinte_chr4_decrease.txt
awk '$2~ /5/' teosinte_sorted2.txt > teosinte_chr5_decrease.txt
awk '$2~ /6/' teosinte_sorted2.txt > teosinte_chr6_decrease.txt
awk '$2~ /7/' teosinte_sorted2.txt > teosinte_chr7_decrease.txt
awk '$2~ /8/' teosinte_sorted2.txt > teosinte_chr8_decrease.txt
awk '$2~ /9/' teosinte_sorted2.txt > teosinte_chr9_decrease.txt
awk '$2~ /10/' teosinte_sorted2.txt > teosinte_chr10_decrease.txt
```

9. 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)
```sh
grep -w "unknown" teosinte_SNP_joined.txt > teosinte_unknown.txt
```
10. 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)
```sh
grep -w "multiple" teosinte_SNP_joined.txt > teosinte_multiple.txt
```







