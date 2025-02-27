## graphKKE: graph Kernel Koopman Embedding. 

Code for the paper Melnyk K., Klus S., Montavon G., Conrad T. "GraphKKE: Graph Kernel Koopman Embedding for Human Microbiome Analysis", https://arxiv.org/abs/2008.05903. 


## Repository Structure
* **graphkke/algorithm**: consists of the main function for the grapphKKE method;
* **graphkke/graph**: consists of the class Graph and of C++ implemenatation of WL kernel with multithreading. The Python interface is created from a C++ source code that is wrapped with SWIG (http://www.swig.org);
* **graphkke/generate_graphs**: consists of functions and classes for generating the benchmark graphs with metastable behavior;
* **examples**: consists of experiments with the benchmark data and real-world datasets, and also a script for generating benchmark data;
* **data**: contains OTU tables and constructed adjacency matrices. Microbiome data is coming from [1] and [2].


## Install
* The package uses setuptools, which is a common way of installing python modules. To install: 
  - To install in your home directory, use:
    ```
    $ git clone https://github.com/k-melnyk/graphKKE.git
    $ cd graphKKE
    
    $ python setup.py build
    $ python setup.py install
    
    NOTE:
    In the Anaconda package, in the file "anaconda3\Lib\site-packages\graphkke-0.1.1-py3.8-win-amd64.egg\graphkke\graph\GraphLib_c.py" do the following:
    #if __package__ or "." in __name__:
    #from . import _GraphLib_c
    #else:
    import _GraphLib_c
    ```
## Examples
* You need first convert your array of adjacency matrices ```adj_matrix``` or adjacency lists ```adj_list``` into graph data type, where ```node_labels``` is a list of node labels for each time-point.
    ```python
    from graphkke.algorithm import m_graphkke
    import graphkke.algorithm.kernels as kernels
    import graphkke.graph.graph as gl
    
    graphs = []
    
    for ind, graph in enumerate(adj_matrix):
        graph = gl.Graph(graph)
        graph.add_node_labels(node_labels[ind])

        graphs.append(graph)
    ```
  Or adjacency lists:
    ```python
    graphs = []
    
    for ind, graph in enumerate(adj_list):
        graph = gl.Graph(graph, num_nodes)
        graph.add_node_labels(node_labels[ind])

        graphs.append(graph)
    ```
* For running graphKKE:
  ```python
  # Hyperparameters of graphKKE
  epsilon = 0.5
  operator = 'K'
  num_iterations = 5
  k = kernels.WlKernel(num_iterations)

  # Run graphKKE method
  m_graphkke.graphkke(graphs, k, tau=1, epsilon=epsilon, outdir=outdir)
  ```

* In order to run WL kernel: 
  ```python
  import graphkke.graph.graph as gl
  
  kernel = gl.wl_subtree_kernel(graph, n_iterations)
  ```

## References
   [0]  Shervashidze  N.  et  al.  "Weisfeiler-Lehman  GraphKernels". In: Journal of Machine Learning Research 12 (2011), pp. 2539–2561.
   ```
    @article{WL,
      title={Weisfeiler-Lehman Graph Kernels.},
      author={Shervashidze N. and Schweitzer P. and Jan van Leeuwen E. and Mehlhorn K. and Borgwardt K.},
      journal={Journal of Machine Learning Research},
      year={2011},
      volume={12},
      pages={2539-2561}
    }
   ```
   [1] Hsiao  A.  et  al.  "Members  of  the  human  gut  mi-crobiota involved in recovery from Vibrio choleraeinfection". In:Nature 515 (Jan. 2014), pp. 423–426.
   ```
   @article{CholeraInfOriginal,
    author = {Hsiao A. and Shamsir Ahmed AM. and Subramanian S. and Griffin NW. and Drewry LL. and Petri WA. and Haque R. and Ahmed T. and Gordon JI.},
    year = {2014},
    month = {01},
    title = {Members of the human gut microbiota involved in recovery from Vibrio cholerae infection},
    volume = {515},
    pages={423-426},
    journal = {Nature},
    doi = {10.1038/nature13738}
    }
   ```
   [2]  Caporaso J. et al. "Moving pictures of the humanmicrobiome". In: Genome biology 12. May 2011, R50.
   ```
   @article{MovingPicture,
    author = {Caporaso J. and Lauber C. and Costello E. and Berg-Lyons D. and González A. and Stombaugh J. and Knights D and Gajer P. and Ravel J. and Fierer N. and Gordon J. and Knight R.},
    year = {2011},
    month = {05},
    pages = {R50},
    title = {Moving pictures of the human microbiome},
    volume = {12},
    journal = {Genome biology},
    doi = {10.1186/gb-2011-12-5-r50}
    }
   ```
  [3] Klus S. et al. "A kernel-based approach to molecular conformation analysis". In: Journal of Chemistry Physics 149. 2018.
  ```
  @article{KlusMolecularAnalysis,
  title={A kernel-based approach to molecular conformation analysis},
  author={Klus S. and Bittracher A. and Schuster I. and Schütte C.},
  journal={Journal of Chemical Physics},
  year={2018},
  volume={149},
  doi={10.1063/1.5063533}
}
```
