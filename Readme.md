

## ReIntNTM

Towards Reinterpreting Neural Topic Models via Composite Activations, EMNLP'22

---

### Key Idea

What we consider to be topics (from a neural topic-word distribution) can be combined to form **better** topics via compositions, and hence, a better interpretation of the **same** topic-word distribution.

---
### Neural Topic Model-Agnostic Approach

Steps:

1. **Train** and obtain a topic-word distribution from a Neural Topic Model
2. **Mining Step** to generate a pool of candidate topics (both composite & original)
3. **Solving Step** using following formulations optimizing preferred metric(s)
	1. Greedy using Heuristics 
	2. Multi-Dimensional Knapsack Problem (MDKP)
	3. Maximum-Weight Budget Independent Set (MWBIS)

While the solving step utilises estimated scores using [gensim](https://radimrehurek.com/gensim/), we recommend the final evaluation to be conducted on a large reference corpus such as [Palmetto](https://github.com/dice-group/Palmetto).

---

### Requirements

Python >= 3.6

Your choice of solver (either):

1) using gurobipy directly: [installation instructions](https://www.gurobi.com/documentation/9.5/quickstart_linux/cs_using_pip_to_install_gr.html) (license required)

~~~
python -m pip install gurobipy
~~~
2) solvers via CVXPY [installation instructions](https://www.cvxpy.org/install/index.html)
	1. with gurobipy solver (see 1.)
	2. with [GLPK_MI](https://www.gnu.org/software/glpk/) via [CVXOPT](https://cvxopt.org/) (no-license)
	3. with [SCIP](https://www.scipopt.org), [installation instructions](https://www.cvxpy.org/examples/basic/mixed_integer_quadratic_program.html) (recommended no-license)
~~~
conda install cvxpy cvxopt numpy pyscipopt==3.5.0 -c conda-forge
~~~
More details on using CVXPY found [here](https://www.cvxpy.org/tutorial/advanced/index.html#mixed-integer-programs)

---
### Play data provided
Trained using [OCTIS by MIND-Lab](https://github.com/MIND-Lab/OCTIS/tree/master/octis), CTM [(Bianchi et al. 2021)](https://www.aclweb.org/anthology/2021.eacl-main.143/) model with 20 and 50 topics generating a  pool of 988 and 1414 composite and component topics .

---
### Code
1. algo/cvxpy_based.py - CVXPY-based solutions for MWBIS & MDKP
2. algo/gp_based.py - gurobipy-based solutions for MWBIS & MDKP
3. algo/normal.py - heuristics-based greedy solution & utility functions

Examples were ran on python 3.6, AMD EPYC 7502 @ 2.50GHz, 512GB RAM

---
### Tutorials/Examples
1. gp_example.ipynb
	1. solver examples using gurobipy directly
	2. greedy heuristic examples
	3. topic examples from play data
2. cvxpy_example.ipynb
	* solver examples using CVXPY with various solvers
3. mining_example.ipynb
	* demonstration of the complete pipeline
        
---
### Ethics Statement
We understand that some corpus might produce topics with group of words that might cause offense due to possible sensitiveness regarding politically-charged affairs. This mainly affects the news corpus as they are built on historical events. The use of the reinterpretation process is largely dependent on the corpus that NTM is trained on.

### Citation
If you find our work helpful, we appreciate a citation!
~~~
@inproceedings{lim-lauw-2022-towards,
    title = "Towards Reinterpreting Neural Topic Models via Composite Activations",
    author = "Lim, Jia Peng  and
      Lauw, Hady",
    booktitle = "Proceedings of the 2022 Conference on Empirical Methods in Natural Language Processing",
    month = dec,
    year = "2022",
    address = "Abu Dhabi, United Arab Emirates",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2022.emnlp-main.242",
    pages = "3688--3703",
    abstract = "Most Neural Topic Models (NTM) use a variational auto-encoder framework producing K topics limited to the size of the encoder{'}s output. These topics are interpreted through the selection of the top activated words via the weights or reconstructed vector of the decoder that are directly connected to each neuron. In this paper, we present a model-free two-stage process to reinterpret NTM and derive further insights on the state of the trained model. Firstly, building on the original information from a trained NTM, we generate a pool of potential candidate {``}composite topics{''} by exploiting possible co-occurrences within the original set of topics, which decouples the strict interpretation of topics from the original NTM. This is followed by a combinatorial formulation to select a final set of composite topics, which we evaluate for coherence and diversity on a large external corpus. Lastly, we employ a user study to derive further insights on the reinterpretation process.",
}
~~~
