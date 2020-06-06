# GCB
Stand-alone version of the GCB-service

## Installation manual

### Install dependencies

`sudo apt-get install graphviz graphviz-dev python3-graphviz python3-pygraphviz`

`pip3 install -r requirements.txt`

`git clone https://github.com/paraslonic/orthosnake`

`git clone https://github.com/DNKonanov/geneGraph`

## Usage

### Start server

To start GCB server on your computer type in GCB_package folder this:

`python3 gcb_server.py`

So, you can open **127.0.0.1:8000** or **localhost:8000** adress in your web-browser and use GCB
There is no pre-computed datasets in the github-version of the GCB_package.

### Add data

[GeneGraph](https://github.com/DNKonanov/geneGraph) is a command-line tool was developed to provide graphs and complexity profiles
generation.

Suppose that we have a number of `*.fna` files for 100 different genomes of Mycoplasma. The first step is the annottion and the inference of orthology groups by [this Snakemake script](https://github.com/paraslonic/orthosnake). The desired file is `OrthoGroups.txt`.

Next, we shuld use `gg.py` script from geneGraph:

`python gg.py -i [path to OrthoGroups.txt] -o Mycoplasma`

What this command does:
* parses `OrthoGroups.txt`
* creates graph file in sif-format and additional information about genes, their contextes, etc.
* creates SQLite database used in GCB
* computes complexity profiles for ALL genomes in the dataset and fills the DB
* dumps graph object for fast access
* all operations are executed by two ways: with deletion of paralogues, and with artificial orthologization

Final result is a folder with name `Mycoplasma`. This folder contains:
* `params.txt` - all selected parameters
* `Mycoplasma.sif` - file with all edges in the graph and some additional information
* `Mycoplasma.db` - SQLite database used in GCB
* `Mycoplasma.dump` - dumped graph object
* `Mycoplasma_genes.sif` - information about all genes in the graph in txt format
* `Mycoplasma_context.txt` - number of different gene contexts for each node in the graph
* all these files with `_pars` suffix contain the same information for graph with orthologized paralogues
* a number of folders. These folders contain complexity profiles for each genome. Structure of folder is described [here](https://github.com/DNKonanov/geneGraph)

Now we should move this `Mycoplasma` folder to `GCB/data`. If there is no `data` folder in `GCB` root folder, just create it with `mkdir data`. It's all.

Reload `gcb_server.py`, open Left Panel, click to Organism selector and select you new dataset.

## Links

Web-service [Genome Complexity Browser](http://gcb.rcpcm.org)

Python module [gene-graph-lib](https://github.com/DNKonanov/gene_graph_lib)

Command-line tool [geneGraph](https://github.com/DNKonanov/geneGraph)


## References

Wll be added

