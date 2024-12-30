# PPI-prediction-in-saccharomyces-cerevisiae
 Analysis of protein interactions. Figuring out important proteins according to the graph structure.  Link prediction with graph convolutional networks.

### <ins>Intro:</ins> <br>
This is project is about predicting physical protein interactions of the baker's yeast organism(saccharomyces cerevisiae). (I hardly know anything about biology but, decided to make a dent in this)

### <ins>EDA:</ins> <br>
The EDA was done on interactions with confidence score >= 700. For 3 key reasons:<br>
1) The interactions with low confidence scores (<= 300) can be assumed to not occur in the organism(in this case, saccharomyces-cerevisiae).
2) The interactions with confidence scores( around 500) do not hit the threshold to fully confirm they occur in the organism or not.
3) We want to only gain a high level understanding of patterns in the interactions, I am ignoring the subtle details . That is available with interactions having a high confidence scores.<br>

As the first step of EDA, pagerank algorithm was run on the graph constructed out of confident(>= 700) interactions. The nodes(proteins) were sized according to their degree and color-graded with their pagerank score. Among the confident interactions, further proteins were filtered out which had in-degree less than 20(i.e those proteins that are involved in less than 20 interactions).<br>

(That is because the number of nodes to fit on screen to apply force directed layout algorithms becomes large. My Gephi application started to crash while I was attempting to run the force directed layout algorithms.) <br>

Now, the force directed layout algorithms are applied to give structure to the interactions network.<br><br>

#### Force directed layout algorithms config:
|***Fruchterman Reingold***|***Force Atlas 2***|
|:------------:|:------------:|
|**Area** : 20000.0|**Scaling** : 15.0|
|**Gravity** : 10.0| **Gravity** : 30.0|
|**Speed** : 10.0|**Prevent overlap** : True|
|          |**Dissuade Hubs** : True|

The **dissuade hubs** parameter helped in identifying key proteins that bridge the clusters of proteins. Also, some of these ***'cluster bridging'*** proteins have a high pagerank score. Proteins like ***YLR167W, YKR094C and YIL148W*** are among the cluster bridging proteins which have a high pagerank score.<br><br>

1) **YLR167W** (Mcm2-7 Complex Member): This protein is essential for DNA replication, which is fundamental to cell division and growth. Without the Mcm2-7 complex, cells cannot replicate their DNA properly, leading to issues in cell proliferation.
2) **YKR094C** (Orc1-6 Complex Member): Similarly, this protein is vital for initiating DNA replication. The Origin Recognition Complex (ORC) is necessary to start the replication process, and without it, cells would fail to duplicate their DNA accurately, which could lead to cell death or malfunction.
3) **YIL148W** (Ubiquitin and Ribosomal Protein): This protein is crucial for protein synthesis. Ubiquitin is involved in targeting proteins for degradation, ensuring that damaged or unneeded proteins are removed. Additionally, the ribosomal protein is essential for building ribosomes, the molecular machines that synthesize new proteins.<br>

Some of the other proteins that got a high pagerank score are ***YNL030W, YBR009C, YBR010W, YBL003C and YNL031C***.<br>

1) **YNL030W** (HHF2): This gene encodes Histone H4, a core histone protein essential for chromatin assembly and chromosome function. It contributes to telomeric silencing and helps maintain genomic integrity.
2) **YBR009C** (HHF1): Similar to YNL030W, this gene also encodes Histone H4 and plays a crucial role in chromatin assembly, chromosome function, and telomeric silencing.
3) **YBR010W**: This gene encodes a protein involved in the pre-replication complex, which is important for DNA replication and maintaining centromeric chromatin stability.
4) **YBL003C**: This gene encodes a protein that is part of the Origin Recognition Complex (ORC), which is essential for the initiation of DNA replication.
5) **YNL031C**: This gene encodes a protein involved in the maintenance of telomeres and chromosomal stability.

### <ins>Graph Convolutional Network:</ins> <br>

GCNs are very similar to dense neural networks. Each layer has an input set of vectors and a weight matrix to transform them. <br>
The additional aspect is the accumulation of neighbouring feature vectors of a node and combining them with its own feature vector. This is formally known as 'message passing'. Happens in two steps:<br>
 1) Aggregation (How to combine the feature vectors of a node's neighbours)
 2) Combination (How to combine the aggregated result with the node's feature vector).<br>

Both these steps can be acheived by using adjacency matrix at every layer.(Because through it, we can pin-point the neighbours of every node).<br><br>
Below is the equation for a single layer in GCN<br>


$H^{(l+1)} = \tilde{D}^{-1/2} \tilde{A} \tilde{D}^{-1/2} H^{(l)} W^{(l)}$<br><br>
$\text{Here, } H^{(l)} \text{   is the feature matrix of layer l}$<br><br>
$\text{And,   } \tilde{D}^{-1/2} \tilde{A} \tilde{D}^{-1/2} \text{   is the normalized adjacency matrix(for balancing the mix of feature vectors from the neighbours)}$<br><br>
$\text{And,   } \tilde{A} \text{is the adjacency matrix A with self-loops included}$<br>
$\text{And,   } \tilde{D} \text{is the degree matrix. Only the diagonal entries contain the degrees of the nodes.}$<br><br>

### <ins>Idea:</ins> <br>

With GCN, our main task is to predict wether an interaction occurs between a pair of proteins or not. Formally, this is an edge prediction task with GCN, i.e to predict wether an edge exists between a pair of nodes in the graph or not.<br>

Since GCNs mainly work with embeddings of nodes, I have chosen to form embeddings by processing the descriptions of the proteins that are part of the physical network of this organism in the Strings database(mention about the strings database and the file names). With the help of the sentence transformer model (mention the model name here), I took the protein descriptions and generated their respective embeddings.<br>

Note : These embeddings are to be frozen throughout the training process, otherwise the GCN can carve shortcuts to minimize the loss function by modifying the embeddings to its convenience. we may fall into an overfitting trap if that happens.<br>

### <ins>Results:</ins> <br>

Validation AUC: 0.9777, AP: 0.9801<br>
Test AUC: 0.9818, AP: 0.9836







