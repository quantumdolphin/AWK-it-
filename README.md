# AWKit 🪓  
*A curated collection of practical `awk` one-liners and shell scripting tricks, used in real-world data wrangling pipelines.*

> 📌 From bioinformatics and cheminformatics workflows to quick CSV hacking, this is a living collection of battle-tested snippets.

---

## 📚 Table of Contents

- [📌 Basics](#-basics)
- [🔎 Filtering by Column](#-filtering-by-column)
- [🧮 Math & Aggregation](#-math--aggregation)
- [📈 CSV Manipulation](#-csv-manipulation)
- [🔗 Joins & Grouping](#-joins--grouping)
- [🧵 Text Processing](#-text-processing)
- [🛠️ Shell Integration](#-shell-integration)
- [💡 Miscellaneous](#-miscellaneous)

---

## 📌 Basics

```bash
# Extract column 1 from a CSV
awk -F',' '{print $1}' file.csv

# Print line number and index of a keyword
awk 's=index($0,"target") { print "line=" NR, "start position=" s}'
```

---

## 🔎 Filtering by Column

```bash
# Filter rows where column 6 equals "heme"
awk -F',' '$6=="heme"' input.csv > output.csv

# Filter rows where column 4 is 1
awk -F',' 'FNR>1 { if ($4 ==1) { print $1,$1,$2,$3,$4,$5} }' input.csv
```

---

## 🧮 Math & Aggregation

```bash
# Divide column 3 by column 4, output in column 5
awk -F',' -v OFS=',' 'FNR >1 {$5 = $3 / $4}1' input.csv > output.csv

# Multiply column 3 by 100
awk -F',' -v OFS=',' 'FNR >1 {print $3*100}' input.csv > scaled.csv

# Subtract 17 from column 5
awk -F',' '{print $1 "," $2 "," $3 "," $4 "," ($5-17)}' file.csv

# Sum column 1
awk '{sum+=$1} END{print sum}' file.txt

# Min and max (for single-column files)
awk 'BEGIN{a=1000}{if ($1<0+a) a=$1} END{print a}' file.txt
awk 'BEGIN{a=0}{if ($1>0+a) a=$1} END{print a}' file.txt
```

---

## 📈 CSV Manipulation

```bash
# Add header to a CSV
awk 'BEGIN{print "SMILES,ID"}1' file.csv > file_with_header.csv

# Duplicate column 1
awk -F',' -v OFS=',' 'FNR >1 {print $1,$1,$2,$3}' input.csv

# Extract specific columns
awk -F',' -v OFS=',' '{print $1,$2,$3,$6,$7,$8}' file.csv > subset.csv

# Move first column to the end
awk 'BEGIN{FS=OFS=","} {a=$1; for (i=1;i<NF;i++) $i=$(i+1); $NF=a}1' file.csv
```

---

## 🔗 Joins & Grouping

```bash
# Count how many times a value appears in column 1
awk -F ',' '{print $1}' file.csv | sort | uniq -c

# Group column 2 by column 1 (comma-separated)
awk -F',' '{ a[$1]=($1 in a? a[$1]"," : "")$2 }END{ for(i in a) print i,a[i] }' OFS=',' file.csv

# Join on column 1 using awk
awk -F"," 'FNR==NR{a[$1]=$2;next} ($1 in a) {print $1,a[$1],$2}' file1.csv file2.csv
```

---

## 🧵 Text Processing

```bash
# Extract substring using sed
sed -n 's/.*\(C=CC.\+\=O\).*//p' hits.smi

# Add parentheses/brackets to start or end of line
sed 's/^/\(C/' file.txt
sed 's/$/\)/' file.txt

# Count characters in each line
awk '{ print length }' file.txt

# Filter lines based on length (e.g., 8 chars)
grep -E '^.{8}$' file.txt
```

---

## 🛠️ Shell Integration

```bash
# Extract last 25 entries from multiple CSVs, make comma list
awk -F"," '{print $1}' file.csv | tail -25 | tr '
' ',' > list.txt

# Inline calculator using awk
calc(){ awk "BEGIN { print $* }"; }
calc 7.5/3.2

# Insert line number as first column
awk '{printf "%s,%s\n", NR, $0}' file.csv > numbered.csv

# Pick every 10,000th line
awk 'NR % 10000 == 0' file.smi > sampled.smi

# Convert space-separated to tab-separated
awk -v OFS='\t' '{$1=$1; print}' file.txt
```

---

## 💡 Miscellaneous

```bash
# Remove line if it doesn't start with digit (merge with previous)
awk 'NR==1{c=$0} /^[0-9]/{print c;c=$0} !/^[0-9]/{c=c" "$0} END{print c}' in.csv

# Find lines that are in file2 but not in file1
awk 'FNR==NR {a[$0]; next} !($0 in a)' file1 file2

# Fill empty lines with "null"
awk '!NF{$0="null"}1' input.txt > output.csv

# Convert Excel list to Python-style string list
dos2unix file.txt
awk '{ print "\""$0"\"" }' file.txt | paste -s -d, -
```

---

## 🙌 Credits

Curated and field-tested by me  
Collected during cheminformatics, bioinformatics, and scientific data engineering workflows.  

MIT License.
