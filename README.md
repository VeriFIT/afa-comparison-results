# Results for paper "Reasoning about Regular Properties: A Comparative Study"

This repository contains data and scripts for replicating results presented
in the paper "Reasoning about Regular Properties: A Comparative Study"
that was accepted to CADE'23.
 
  * Check our [main site](https://www.fit.vutbr.cz/research/groups/verifit/tools/afa-comparison/) for more information,
    updates and many more.
  * [arXiv preprint](https://arxiv.org/abs/2304.05064): preprint of our paper.
  * For more recent version of our paper, either check the official [CADE'23](https://easyconferences.eu/cade2023/) submission or newer version in the arXiv
  * [Replication package](https://www.fit.vutbr.cz/research/groups/verifit/tools/automata-bench.ova): A virtual machine for replication of our results
    * The replication package is in `.ova` format and needs to be imported in some virtual image manager (see, e.g., [VirtualBox](https://www.virtualbox.org/) or [VMware](https://www.vmware.com/).
    * The replication packages occupies about 10GB of data.</li>
  * [Benchmarks](https://github.com/VeriFIT/automata-bench): source benchmarks that were used in the comparison.
  * [Benchmarks (mirror)](https://pajda.fit.vutbr.cz/ifiedortom/afa-comparison-benchmarks): mirror of source benchmarks that were used in the comparison.
    * Cloning our benchmarks from this mirror requires <code>git-lfs</code> support (see [git-lfs](https://git-lfs.com/))
    * Note that the benchmarks are quite extensive and occupies more than 10GB of storage.

Note that you might need to install <code>git-lfs</code> support (see [git-lfs](https://git-lfs.com/))
and run `git lfs pull` for pulling the files, since they are stored in `git-lfs` format.

## Installation

The replication script is in form of jupyter notebook. First, install `jupyter` and
additional requirements using `pip`. 
We advise to use [venv](https://docs.python.org/3/library/venv.html) package to avoid version 
clashes with your other projects.

    pip3 install jupyter
    pip3 install -r requirements.txt

You might encounter some issues (e.g., missing requirement or clashes in versions,
in that case you can try to fix the version in requirements to those specified in the file).

## Usage

  1. Run the following command in the root directory of the repository:

    jupyter notebook

  * You should see, output similar to following:

        [I 12:21:37.376 NotebookApp] Serving notebooks from local directory: /mnt/e/repositories/github/afa-comparison-results
        [I 12:21:37.377 NotebookApp] Jupyter Notebook 6.5.1 is running at:
        [I 12:21:37.377 NotebookApp] http://localhost:8888/?token=deec5ce73f6ded4323f49e8b933087b7a6f0216fc7615aa9
        [I 12:21:37.377 NotebookApp]  or http://127.0.0.1:8888/?token=deec5ce73f6ded4323f49e8b933087b7a6f0216fc7615aa9
        [I 12:21:37.377 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
        [C 12:21:39.939 NotebookApp]

    The command runs a local server, that host different jupyter notebooks. 
  2. Navigate to the listed address (`localhost` on port `8888` unless specified or configured otherwise), 
     which will list all available jupyter notebooks in the directory. 
  3. Click the `ExperimentalEvaluation.ipynb`. This will open jupyter notebook interpret. 
  4. At the menu, select `Cell` -> `Run All`. This will replicate the results of our experiments.

Feel free to change anything or play with the results. We recommend using `pandas` library. 
You can load our datasets from `csv` files into a `pandas.DataFrame` a tabular representation
that supports basic as well as advanced data manipulation and statistics.

```python
import pandas
df = pandas.read_csv('data/automata_inclusion-timeout-60.csv')
```

## Structure of the repository

  1. `data` contains data measured on Debian GNU/Linux 11, with Intel Core 3.4GHz processor, 
     8 CPU cores, and 20 GB RAM. All experiments were run with the timeout 60s (note that
     increasing the timeout does not have significant effect in our experience). Each dataset is
     in `csv`. We further describe the data in individual section below.
  2. `figs` contains data generated and presented in our paper. In particular:
     * `afa-stats.{tex,html}` and `bre-stats.{tex,html}` contains summary of the results in tabular
        format; it lists runtime average, runtime median and number of timeouts/errors/out of memories. 
     * `cactus.{pdf,png}` is a cactus plot that shows the cumulative time of each tool, i.e. how 
       much time it takes to process the whole benchmark, ordered by time it takes.
     * `models.{pdf,png}` are parametric models for families of formulae identified in benchmarks.
       Each model is in form of "model of runtime based on parameter `k`", where `k` differes
       from each family. See the [Benchmarks](https://github.com/VeriFIT/automata-bench) repository
       or the paper for more information about these families.
     * `scatter.{pdf,png}` are scatter plots of winners for each benchmarks. We identified three 
       best performing tools for each benchmark and plotted their runtimes against each other.
  3. `db.pickle` contains various statistics we collected about formulae and benchmarks. 
  4. `ExperimentalEvaluation.ipynb` contains jupyter notebook, that replicates results presented
     in our paper.

## Description of Data

The data are in `csv` delimited by `;`.

Each column corresponds to metrics collected for each tools. In particular, we list the following
metrics and results:
  1. `*-result` lists either `MISSING` (there is no benchmark for this tool), `TO` (Timeout), `ERR` 
    (Error, due to out of memory, or other issues),
    `EMPTY`/`NOT_EMPTY` (depending on the problem the benchmark was solving) 
  3. `*-runtime` lists either `TO`, `ERR` or time in seconds.
  4. `*-mintermization` list for some tools how long took the mintermization of source automaton.

Note, that the tools do not correspond to those presented in the paper.
The mapping of tools measured in results to those presented in paper are as follows:

* `abc`: `bwIC3`,  is our own implementation of backward reduction based on the model checker ABC.
* `afaminisat`: `Antisat`, is our own implementation of antichain AFA emptiness test integrated with SAT solver.
* `automata`: `Automata`, is a C# library implementing symbolic NFA/DFA; available at [Automata](https://github.com/AutomataDotNet/Automata).
* `bisim`: `Bisim`, is an Java implementation of the AFA-emptiness check based on bisimulation.
* `bricks`: `Brics`, is a baseline automata library primarly focusing on DFA; available at [dk.brics.automaton](https://www.brics.dk/automaton/).</li>
* `cvc5`: `CVC5`, is an automatic theorem prover for SMT; available at [cvc5](https://cvc5.github.io/)
* `jaltimpact`: `JaltImpact`, is an implementation of interpolation-based algorithm; available at: [JAltImpact](https://github.com/cathiec/JAltImpact).
* `mata-nfa`: `eNfa`, is our own implementation of NFA.
* `mona`: `Mona`, is an optimized implementation of DFA; available at [The Mona project](https://www.brics.dk/mona/).
* `vata`: `VATA`, is a library implementing NFA; available at [libvata](https://github.com/ondrik/libvata).
* `z3`: `Z3`,  is an automatic theorem prover for SMT developed by MicroSoft; available at [Z3](https://github.com/Z3Prover/z3).

## Contributing

Our repositories are open for forking, enabling interested individuals to explore,
modify, and build upon our codebase.

If you are interested in contributing your own results, or polish the presentation of our current results 
we suggest following: 
  1. Fork this repository. 
  2. Create a Pull Request from your fork. We will check your contribution, optionally request
     changes and if we are satisfied, we will merge the changes into our repository.

For other ways of contributing refer to our [main site](https://www.fit.vutbr.cz/research/groups/verifit/tools/afa-comparison/#contributing)

## License

The software is free under the CC BY NC SA licence (https://creativecommons.org/licenses/by-nc-sa/3.0/)

## Troubleshooting

If you encounter any problems, either create a [new issue](https://github.com/VeriFIT/afa-comparison-results/issues/new/choose), 
or contact us. We will try to assist you.

The issues might come from different version of Python (the notebook should work with `Python 3.8`
and `Python 3.9`; newer versions might introduced some incompabilities in dependencies). Try to
fix the version of depedencies to recommended versions. The code was not tested on Python 2.

## Change log

  * **1.6.2023**: Initial proper version

## Contact Us

* Tomas Fiedor &lt;[ifiedortom@fit.vutbr.cz](mailto:ifiedortom@fit.vutbr.cz)&gt;
* **Lukas Holik** &lt;[holik@fit.vutbr.cz](mailto:holik@fit.vutbr.cz)&gt;
* Martin Hruska &lt;[ihruska@fit.vutbr.cz](mailto:ihruska@fit.vutbr.cz)&gt;
* Adam Rogalewicz &lt;[rogalew@fit.vutbr.cz](mailto:rogalew@fit.vutbr.cz)&gt;
* Juraj Sic &lt;[sicjuraj@fit.vutbr.cz](mailto:sicjuraj@fit.vutbr.cz)&gt;
* Pavol Vargovcik &lt;[ivargovcik@fit.vutbr.cz](mailto:ivargovcik@fit.vutbr.cz)&gt;

## Acknowledgements

This work has been supported by the Czech Ministry of Education, Youth and Sports
ERC.CZ project LL1908, and the FIT BUT internal project FIT-S-20-6427.
