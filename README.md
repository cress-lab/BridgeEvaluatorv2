# BridgeEvaluator
This package designs bridgeRNAs targeting a given locus and and provides metrics to evaluate their efficiencies and specificities

*__Author__: Jaymin Patel: jayman1466@gmail.com*

## Installation
### Streamlined Installation
You can install this package via pip:
```
pip install BridgeEvaluator
```
You must also install the [Vienna RNA Suite](https://www.tbi.univie.ac.at/RNA/ViennaRNA/doc/html/install.html) if you want to score the predicted folding of the designed bRNAs. If you don't have this installed, set *score_structure=False* in the *design_bridges()* command. 

If you are getting a package not found error, this can usually be fixed by updating your python and pip versions. It may make sense to do this within a new conda environment. 

### Manual Installation
Alternatively, for manual installation, you can place the files from the __"src/BridgeEvaluator/"__ directory directly into your working directory. If you use this manual installation, make sure you have the following dependencies installed with a __python version >=3.9__:


[biopython >= 1.85](https://pypi.org/project/biopython/)

[Levenshtein >= 0.27.1](https://pypi.org/project/Levenshtein/)

[viennarna >= 2.7.0](https://pypi.org/project/ViennaRNA/)

[pandas >= 2.2.0](https://pypi.org/project/pandas/)


## Usage
The simplest usage within a python script is as follows:
```python
from bridge_evaluator import iterate_bridge_design

#Specify the locus that will be scanned to design bRNAs
target_locus = "ATGAGCAAAGGAGAAGAACTTTTCACTGGAGTTGTCCCAATTCTTGTTGAATTAGATGGTGATGTTAATGGGCACAAATTTTCTGTCCGTGGAGAGGGTGAAGGTGATGCTACAAACGGAAAACTCACCCTTAAATTTATTTGCACTACTGGAAAACTACCTGTTCCGTGGCCAACACTTGTCACTACTCTGACCTATGGTGTTCAATGCTTTTCCCGTTATCCGGATCACATGAAACGGCATGACTTTTTCAAGAGTGCCATGCCCGAAGGTTATGTACAGGAACGCACTATATCTTTCAAAGATGACGGGACCTACAAGACGCGTGCTGAAGTCAAGTTTGAAGGTGATACCCTTGTTAATCGTATCGAGTTAAAGGGTATTGATTTTAAAGAAGATGGAAACATTCTTGGACACAAACTCGAGTACAACTTTAACTCACACAATGTATACATCACGGCAGACAAACAAAAGAATGGAATCAAAGCTAACTTCAAAATTCGCCACAACGTTGAAGATGGTTCCGTTCAACTAGCAGACCATTATCAACAAAATACTCCAATTGGCGATGGCCCTGTCCTTTTACCAGACAACCATTACCTGTCGACACAATCTGTCCTTTCGAAAGATCCCAACGAAAAGCGTGACCACATGGTCCTTCTTGAGTTTGTAACTGCTGCTGGGATTACACATGGCATGGATGAGCTCTACAAAtaa"

#Name of this locus. The results will be outputted with this filename
locus_name = "sfGFP"

#Genbank files of the full genome of the target. This will be used to evaluate possible off targets 
genbank_files = ["MG1655.gb"]

iterate_bridge_design(target_locus, locus_name, genbank_files=genbank_files)
```

### Example python script utilizing some optional parameters:
```python
from bridge_evaluator import iterate_bridge_design

#Specify the locus that will be scanned to design bRNAs
target_locus = "ATGAGCAAAGGAGAAGAACTTTTCACTGGAGTTGTCCCAATTCTTGTTGAATTAGATGGTGATGTTAATGGGCACAAATTTTCTGTCCGTGGAGAGGGTGAAGGTGATGCTACAAACGGAAAACTCACCCTTAAATTTATTTGCACTACTGGAAAACTACCTGTTCCGTGGCCAACACTTGTCACTACTCTGACCTATGGTGTTCAATGCTTTTCCCGTTATCCGGATCACATGAAACGGCATGACTTTTTCAAGAGTGCCATGCCCGAAGGTTATGTACAGGAACGCACTATATCTTTCAAAGATGACGGGACCTACAAGACGCGTGCTGAAGTCAAGTTTGAAGGTGATACCCTTGTTAATCGTATCGAGTTAAAGGGTATTGATTTTAAAGAAGATGGAAACATTCTTGGACACAAACTCGAGTACAACTTTAACTCACACAATGTATACATCACGGCAGACAAACAAAAGAATGGAATCAAAGCTAACTTCAAAATTCGCCACAACGTTGAAGATGGTTCCGTTCAACTAGCAGACCATTATCAACAAAATACTCCAATTGGCGATGGCCCTGTCCTTTTACCAGACAACCATTACCTGTCGACACAATCTGTCCTTTCGAAAGATCCCAACGAAAAGCGTGACCACATGGTCCTTCTTGAGTTTGTAACTGCTGCTGGGATTACACATGGCATGGATGAGCTCTACAAAtaa"

locus_name = "sfGFP"
genbank_files = ["MG1655.gb"]

#Donor A is the native Donor of IS621
donor_seq = 'ACAGTATCTTGTAT'

#Cores to use
cores = ["CT","GT"]

#avoid the following sequences (reverse complements added automatically)
avoid_restriction = ["GGTCTC","GCTCTTC"]

iterate_bridge_design(target_locus, locus_name, genbank_files=genbank_files, donor_seq=donor_seq, cores=cores, avoid_restriction=avoid_restriction)
```

See [iterate_bridge_design()](#iterate_bridge_designtarget_locus-locus_name-kwargs) below for the full parameter reference.

## Output

The results are exported in the present directory as a csv file named __{locus_name}.csv__. Each row represents a potential bridgeRNA that can target the provided locus. See the full column reference under [iterate_bridge_design()](#iterate_bridge_designtarget_locus-locus_name-kwargs) below.

### Key Output Columns

__locus_name__: Name of locus being targeted

__feature_type__: Feature type of locus being targeted (if provided)

__donor_seq__: Donor Sequence (14mer)

__bridge_name__: Unique name for this bridgeRNA

__scaffold__: bridgeRNA scaffold used

__target_seq__: Target Sequence (14mer)

__index__: Position of the Target Sequence relative to target locus

__strand__: Orientation of the Target Sequence (+ or -) relative to the target locus

__core__: Core sequence (2bp) at positions 8–9 of the target

__full_bRNA_seq__: Full sequence of the designed bridgeRNA

__split_TBL_seq__: Target-binding loop sequence for split-bRNA applications

__split_DBL_seq__: Donor-binding loop sequence for split-bRNA applications

__p6p7_warning__: Warning if this bridgeRNA violates preferred handshake rules

__RNA_structural_similarity__: The predicted MFE secondary structure of the designed bridgeRNA is compared to the native IS621 reference using Vienna RNA's RNAforester. A similarity score from 0 to 1 is provided.

__cloning_fragment_seq_Patel_et_al__: The bridgeRNA converted into a DNA fragment for Golden Gate cloning into the bsaI sites in *Patel et al.* IS110 vectors

__fulllength_matches_LevDist_0__: Number of perfect full-length (14mer) matches to the provided genome

__{kmer_length}mer_matches_LevDist_{d}__: Number of matches within the leading `kmer_length` bp at each Levenshtein distance `d` (indels scored as 2, SNPs as 1)

---

## Additional Functions

The following functions provide more granular control over bridgeRNA design and off-target analysis. They are available directly from `bridge_evaluator`.

---

### `iterate_bridge_design(target_locus, locus_name, **kwargs)`

Scans a target locus to identify all permissive target sites and designs a bridgeRNA for each one. Results are exported as a CSV file named `{locus_name}.csv` and returned as a DataFrame.

```python
from bridge_evaluator import iterate_bridge_design

target_locus = "ATGAGCAAAGGAGAAGAACTTTTCACTGGAGTTGTCCC..."
locus_name = "sfGFP"
genbank_files = ["MG1655.gb"]

df = iterate_bridge_design(target_locus, locus_name, genbank_files=genbank_files)
```

#### Required Parameters
__target_locus__: Nucleotide sequence of the locus to scan for target sites.

__locus_name__: Name of the locus. Used to name output bridgeRNAs and the exported CSV file. bridgeRNA names follow the syntax: `bridge_{scaffold}_T_{locus_name}_{index}_D_{donor_name}`.

#### Optional Parameters
__genbank_files__: List of genbank file paths against which to screen off-targets. Required if `check_offtargets=True`. Default is `""`.

__donor_seq__: 14mer donor sequence. Default is the native IS621 donor `"ACAGTATCTTGTAT"`. The core of the donor is automatically updated to match each target site's core.

__donor_name__: Name of the donor, used in bridgeRNA naming. Default is `"1"`.

__cores__: List of 2bp core sequences to screen. Default is `['CT']`.

__scaffold__: bridgeRNA scaffold to use. Options: `"IS621_WT"`, `"IS621_enhanced"`, `"ISCro4_WT"`, `"ISCro4_enhanced"`. Default is `"IS621_WT"`.

__include_left__: Number of bases to include to the left of the core in the target 14mer. Default is `7`.

__include_right__: Number of bases to include to the right of the core in the target 14mer. Default is `5`. Note: `include_left + include_right` must equal 14.

__kmer_length__: Number of leading bp of the target sequence used for off-target kmer matching. Default is `11`.

__primer_seqs__: PCR primer sequences for amplification of cloning fragments, provided as a dictionary keyed by core sequence. Default is `{"CT": ["",""], "GT": ["",""], "AT": ["",""], "TT": ["",""]}`.

__avoid_restriction__: List of restriction site sequences to exclude from designed bridgeRNAs. Reverse complements are added automatically. Default is `["GGTCTC"]`.

__feature_type__: Metadata label for the type of locus being targeted (e.g. `"CDS"`, `"ncRNA"`). Default is `""`.

__check_offtargets__: Whether to screen for off-target sites in the provided genbank files. Default is `True`.

__score_structure__: Whether to score the predicted secondary structure of each designed bridgeRNA against the native IS621 reference using RNAforester. Default is `True`.

__distances__: List of Levenshtein distances to tabulate for off-target analysis. Default is `[0, 1, 2]`.

#### Output Columns
__locus_name__: Name of the target locus.

__feature_type__: Feature type of the locus (if provided).

__donor_seq__: Donor sequence (14mer) used.

__bridge_name__: Unique name for this bridgeRNA.

__scaffold__: bridgeRNA scaffold used.

__target_seq__: Target sequence (14mer).

__index__: Position of the target sequence relative to the target locus.

__strand__: Orientation of the target sequence (`+` or `-`).

__core__: Core sequence (2bp) at positions 8-9 of the target.

__full_bRNA_seq__: Full sequence of the designed bridgeRNA.

__split_TBL_seq__: Target-binding loop sequence for split-bRNA applications.

__split_DBL_seq__: Donor-binding loop sequence for split-bRNA applications.

__p6p7_warning__: Warning if this bridgeRNA violates preferred handshake rules.

__RNA_structural_similarity__: Similarity score (0–1) comparing the predicted MFE secondary structure of the designed bridgeRNA to the native IS621 reference structure. Computed using Vienna RNA's RNAforester.

__cloning_fragment_seq_Patel_et_al__: DNA fragment for Golden Gate cloning into the bsaI sites in *Patel et al.* IS110 vectors.

__fulllength_matches_LevDist_0__: Number of perfect full-length (14mer) matches to the provided genome.

__{kmer_length}mer_matches_LevDist_{d}__: Number of matches within the leading `kmer_length` bp at each Levenshtein distance `d`.

---

### `single_bridge_design(target_seq, **kwargs)`

Designs a single bridgeRNA for a specified 14mer target sequence. Returns a dictionary of design attributes rather than a DataFrame.

```python
from bridge_evaluator import single_bridge_design

result = single_bridge_design("TTTCACCCTGGAGG")
print(result['full_bRNA_seq'])
```

#### Required Parameters
__target_seq__: 14mer target sequence. The core is automatically inferred from positions 8–9 (0-indexed: `target_seq[7:9]`).

#### Optional Parameters
__donor_seq__: 14mer donor sequence. Default is `"ACAGTATCTTGTAT"`.

__scaffold__: bridgeRNA scaffold to use. Default is `"IS621_WT"`.

__kmer_length__: Number of leading bp used for off-target kmer matching. Default is `11`.

__avoid_restriction__: List of restriction sites to flag in the cloning fragment. Default is `[]`.

__genbank_files__: List of genbank file paths for off-target screening. Required if `check_offtargets=True`. Default is `""`.

__check_offtargets__: Whether to compute off-target counts against the provided genbank files. Default is `False`.

__score_structure__: Whether to score predicted secondary structure against the native IS621 reference. Default is `True`.

__distances__: Levenshtein distances to tabulate for off-target analysis. Default is `[0, 1, 2]`.

#### Return Value
Returns a dictionary with the following keys: `target_seq`, `donor_seq`, `scaffold`, `core`, `full_bRNA_seq`, `split_TBL_seq`, `split_DBL_seq`, `p6p7_warning`, `RNA_structural_similarity` (if `score_structure=True`), `cloning_fragment_seq_Patel_et_al`, `restriction_site_warnings` (if restriction sites detected), and off-target count keys (if `check_offtargets=True`).

---

### `off_target_assess(target_seq, genbank_files, **kwargs)`

Evaluates the off-target landscape of a given 14mer target sequence against one or more genbank files. Does not design a bridgeRNA — use this to quickly check specificity of an existing target.

```python
from bridge_evaluator import off_target_assess

genbank_files = ["MG1655.gb"]
result = off_target_assess("TTTCACCCTGGAGG", genbank_files)
print(result)
```

#### Required Parameters
__target_seq__: 14mer target sequence to evaluate.

__genbank_files__: List of genbank file paths defining the genome to screen against.

#### Optional Parameters
__kmer_length__: Number of leading bp used for kmer-based off-target matching. Default is `11`.

__distances__: Levenshtein distances to tabulate. Default is `[0, 1, 2]`.

#### Return Value
Returns a dictionary with the following keys: `target_seq`, `core`, `fulllength_matches_LevDist_0`, and `{kmer_length}mer_matches_LevDist_{d}` for each distance `d`.

---

### `find_unique_targets(genbank_files, **kwargs)`

Scans a genome for the most unique 14mer target sequences (those with the fewest genomic matches) for a given set of core sequences. Useful for identifying landing-pad or safe-harbor target sites.

```python
from bridge_evaluator import find_unique_targets

genbank_files = ["MG1655.gb"]
df = find_unique_targets(genbank_files, cores=["CT", "GT"], cutoff=10)
print(df)
```

#### Required Parameters
__genbank_files__: List of genbank file paths defining the genome to search.

#### Optional Parameters
__cores__: List of 2bp core sequences to search for. Default is `['CT']`.

__kmer_length__: Number of leading bp used for kmer-based off-target matching. Default is `11`.

__distances__: Levenshtein distances to tabulate. Default is `[0, 1, 2]`.

__cutoff__: Number of unique target sequences to return per core. Default is `10`.

#### Return Value
Returns a DataFrame of the `cutoff` most unique 14mer targets, sorted by ascending off-target counts. Columns include `target_seq`, `fulllength_matches_LevDist_0`, and `{kmer_length}mer_matches_LevDist_{d}` for each distance `d`.

---

## References 
[Bridge RNAs direct programmable recombination of target and donor DNA](https://www.nature.com/articles/s41586-024-07552-4)<br>
[Arc Institure Bridge RNA Design Tool](https://bridge.hsulab.arcinstitute.org/)