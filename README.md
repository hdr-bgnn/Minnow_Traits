# minnowTraits
Using trait segmentation to understand minnow trait evolution across an ecological gradient.

## Minnow image selection
Images were previously segmented using machine learning (<a href="https://github.com/hdr-bgnn/BGNN-trait-segmentation">BGNN trait segementation</a>).

We used the combined violation tables merged with the image quality metadata and image metadata (<a href="https://drive.google.com/file/d/1rrXSM77S7iduVbNogI-_0bucrpqZdzvM/view?usp=sharing">fish.meta.qual.tax.csv</a>), this was done using this <a href=
"https://drive.google.com/file/d/13o_ComN2cNaZxT_gqjqFi6_If0FFavBQ/view?usp=sharing">R code</a>. See the <a href="https://drive.google.com/file/d/1mtSAuxQKvctaUp4ksPPGYpsvNzsoypc9/view?usp=sharing">Batch Info</a> for how these violation tables were made and <a href="https://drive.google.com/file/d/1H0AQSLY3-Akr4DFa2zYo8JJ_N_ET0I7m/view?usp=sharing">CSV Info</a> for information about the column headers.

The minnow.selected.csv file was derived from the fish.meta.qual.tax.csv, and filtered on institution.y == “INHS”, specimen.viewing == “left”, as well as only one “blob” for the head and the eye (CC.HEAD == 1, CC.EYE == 1). 


### 1- Creation of minnow.images.for.segmenting.csv

R code (Minnows.R) was used to filter out high quality, minnow images using the Image_Quality_Metadata_v1_202111206_151204.csv matching the following criteria:

List of criteria chosen :

* family == "Cyprinidae" 
* specimen_viewing == "left" 
* straight_curved == "straight" 
* brightness == "normal" 
* color_issues == "none" 
* has_ruler == "True" 
* if_overlapping == "False" 
* if_focus == "True"
* if_missing_parts == "False"
* if_parts_visible == "True"
* fins_folded_oddly == "False"
* at least 10 images per species
* from either INHS or UWZM institutions
    - Note: there currently is not any image quality metadata for UWZM, so this institution is omitted
* no duplicated original_file_names

**The resulting dataset hs 50 species and 8791 images.**

We ignored if_background_uniform == "True" because it reduced the sample size too much.

The resulting dataset was then merged with the Image_Metadata_v1_20211206_151152.csv.

### 2- Minnow trait selection

![Minnow Landmarks](https://github.com/hdr-bgnn/minnowTraits/blob/main/Traits/Minnow%20Landmarks.png)
![Minnow Measurements](https://github.com/hdr-bgnn/minnowTraits/blob/main/Traits/Minnow%20Length%20Traits.png)

#### Which traits & why
(subject to change)

**Standard length** (edge of head to beginning of caudal fin along nose line) [done in Nagel & Simons 2012 where they showed DNA aligned with morphological data for Nocomis; also done in Burress et al. 2016 looking at benthic-pelagic transition in NA minnows]

**Eye size to head size (area)** (use pixels and convert using scale bar) This would be useful if we can get even a qualitative measure of how turbid the waters are. Area from the segmentation is different from the area researchers tend to take, however.

**Length from back of head to end of caudal peduncle** (back of head in white to beginning of caudal fin on dorsal side)

**Eye diameter** (anterior-posterior length of eye segmentation) [Burress et al. 2016]

**Head length** (tip of snout to posterior tip of opercle) (anterior-posterior length of head segmentation) [Burress et al. 2016]

**Head depth** (length of head dorso-ventrally measured from head/trunk junction ventrally to end of preopercle (?)) [Burress et al. 2016]

**Snout length** (anterior tip of head to anterior eye) [Burress et al. 2016]

**Fin and eye positions** (a series of landmarks; we can use the segmentation to our advantage: [Armbruster 2012]

1. Anterior portion of head
2. Posterior-dorsal edge of head
3. Anterior insertion of dorsal fin
4. Posterior insertion of dorsal fin which may not be possible
5. Anterior-dorsal caudal fin connection with the trunk
6. Posterior caudal fin connection with trunk
7. Anterior-ventral caudal fin connection with trunk
8. Posterior anal fin insertion with may not be possible
9. Anterior anal fin insertion
10. Anterior pelvic fin insertion
11. Anterior pectoral fin insertion
12. Posterior part of head segmentation
13. posterior of eye
14. anterior of eye
15. Ventral part of head, where it connects with the trunk)
