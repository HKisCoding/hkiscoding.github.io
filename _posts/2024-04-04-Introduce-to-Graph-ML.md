---
layout: post
title: Introduce to Graph ML
categories: [GraphML, Graph Neural Network]
---

### Abstract
A gentle introduction into Graph Network and how to use it in Machine Learning


## 1.Introduction to Graph
### Introduce
- **Graphs** are a general language for describing and analyzing entities with relations/interations
- Networks are complex:
	- Arbitrary size and complex topolocigcal structure
	- No fixed node ordering or reference point
	- Dynamic and multimodal features
  
<p align="center">
  <img src="/images/graphML/Pasted image 20221101171357.png" alt="Graph Exampe"/>
</p>


- Machine learning Lifecycle in Graph Data:
<p align="center">
  <img src="/images/graphML/Pasted image 20221101171535.png" alt="Graph Exampe"/>
</p>

- **Representation Learning**: Map nodes to d-dims embeddings :luc_arrow_right: similar nodes embedded close together
<p align="center">
  <img src="/images/graphML/Pasted image 20221101171816.png" alt="Graph Exampe"/>
</p>

### Application of Graph ML
Graph ML tasks:
- <span style="color: #FF6645; font-weight: bold">Node classification </span>: predict a properity of a node 
- <span style="color: #FF6645; font-weight: bold">Link prediction </span>: predict whether missing link between 2 nodes. Example: Recommend systems, Recommend related pins.
- <span style="color: #FF6645; font-weight: bold">Graph classification </span>: categorize different graph
- <span style="color: #FF6645; font-weight: bold">Clustering </span>: Detect if nodes form a community
- <span style="color: #FF6645; font-weight: bold">Graph generation, Graph evolution </span>

### Graph Representation
- <span style="color: #0373fc">Components of Network</span>:
	- Objects: nodes / vertices **N**
	- Interactions: links, edges **E**
	- Systems: network, graph **G(N,E)**
- <span style="color: #0373fc">Heterogeneous Graphs</span>:
	**G = (V,E,R,T)**
	- Nodes with node types $v_i \in V$ 
	- Edges with relation types $\left(v_i, r, v_j\right) \in E$
	- node type $T\left(v_i\right)$
	- Relation type $r \in R$



- <span style="color: #0373fc">Bipartite Graph</span>: nodes can be divined into 2 disjoint sets **U** and **V** such that every link connects a node **U** to one in **V**. **U** and **V** are independents set.

<p align="center">
  <img src="/images/graphML/Pasted image 20221101180440.png" alt="Graph Exampe"/>
</p>


- <span style="color: #0373fc">Projected Graph</span> 
<p align="center">
  <img src="/images/graphML/Pasted image 20221101180558.png" alt="Graph Exampe"/>
</p>

- <span style="color: #0373fc">Adjacency Matrix</span>: $A_{ij} = 1$ if there is a link from node i to node j else 0

<p align="center">
  <img src="/images/graphML/Pasted image 20221101180825.png" alt="Graph Exampe"/>
</p>

<p align="center">
  <img src="/images/graphML/Pasted image 20221101180941.png" alt="Graph Exampe"/>
</p>

- Esier to work with <span style="color: #FF6645">Large, sparse Network</span> 
- Quickly retrieve all neighbors of a given node
- Connectivity: Adjacency matrix can be written in a block-diagonal form :obs_right_arrow_with_tail: nonzero elements confined to squares, others being zero 
<p align="center">
  <img src="/images/graphML/Pasted image 20221101181839.png" alt="Graph Exampe"/>
</p>

- <span style="color: #0373fc">Node and edge Attributes</span>:
	- Weight
	- Ranking
	- Type
	- Sign
	- Properties depending on the struture of the rest of the graph
- <span style="color: #0373fc">Connectivity in Directed Graphs</span>:
	- Strongly connected: A-B and B-A path
	- Weakly connected directed graph: connected if disregard the edge directions
<p align="center">
  <img src="/images/graphML/Pasted image 20221101182428.png" alt="Graph Exampe"/>
</p>	


- - - 
## 2.[Traditional Methods for ML on Graphs](http://web.stanford.edu/class/cs224w/slides/02-tradition-ml.pdf)

### **Review** Task:
- Node-level 
- Edge-level
- Graph-level

### Node-level Tasks and Features:
**Goal**: Characterize the structure and position of a node in network:
- <span style="color: #FF6645; font-weight: bold">Node degree</span>: Number of edges the node has
- <span style="color: #FF6645; font-weight: bold">Node centrality</span>: The importance of a node in a graph
- <span style="color: #FF6645; font-weight: bold">Clustering coefficient</span>
- <span style="color: #FF6645; font-weight: bold">Graphlest</span>

**Node centrality:**
1. <span style="color: #0373fc">Eigenvector centrality</span>:
		**Hypothesis**: node $v$ is important if surrounded by important neighboring nodes $u \in N(v)$ 
		**Centrality** of $v$ is the sum of centrality of neighboring nodes:
	$$
        \begin{equation}
        c_v=\frac{1}{\lambda} \sum_{u \in N(v)} c_u
        \end{equation}
    $$
	with $\lambda$ is normalization constant
		Rewrite the equation in matrix form:
	$$\lambda c = Ac$$
	with:
   - **A**: Adjacency matrix
   - **c**: Centrality vector
   - $\lambda$: Eigenvalue
