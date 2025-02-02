# Fungal-Proteomics

## Abstract

Investigating phylogenetics allows researchers to understand organisms' positions within the broader tree of life. Fungi, in particular, possess notable properties like the production of penicillin and the ability to break down complex materials, making them a promising source of novel compounds. However, significant disagreements in the field regarding clade contents makes extrapolating between species difficult and traditional methods for constructing phylogenies often yield conflicting results. To address these challenges, we explored the potential of proteomics for phylogenetic analysis. This study used Uniform Manifold Approximation and Projection (UMAP) to uncover relationships between the protein domains of 886 fungal organisms. By incorporating both local and global relationships, UMAP effectively identified similarities within the fungal proteome. Our results demonstrate that UMAP can reconstruct existing phylogenies from protein domains and potentially resolve classification debates within fungal phylogenetics. UMAP projections, using the 'cosine' metric and k=12 nearest neighbors, formed strong, consistent clusters that aligned with known phylogenetic categories. These findings suggest that proteomics, analyzed through UMAP, provides a valuable tool for constructing accurate phylogenies and furthering our understanding of fungal diversity.

## Introduction 

Investigating phylogenetics allows researchers to understand organisms relative to where they are in the broader tree of life. Knowing an organism's tree allows scientists to more quickly identify special properties or weaknesses, such as knowing the correct antibiotic to a bacterial infection [3]. Many species of fungi have special properties–the most notable being penicillin–making them a promising source of novel compounds. Unfortunately, there is significant disagreement in the field surrounding the contents of certain clades, making it difficult to confidently research related organisms. 
Classically, phylogeny and taxonomy are generated through identifying physical characteristics and genomic/metagenomic analysis. However, these methods often produce conflicting answers, which is particularly troublesome in a kingdom as large as Fungi. Because of the challenges of creating a cohesive phylogenetic tree, it is necessary to broaden the types of available data for investigation. Proteomics provides a potential piece of the puzzle, since it has been used to create phylogenetic trees [1]. 
The protein-based tree in the paper by Choi and Kim (2017) also contains discrepancies with gene-based trees. This paper uses Uniform Manifold Approximation and Reduction (UMAP) to find relationships between the protein domains of 886 fungal organisms. Our aim is to show that UMAP can effectively reconstruct existing phylogenies from protein domains, and potentially help settle debates over the proper classifications of fungi. 

## Results

### UMAP projections 

The data visualizations from the UMAP projections form strong clusters using the ‘cosine’ metric and k=12 nearest neighbors. The clusters appeared to be maintained when the supervised learning function was provided the known phylogenetic category at increasing depths for each organism. 

Fig. 1 2D UMAP projections showing maintained clusters at increasing phylogenetic depth. The supervised learning component moves clusters around, but the content of the clusters remains largely the same. 

The consistency of the clusters indicates that UMAP may be finding relevant patterns in the proteomic dataset that can be used to accurately group related organisms together, even without ground truth labels. Fig. 2 Unsupervised 2D UMAP projection. This projection is made purely from patterns within the proteomic domains. 

In particular yeasts, aspergillus/penicillium, and psilocybe’s retained strong clusters even at a depth of 8. These data are very promising for using proteomics to construct phylogenies. 

Comparison to existing phylogenies 
When clustered, the protein domain embeddings strongly relate to phylogeny. 

Fig. 3 Four identical 2D embeddings, each colored using labels from 2, 6, 10, or 50 dimensions 
When comparing the clusters to the ground truth phylogenetics of each organism, the clusters typically maintain homogenous trees, where >50% of the organisms at the given depth share a category, until a phylogenetic depth of eleven or twelve. Increasing the dimension of the embedding did not change this property. 
Fig. 4 90% of the data in the 6D plot are members of Aspergillaceae, and the rest are related species. However, as the data goes from a 6D embedding to 7D, the clusters merge to form a larger, more robust group. 

Instead, more of the uncategorized data was assigned to a cluster, and clusters were merged, as can be seen in figure 4. Experimentally, the clusters begin to solidify when embedded into a fifteen dimensional space. At this point, the number of uncategorized organisms stops dropping monotonically, and begins to vary slightly across dimensions. 
	The improvements from a 2D embedding cannot be overstated. In two dimensions, 68 organisms could not be adequately clustered, but that number decreased to 23 in just ten dimensions. This data paired with merging similar clusters provides good evidence that phylogenies can be created using protein domains, though it should be noted that higher dimensional relationships are needed to accurately capture the data.  

## Methods

### Data 

This project used data collected from n=886 different fungal organisms, each with n=17143 protein domains that could be analyzed. Each sample also had its previously known phylogeny characterized in a separate CSV file. 
 

Fig. 3 Subset of the existing taxonomy for each organism. The organisms are organized by ID to ease computation. 

