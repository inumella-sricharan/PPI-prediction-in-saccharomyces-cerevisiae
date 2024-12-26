# PPI-prediction-in-saccharomyces-cerevisiae
 Analysis of protein interactions. Figuring out important proteins according to the graph structure.  Link prediction with graph convolutional networks.


### <ins>EDA:</ins> <br>
The EDA was done on interactions with confidence score >= 700. For two key reasons:<br>
1) The number of nodes to fit on screen apply force directed layout algorithms becomes large. My Gephi application started to crash while I was attempting to run the force directed layout algorithms.
2) We want to only gain a high level understanding of patterns in the interactions. We can omit the minute details(only while performing EDA).

As the first step of EDA, pagerank algorithm was run on the graph constructed out of confident(>= 700) interactions. The nodes(proteins) were sized according to their degree and color-graded with their pagerank score. Among the confident interactions, further proteins were filtered out which had in-degree less than 20(i.e those proteins that are involved in less than 20 interactions).<br><br>

Now, the force directed layout algorithms are applied to give structure to the interactions network.<br><br>

#### Force directed layout algorithms config:
|***Fruchterman Reingold***|***Force Atlas 2***|
|:------------:|:------------:|
|**Area** : 20000.0|**Scaling** : 15.0|
|**Gravity** : 10.0| **Gravity** : 30.0|
|**Speed** : 10.0|**Prevent overlap** : True|
|          |**Dissuade Hubs** : True|

The dissuade hubs parameter helped in identifying key proteins that bridge the clusters. Also, some of these ***'cluster bridging'*** proteins have a high pagerank score. Proteins like ***YLR167W, YKR094C and YIL148W*** are among the cluster bridging proteins which have a high pagerank score.<br><br>

1) YLR167W (Mcm2-7 Complex Member): This protein is essential for DNA replication, which is fundamental to cell division and growth. Without the Mcm2-7 complex, cells cannot replicate their DNA properly, leading to issues in cell proliferation.
2) YKR094C (Orc1-6 Complex Member): Similarly, this protein is vital for initiating DNA replication. The Origin Recognition Complex (ORC) is necessary to start the replication process, and without it, cells would fail to duplicate their DNA accurately, which could lead to cell death or malfunction.
3) YIL148W (Ubiquitin and Ribosomal Protein): This protein is crucial for protein synthesis. Ubiquitin is involved in targeting proteins for degradation, ensuring that damaged or unneeded proteins are removed. Additionally, the ribosomal protein is essential for building ribosomes, the molecular machines that synthesize new proteins.<br>

Some of the other proteins that got a high pagerank score are ***YNL030W, YBR009C, YBR010W, YBL003C and YNL031C***.<br>

1) YNL030W (HHF2): This gene encodes Histone H4, a core histone protein essential for chromatin assembly and chromosome function. It contributes to telomeric silencing and helps maintain genomic integrity.
2) YBR009C (HHF1): Similar to YNL030W, this gene also encodes Histone H4 and plays a crucial role in chromatin assembly, chromosome function, and telomeric silencing.
3) YBR010W: This gene encodes a protein involved in the pre-replication complex, which is important for DNA replication and maintaining centromeric chromatin stability.
4) YBL003C: This gene encodes a protein that is part of the Origin Recognition Complex (ORC), which is essential for the initiation of DNA replication.
5) YNL031C: This gene encodes a protein involved in the maintenance of telomeres and chromosomal stability.



