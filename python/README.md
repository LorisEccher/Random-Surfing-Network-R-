# Random Surfer: From Network Structure to Visit Frequencies

This Python notebook explores how PageRank turns a directed link graph into a ranking of web pages. Four six-page examples isolate the effects of dangling pages, closed groups and periodicity. The final experiment uses Harvard500, a historical crawl of 500 pages starting at Harvard University's homepage.

## Run the notebook

Run these commands from the project directory. Download the NCM toolbox from [MathWorks](https://www.mathworks.com/moler/chapters.html), extract its `harvard500.mat`, and place it beside `Network Surfing.ipynb`. In JupyterLab, select the Python 3 kernel and run all cells in order.

```sh
python -m pip install -r requirements.txt
python -m jupyterlab
```

The notebook records Python 3.14.10 in its kernel metadata. The Harvard500 simulation uses one million visits and random seed 123.

## Method

1. Remove self-links and build a column-oriented transition matrix. Entry `P[i, j]` is the probability of moving from page `j` to page `i`. Divide each nonempty column by its outgoing link count.
2. For corrected models, replace empty columns with a uniform distribution. To add teleportation, use the corrected link transitions with probability 0.85 and choose uniformly among all pages with probability 0.15.
3. Examine eigenvalues and normalize the right eigenvector associated with eigenvalue 1 when that eigenvalue is simple. Search for a strictly positive matrix power to check primitivity.
4. Iterate the transition matrix on an initial probability vector and compare the resulting distributions with equilibrium. For Harvard500, measure the L1 distance to each model's own stationary distribution.
5. Simulate a random surfer and compare empirical visit frequencies with the stationary probabilities.

## Results

In the saved Harvard500 experiment, 124 pages have no outgoing links after self-links are removed. From a uniform start, the dangling-corrected model reaches an L1 distance below 1e-8 after 198 updates. With teleportation, the corresponding count is 62. The Harvard homepage ranks first with a stationary probability of about 8.43%.

## Sources and data

- Page, L., Brin, S., Motwani, R., and Winograd, T. (1999). *The PageRank Citation Ranking: Bringing Order to the Web*. Stanford InfoLab Technical Report 1999-66. [Original report, hosted by USC](https://bytes.usc.edu/files/cs103/pagerank/pagerank.pdf)
- Moler, C. B. (2004). *Numerical Computing with MATLAB*, Chapter 2, “Linear Equations,” Sections 2.11–2.12, especially p. 31. Describes the August 2003 crawl and reports the Harvard500 rankings. [MathWorks chapter](https://www.mathworks.com/content/dam/mathworks/mathworks-dot-com/moler/lu.pdf)
- SuiteSparse Matrix Collection. *MathWorks/Harvard500*, matrix ID 1172. [Dataset catalog record](https://sparse.tamu.edu/MathWorks/Harvard500)

## Contributors and licensing

- Lead author: Loris Eccher
- Contributor: Alfio Perotti

The proposed MIT license covers the original Python project material. Harvard500 is available separately from MathWorks under its own terms.

See [the draft MIT license](LICENSE.draft.txt).

The original MATLAB routine started from Harvard University's home page and generated a 500-page graph for a numerical demonstration of PageRank. The file contains a sparse hyperlink matrix (`G`) and its corresponding URL list (`U`). Visits in the notebook are simulated. Moler's account dates the worked example to August 2003, while the SuiteSparse catalog lists 2002. The attached MATLAB file has 2,636 stored links before self-link removal. [MathWorks NCM toolbox](https://www.mathworks.com/moler/chapters.html)