Likely due to the fragility of certain fungal species, some phylogenies were left incomplete. In addition, some trees were far more extensive than others, making it necessary to augment the data with a label if one did not exist at that depth. This may have impacted the effectiveness of supervised learning at high depth because it would weight the mapping of organisms by whether or not they had complete trees. 
It is important to note that Dikarya is by far the most commonly characterized clade[4] . In the dataset used, the two subdivisions of Dikarya, Ascomycota and Basidiomycota, comprised 68% and 23% of the samples respectively.


### UMAP

To reduce the dimensionality of the dataset I used Uniform Manifold Approximation and Projection (UMAP). UMAP was selected for its ability to preserve both local and global data structures while providing an interpretable low-dimensional representation. The distance metrics used were ‘euclidean’ and ‘cosine’. Of the two, the ‘cosine’ metric was superior because it favors local correlations, while the ‘euclidean’ takes the distances to all other points into account. The number of nearest neighbors for k-nearest neighbors was also specified to ensure the quality of the clusters. I used supervised learning to create plots that weight the representation by accounting for known phylogenies of varying depths. The code provided also creates interactive Plotly graphs that include hover data, among other features such as zooming and panning, and the points are colored by the phylogenetic depth. 

### Clustering

To generate clusters, protein domains were embedded into an n-dimensional space. Hierarchical Density-Based Spatial Clustering of Applications with Noise (HDBSCAN) was used, as more common methods like k-means become less effective with increased dimensionality. The Louvain algorithm was also tried, but it tended to produce lower quality clusters. 
	To visualize the high dimensional data, a 2D embedding of the data was plotted using a Plotly scatterplot. Since each organism received a hard cluster assignment, the 2D data points could be colored using the same specimen’s membership in the high dimensional clusters. This method was employed to visually ascertain how cluster membership changes when protein domains are embedded in increasing dimensions. 	
	The known phylogenetic tree of the members of a cluster were compared at each phylogenetic depth to determine the accuracy of the cluster. Clusters that maintained a high proportion of similar organisms at high depth were considered to be high quality. 

## Discussion
The application of Uniform Manifold Approximation and Projection (UMAP) to proteomic data in fungal phylogeny reconstruction has shown promising results in capturing the relationships between different fungal species. Our study demonstrates that UMAP can effectively group related fungal organisms based on their protein domain structures, providing insights that align well with established phylogenetic categories. UMAP's projections formed robust and consistent clusters that reflected known phylogenetic categories. This clustering accuracy was particularly notable in groups such as yeasts, Aspergillus/Penicillium, and Psilocybe, which retained strong cluster integrity even at deeper phylogenetic levels.
 One limitation of the project was the incomplete phylogenetic data for some fungal species, which may have impacted the effectiveness of the supervised learning component. In future work, more data should be collected to improve model training. This project also only explored the used proteomic data to create phylogenies. Now that it is clear this type of data can be used, it should be integrated with other ‘omics’ to create more robust phylogenies.
This research highlights the importance of inspecting high-dimensional relationships to accurately capture the data in fungal phylogeny reconstruction. While 2D embeddings provided useful visualizations, higher-dimensional embeddings significantly improved clustering accuracy and reduced the number of unclustered organisms. This research demonstrates that UMAP can effectively group related fungal organisms based on their protein domain structures, providing insights that align well with established phylogenetic categories. 




## References

[1] ​​Choi, J., & Kim, S. H. (2017). A genome Tree of Life for the Fungi kingdom. Proceedings of the National Academy of Sciences of the United States of America, 114(35), 9391–9396. https://doi.org/10.1073/pnas.1711939114

[2] Li, Y., Steenwyk, J. L., Chang, Y., Wang, Y., James, T. Y., Stajich, J. E., Spatafora, J. W., Groenewald, M., Dunn, C. W., Hittinger, C. T., Shen, X. X., & Rokas, A. (2021). A genome-scale phylogeny of the kingdom Fungi. Current biology : CB, 31(8), 1653–1665.e5. https://doi.org/10.1016/j.cub.2021.01.074

[3] Soltis DE, Soltis PS. The role of phylogenetics in comparative genetics. Plant Physiol. 2003 Aug;132(4):1790-800. doi: 10.1104/pp.103.022509. PMID: 12913137; PMCID: PMC526274.

[4] Yan Wang, Ying Chang, Jericho Ortañez, Jesús F Peña, Derreck Carter-House, Nicole K Reynolds, Matthew E Smith, Gerald Benny, Stephen J Mondo, Asaf Salamov, Anna Lipzen, Jasmyn Pangilinan, Jie Guo, Kurt LaButti, William Andreopolous, Andrew Tritt, Keykhosrow Keymanesh, Mi Yan, Kerrie Barry, Igor V Grigoriev, Joseph W Spatafora, Jason E Stajich, Divergent Evolution of Early Terrestrial Fungi Reveals the Evolution of Mucormycosis Pathogenicity Factors, Genome Biology and Evolution, Volume 15, Issue 4, April 2023, evad046, https://doi.org/10.1093/gbe/evad046



