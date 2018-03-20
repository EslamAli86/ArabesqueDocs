# Component diagram of computation engines

This document describes the architecture and interaction of the computation engines (Master/Workes)



<p align="center"> 
<img src="./resources/ClassDiag/Engines/ODAGs_Engines_relations.jpg" alt="Computation engines component diagram">
</p>


<br>
<font size="4">The master engine repeats the following steps till no more possible results or reaches the desired embedding/pattern size:</font>

<font color="blue">1.</font> Creates a *stash* of ODAGs (embty for the first step only)
<font color="blue">2.</font> Creates a list of **ODAGEngine** objects, each is associated with a partition and has a unique idenifier **partitionId** (Usually the number of partitions is equivalent to the number of threads)
<font color="blue">3.</font> Broadcasts the *stash* to the workers, such that all the engines residing in a single worker share the same copy of the ODAG stash.
<font color="blue">4.</font> Waits till all engines finish hteir computing their partitions. Then retrieve and aggrgate the new ODAGs in order to send them to the new engines in the next step.

<br>
<font size="4">Each engine performs the following steps:</font>

<font color="blue">1.</font> Determines which enumerations/embeddings to read from each ODAG. This set of enumerations is generated from an ODAG using a load balancing algorithm mentioned in the paper in section 5.3
<font color="blue">2.</font> For each embedding read, the engine expands it generating a set of new embeddings
<font color="blue">3.</font> The new expanded embeddings go through the filter/process operations of the Arabesque programming model
<font color="blue">4.</font> The set of embeddings survived from the filteration phase are being added to the newEmbedds ODAG stash. Those new stashes are being aggregatd from the engines and retrieved by the master engine
