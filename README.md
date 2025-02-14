# This documentation for this project is being updated at the moment (Feb 13th).


# Code for paper *The computational core of gene regulatory networks in bacteria*

The following code is for reproducing the analysis and results of the paper [The computational core of gene regulatory networks in bacteria](https://arxiv.org/abs/2310.10895). The purpose of the analysis is to reveal the subnetwork of a message-passing network that controls the rest of the system. We applied this analysis to gene regulatory networks (GRNs) to reveal their computational core, as the name implies, specifically, we analyzed the GRNs of E. coli and B. subtilis. 

The analysis consists of obtaining the fibers of the network, synchronized clusters, collapsing the fibers into a single representative node for each fiber (dynamics are preserved). After these the k-core decomposition is taken to determine the k-out = 1 core of the network. This corresponds to the minimal network, or computational core, that drives the rest of the system. The remaining corresponds to searching for logic circuits in the minimal network and for the simple directed cycles in the strongly connected components that connect the different logic circuits. Additionally, a statistical test is conducted to determine the likelihood that the structures observed are formed by pure chance alone.

The code is written on R. You can download R here: https://cran.r-project.org/, click on the "base" link under "Subdirectories" and download the latest version of R for your system. We recommend working with R-studio, a user friendly IDE you can obtain here: https://posit.co/downloads/

The flow is the following:
1. Get fibers for the network using the script **fiber.R** from the directory **Code_for_fibers**
2. Use **Clean_paper.R** to obtain the *Minimal Network* (the core of the original entire network).
3. Use **circuits_luis.R**, **cycles.R** to find the circuits and the cycles in the network, correspondigly.
4. The **randomized.R** script is for obtaining a randomized version of the original network that still preserves the degree distribution (i.e. a re-wiring) to know what would be the core structures for randomly generated networks and thus compare the likelihood of the actual observed structures in the real networks arising by chance

## 1. Code_for_fibers.R

This is a modification of an older version of the code that were made available on [this](https://github.com/makselab/fibrationSymmetries) repository. Most of the changes made were so that it would be easier to work with the rest of the code for the paper. (Its important to note that all the C++ scripts need to be on the same directory as **fiber.R**)

The script **fiber.R** runs all of the scripts in C++ to obtain the coloring of the nodes, or fibers, of the network. This script calls the functions on the **functions.R** script, firstly for preparing the network files to be run on the C++ code and later to retrieve the obtained colors. After the colors are obtained the script calls **classifier.R** to classify the building blocks. 

Run the script **fiber.R**, specify the location of the network file as well as the separating character. The network file must be a list of edges in the format: Source node (first col), Target node (second col) and Type of edge (optional). The output will be two data frames, one with the list of nodes in the network with a column for FiberId (the color or fiber of the node) as well as another data frame with the list of fiber building blocks and their classifications.

## 2. Clear_paper.R

This script requires both the network file (in an edge -> edge format) and a file with the list of nodes and their colors or fibers. The script first collapses the network to its base, i.e. collapses all the fibers to a single representative node per fiber. This reduced network still preserves the same information flow dynamics. The base of the network is further reduced by applying the kcore decomposition to obtain the k<sub>out</sub>=0 core of the network. 

## 3. circuits_luis.R and cycles.R

Both of this scripts take the network file and returns a list of all the circuits/cycles found, it also classify the circuits according to the type of edges (different types of edges produce different logical circuits).

## 4. randomized.R 

Again, takes the network files, outputs an ensemble of random networks (a rewiring of the original network, i.e. the null model) that still preserve the degree distribution and then analyzes the structure at the core of these randomized networks to give a z-score as a notion of how close to the expected structure for a random network the observed structure is. 
