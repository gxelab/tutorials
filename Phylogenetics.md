### Phylogenetic Resources
Collection of phylogenetic resources (In progress). The list below is far from complete. New items and links to relevant benchmark papers will be included in the future.

-----------------------------------------------------------------------------

#### All in one
##### [MEGA](https://www.megasoftware.net/)

-----------------------------------------------------------------------------

#### Multiple Sequence Alignment
#### [ClustalW](http://www.clustal.org/)

#### [Muscle](https://www.drive5.com/muscle)
MUSCLE was configure for best accuracy [by default](https://www.drive5.com/muscle/manual/accurate.html). Parameters for fastest speed is available [here](https://www.drive5.com/muscle/manual/fastest.html). It could also build UPGMA or NJ tree.

#### [Prank](http://wasabiapp.org/software/prank/)
Manual available [here](http://wasabiapp.org/software/prank/). Prank could back-translate AA alignment to DNA alignment. Note:
> `-F` specifies that the inference of insertions should be trusted and sites appearing as insertions should not be aligned at the later stages of the process ... if option -F is omitted, the algorithm does not incorrectly penalise for gaps for insertions but it will also not guarantee to leave the insertions unmatched.

#### [MAFFT](https://mafft.cbrc.jp/alignment/software/)
Official suggestions for choosing which program to use are available [here](https://mafft.cbrc.jp/alignment/software/about.html). Manuals available at [Algorithms](https://mafft.cbrc.jp/alignment/software/algorithms/algorithms.html) and [Tips](https://mafft.cbrc.jp/alignment/software/tips0.html).

-----------------------------------------------------------------------------

#### Phylogenetic tree resconstruction
##### [IQ-TREE](iqtree.org)
IQ-TREE could perform model selection, Maximum-likelihood tree reconstruction, and branch support assessment by bootstrap.
[Documentation](http://www.iqtree.org/doc/) is comprehensive and well-organized.

##### [RAxML-NG](https://github.com/amkozlov/raxml-ng)
model selection, Maximum-likelihood tree reconstruction. Documentation available from:
- [wiki](https://github.com/amkozlov/raxml-ng/wiki)
- [Using RAxML-NG in Practice](https://www.preprints.org/manuscript/201905.0056/v1)

##### [PhyML](https://github.com/stephaneguindon/phyml)
Estimate maximum likelihood phylogeny. Support a large number of substitution models coupled to various options to search the space of possible tree topologies.

- Documentation: [manual](https://raw.githubusercontent.com/stephaneguindon/phyml/master/doc/phyml-manual.pdf)
- Online server: [ATGC: PhyML](http://www.atgc-montpellier.fr/phyml/)

-----------------------------------------------------------------------------

#### Hypothesis testing
##### [PAML](http://abacus.gene.ucl.ac.uk/software/paml.html)
Classic. ML based. Tests include:
- Rate heterogeneity among sites (M0-M3)
- Positive selection (M1a-M2a, M7-M8, M8a-M8)
- Branch-site test of positive selection
- Free-ratio model (rate heterogeneity among branches)

##### [HyPhy](https://www.hyphy.org/)
ML based. Tests supported include: aBSREL, BUSTED, FUBAR, MEME, etc. Manual and Methods are comprehensive.

-----------------------------------------------------------------------------

#### Online visualization tools
Visualize and manipulate tree
- [Interactive Tree Of Life](https://itol.embl.de/): display, annotation and management of phylogenetic trees.
- [Phylotree.js](http://phylotree.hyphy.org/): visualize and label branches in phylogenetic trees (for use in branch or branch-site models in HyPhy and PAML). 

Visualize MSA
- [MView](https://www.ebi.ac.uk/Tools/msa/mview/)

#### Local visualization tools
MSA
- [JalView](http://www.jalview.org/)
- [AliView](https://github.com/AliView/AliView)

Tree
- [FigTree](http://tree.bio.ed.ac.uk/software/figtree/)

------------------------------------------------------------------------------

#### Format conversion
Online
- [EMBOSS Seqret](https://www.ebi.ac.uk/Tools/sfc/)