2. <span style="color: #0373fc"> Betweenness centrality </span>
	**Hypothesis**: node is important if it lies on many shortest paths between other nodes
	$$
    \begin{equation}
    c_v=\sum_{s \neq v \neq t} \frac{\#(\text { shortest paths betwen } s \text { and } t \text { that contain } v)}{\#(\text { shortest paths between } s \text { and } t)}
    \end{equation}
    $$
3. <span style="color: #0373fc"> Closeness centrality </span>
		**Hypothesis**: Node is important if it has small shortest path lengths to all other nodes
	$$\begin{equation}
    c_v=\frac{1}{\sum_{u \neq v} \#(\text { shortest paths between } u \text { and } v)}
    \end{equation}
$$

**Clustering Coefficient**:
Measures how connected $v$ neighboring nodes are:
$$\begin{equation}
e_v=\frac{\#(\text { edges among neighboring nodes })}{\left(\begin{array}{c}
k_v\\
2
\end{array}\right)} \in[0,1]
\end{equation}
$$

<p align="center">
  <img src="/images/graphML/Pasted image 20221102175155.png" alt="Graph Exampe"/>
</p>

with $(\begin{array}{c}k_v\\2\end{array})$  is node pairs among $k_v$ neighboring nodes.
=> <span style="color: #FF6645">COUNTS THE TRIANGLES IN THE NETWORK</span> 

**Graphlets:** 
- <span style="color: #0373fc"> Definition </span>: small subgraphs that describe the struvture of node $u$ network neighborhood
- <span style="color: #0373fc"> Goals </span>: Describe network structure around node $u$ 
- <span style="color: #0373fc"> Analogy </span>:
	- Degree: counts num edges that node touch
	- Clustering coefficient counts triangle that a node touches.
	- Graphlet Degree Vector (GDV): count num graphlet that a node touches. Provide a measure of a nodes's local network topology.
- <span style="color: #0373fc"> Induced subgraph </span>: formed from subset of vertices and all the edges connecting the vertices in that subset
- <span style="color: #0373fc"> Graph Isomorphism </span>: 2 graphs contain the same number of nodes and connected in the same way.

<p float="left">
  <img src="/images/graphML/Pasted image 20221102183305.png" width="200" />
  <img src="/images/graphML/Pasted image 20221102183317.png" width="200" /> 
</p>
Rooted connected induced non-isomorphic subgraphs. Ex: 5 nodes can create 29 subgraphs with 73 differents graphlets (different position of root node).
- <span style="color: #0373fc"> Graphlet Degree Vector (GDV) </span>: Count vector of graphlets rooted at a given node.

<p align="center">
  <img src="/images/graphML/Pasted image 20221103092930.png" alt="Graph Exampe"/>
</p>

**Summary:** 
Node feature can be categorized:
- Importance-based features:
	- Node degree
	- Node centrality
	=> **Useful for predicting influential nodes in graph.**
- Structure-based features:
	- Node degree
	- Clustering coefficient
	- Graphlet count vector
	=> **Useful for predicting partcular role a node plays in graph.**

### Link Prediction and features:
**Task:** 
- Predict new link based on existing link
- Node pairs (no link) are rank -> Get top K node pairs
=> **Remove random set of links and predict - Predict a ranked list Link that appear in time $t_{1}$** 

**Overview:**
- <span style="color: #FF6645; font-weight: bold">Distance-based feature</span>: Shortest-path distance between 2 nodes
- <span style="color: #FF6645; font-weight: bold">Local neighborhood overlap</span>: Captures num neighboring nodes shared between 2 nodes.
1. <span style="color: #0373fc"> common neighbors: </span> number of shared nodes.
$$\begin{equation}
	\left|N\left(v_1\right) \cap N\left(v_2\right)\right|
\end{equation}$$

2. <span style="color: #0373fc">Jaccard coefficient:</span> similarly to IOU score
$$\begin{equation}
	\frac{\left|N\left(v_1\right) \cap N\left(v_2\right)\right|}{\left|N\left(v_1\right) \cup N\left(v_2\right)\right|}
\end{equation}$$
3. <span style="color: #0373fc">Adamic-Adar index: </span> Calculate using degree of shared nodes.
$$\begin{equation}
\sum_{u \in N\left(v_1\right) \cap N\left(v_2\right)} \frac{1}{\log \left(k_u\right)}
\end{equation}$$

Limit: 2 Nodes with no neighbors in common -> always 0.
- <span style="color: #FF6645; font-weight: bold">Global neighborhood overlap</span>: local neighborhood features return 0 if 2 nodes have no neighbors in common 
=> Consider entire graph
<span style="color: #0373fc">Katz index: </span> count the number of walks of all lenghths between a given pair of nodes. 
Compute the walks of length 1 (directed walk) between $u$ and $v$ (node1 and node2)

<p float="left">
  <img src="/images/graphML/Pasted image 20221201175439.png" width="200" />
  <img src="/images/graphML/Pasted image 20221201175455.png" width="200" /> 
</p>


To compute walk of lengths 2:
- **step1:** Compute walks of length 1 between each of $u$ neightbor and $v$ 
- **step2:** Sum up these walks accross $u$ neighbors
$$\begin{equation}
P_{u v}^{(2)}=\sum_i A_{u i} * P_{i v}^{(\mathbf{1})}=\sum_i A_{u i} * A_{i v}=A_{u v}^2
\end{equation}$$
$A_{u v}^l$ specifies walks of length l.
<span style="color: #0373fc">Katz index: </span>between $v_1$ and $v_2$ is calculated as **Sum over all walk lengths** 
$$S_{v1 v2} =\sum_{i=1}^{\infty} \beta^i \boldsymbol{A}^i = (I - \beta A)^-1 - I $$
$0 < \beta <1$: discount factor

**Summary:** 
- Distance-based features
- Local neighborhood overlap
- Global neighborhood overlap