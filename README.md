# DDI-DG

**Important Notice: All resources in this link are anonymous**

This link contains code, data, necessary figures (Fig-add 1 to Fig-add 4) and tables (Table-add 1 to Table-add 3).

![fig-add1](https://github.com/DoubleBlindAnonymous/DDI-DG/blob/main/add_result/figadd1.png)

**Fig-add 1**: Statistical distribution of two datasets. Drugbank contains 86 categories: the distribution of categories is unbalanced. Twosides contains 963 categories: the distribution of categories is balanced. In Figure 6 of submission (as shown in the figure below), the prediction distribution on the target domain is shown. For the first row of Figure 6, our method is trained on Drugbank, tested on Twosides, and iteratively optimizes the prediction results (1st row 1st column to 1st row 4th column). Twosides is unseen domain, so the model does not know Twosides' distribution, but the t-sne results in the first row present uniform clusters, which correspond to the statistical distribution of the right panel in Fig-add 1 (statistical distribution of Twosides). Similarly, the t-sne results in the second row correspond to the statistical distribution of the left panel in Fig-add 1 (statistical distribution of Drugbank).
Taking the t-sne result in the second row as an example, when the number of iterations increases, the DDI embedding ddi_r will increase the discriminative information corresponding to the unseen domain, so the intra-cluster difference is reduced, the inter-cluster difference is increased. DDI embedding is unsupervised fitted to the unseen domain, and the accuracy is improved.

![fig6](https://github.com/DoubleBlindAnonymous/DDI-DG/blob/main/add_result/subm6.png)

**Figure 6 (from submission)**: We test two scenarios of domain generalization: Drugbank (source) to Twosides (target) and Twosides (source) to Drugbank (target).

***
**Additional note: Although the t-sne result will change when we run it per time, we can still see whether the distribution of the data is uniform, whether it is tight or scattered.**
***

![fig-add2](https://github.com/DoubleBlindAnonymous/DDI-DG/blob/main/add_result/figadd2.png)

**Fig-add 2**: Using heatmaps to visualize tensors in computation. Fig-add 2 corresponds to Figure 3 of submission (as shown in the figure below), which is an illustration of substructure interaction layer.
In Fig-add 2, for a drug pair, we calculated the interaction vector of all substructure pairs (such as ssi_ij in Fig-add 2) and the corresponding weight, and it can be seen that the largest weight can index substructure i and j. Each weight is used to correct the corresponding substructure pair to obtain a posterior interaction vector (such as postssi_ij in the figure). Concatenating these vectors to get the Substructure interaction matrix ssim, and we sum the matrix to get the DDI embedding ddi_r ("Sum" represents aggregating the information of all substructure pairs).

![fig3](https://github.com/DoubleBlindAnonymous/DDI-DG/blob/main/add_result/subm3.png)

**Figure 3 (from submission)**: Substructure interaction layer.

![fig-add3](https://github.com/DoubleBlindAnonymous/DDI-DG/blob/main/add_result/figadd3.png)

**Fig-add 3**: Statistical analysis of explainability. For a drug pair, we can index the corresponding substructure pair according to the maximum attention weight. It is known that GMPNN can extract size-adaptive substructure. For two substructures in a substructure pair, if the maximum hop is g, we will denote the substructure pair as g-hop substructure (the node of the drug graph represents the center of the substructure , we let the order of the farthest neighbor node from the central node be the hop of this substructure).

Generally, 5-hop means that the substructure pair contains size-large chemical functional group, and 3-hop means that the pair does not contain size-large chemical functional group.
First, we count the frequencies of 3-hop and 5-hop in each category. It is found that the statistical distributions of 3-hop and 5-hop are significantly different in both Drugbank and Twosides datasets, respectively (two bar plots on the upper left).

For the sankey plot in the lower left, we select drug pairs on the subset of categories0-6 in Drugbank, use the label of Drugbank as reference (restrict predicted categories to Drugbank categories) for prediction, and then use the label of Twosides as reference (restrict predicted categories to Twosides categories). It is found that the samples with predicted results of Drugbank category 0 and 6 are assigned to Twosides category 0 and 1, and the samples with predicted results of drugbank categories 1-5 are assigned to twosides category 961. At the same time, we noticed a phenomenon: for categories 0-6 on Drugbank (category 0 and 6 in the frequency bar plot correspond to 5-hop, and the rest correspond to 3-hop), for categories 0, 1 and 961 on Twosides (category 0 and 1 in the frequency bar plot correspond to 5-hop, 961 corresponds to 3-hop).

The relationship between Drugbank categories and Twosides categories is provided in the gray box on the right of Fig-add 3, which can prove that our method is reasonable and explainable.

***
**Additional note: In Fig-add 3 sankey plot, Drugbank 0-6 and Twosides 0, 1 and 961 are partial presentations (in order to better demonstrate the explainable analysis). We've done a full statistic on the category assignments from Drugbank 0-86 to Twosides 0-963, and the results can be found in** `./add_result/skany.txt`
***

![fig-add4](https://github.com/DoubleBlindAnonymous/DDI-DG/blob/main/add_result/figadd4.png)

**Fig-add 4**: Unsupervised domain generalization framework. For the intuition behind distribution alignment: First, we use the substructure interaction layer to learn domain-invariant representations (DDI embedding ddi_r). The domain-invariant representation ensures that the predicted target distribution approximates the true distribution of the unseen domain, which can reduce the difficulty of aligning two distributions. Secondly, in the process of distribution alignment, the auxiliary distribution can be regarded as the sharpening of the initial predicted target distribution, and the auxiliary distribution can increase the DDI prediction probability with high confidence, thereby refining the target distribution. We achieve clustering on the target distribution by minimizing the KL divergence of the auxiliary and target distributions. For the evaluation metric of domain generalization, we first use a bipartite graph matching algorithm to match the predicted categories of the cluster centroids with the ground truth categories, and then calculate the accuracy.

**Table-add 1**：Dataset Statistics.
| Dataset  | Number of drug | Drug pairs | Categories |
| ------------- | ------------- | ------------- | ------------- |
| Drugbank  | 1,706  | 191,808 | 86 |
| Twosides  | 645  | 4,576,287 | 963 |

**Table-add 2**：Atom or node feature (*One-hot). For each drug, a graph was generated where bonds were represented as edges, and atoms as nodes. 
| Name  | Description | Dim |
| ------------- | ------------- | ------------- |
| Atom type  | Heavy atom type (e.g., C, O, N, S, I)  | Heavy atom numbers* |
| Degree  | Number of covalent bonds [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  | 11* |
| Implicit valence  | Implicit valence of the atom [0, 1, 2, 3, 4, 5, 6,]  | 7* |
| Hybridization  | [sp, sp2, sp3, sp3d, sp3d2]  | 5* |
| Aromatic  | Whether the atom is part of an aromatic system  | 1 |
|Formal charge|Formal charge of the atom|1|
|Radical electrons|Number of radical electrons for the atom|1|

**Table-add 3**: Bond or edge feature (*One-hot).
| Name  | Description | Dim |
| ------------- | ------------- | ------------- |
|Bond type|[single, double, triple, aromatic]|4*|
|Conjugated|Whether the bond is part of a conjugated system|1|
|Ring|Whether the bond is part of a ring|1|
