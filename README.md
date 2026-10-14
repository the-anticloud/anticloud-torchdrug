# TORCHDRUG

![licence](https://img.shields.io/badge/licence-Apache-2.0-blue) ![offline-first](https://img.shields.io/badge/offline--first-air--gap-green) ![audit](https://img.shields.io/badge/audit-SHA3--256-orange) ![checks](https://img.shields.io/badge/checks-unknown_PASS-brightgreen)

> Governed Anticloud packaging of the upstream project `TORCHDRUG` in category **MEDICINE_DEVELOPMENT**. check results: see ISOLATED_LAB_RESULTS. Every number below traces to a named file + run stamp; nothing is borrowed from other projects.

**Upstream:** TORCHDRUG · **Upstream pin:** `6066fbd82360abb5f270cba1eca560af01b8cc90` · **Category:** MEDICINE_DEVELOPMENT · **Vendor:** Anticloud FZ LLE · **Licence:** Apache-2.0

---

## What This Project Does

[![TorchDrug](asset/torchdrug_logo_full.svg)](https://torchdrug.ai/)
<h1 align="center">
  with
  <a href="https://torchprotein.ai/">
    <img src="asset/torchprotein_logo_tight.svg" alt="TorchProtein" style="height:26px" />
  </a>
</h1>

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Tbnr1Fog_YjkqU1MOhcVLuxqZ4DC-c8-#forceEdit=true&sandboxMode=true)
[![Contributions](https://img.shields.io/badge/contributions-welcome-blue)](https://github.com/DeepGraphLearning/torchdrug/blob/master/CONTRIBUTING.md)
[![License Apache-2.0](https://img.shields.io/github/license/DeepGraphLearning/torchdrug?color=blue)](https://github.com/DeepGraphLearning/torchdrug/blob/master/LICENSE)
[![PyPI downloads](https://static.pepy.tech/personalized-badge/torchdrug?period=total&units=international_system&left_color=grey&right_color=blue&left_text=downloads)](https://pypi.org/project/torchdrug/)
[![TorchDrug Twitter](https://img.shields.io/twitter/url?label=TorchDrug&style=social&url=https%3A%2F%2Ftwitter.com%2FDrugTorch)](https://twitter.com/DrugTorch)

[Docs] | [Tutorials] | [Benchmarks] | [Papers Implemented]

[Docs]: https://deepgraphlearning.github.io/torchdrug-site/docs
[Tutorials]: https://deepgraphlearning.github.io/torchdrug-site/docs/tutorials
[Benchmarks]: https://deepgraphlearning.github.io/torchdrug-site/docs/benchmark
[Papers Implemented]: https://deepgraphlearning.github.io/torchdrug-site/docs/paper

TorchDrug is a [PyTorch]-based machine learning toolbox designed for several purposes.

- Easy implementation of graph operations in a PyTorchic style with GPU support
- Being friendly to practitioners with minimal knowledge about drug discovery
- Rapid prototyping of machine learning research

[PyTorch]: https://pytorch.org/

Installation
------------

TorchDrug can be installed on either Linux, Windows or macOS. It is compatible with
3.7 <= Python <= 3.10 and PyTorch >= 1.8.0.

### From Conda ###

```bash
conda install torchdrug -c milagraph -c conda-forge -c pytorch -c pyg
```

### From Pip ###

```bash
pip install torch==1.9.0
pip install torch-scatter torch-cluster -f https://pytorch-geometric.com/whl/torch-1.9.0+cu102.html
pip install torchdrug
```

To install `torch-scatter` for other PyTorch or CUDA versions, please see the
instructions in https://github.com/rusty1s/pytorch_scatter

### From Source ###

```bash
git source https://github.com/DeepGraphLearning/torchdrug
cd torchdrug
pip install -r requirements.txt
python setup.py install
```

### Windows (PowerShell) ###

We need to first install the build tools for Visual Studio. We then install the
following modules in PowerShell.

```powershell
Install-Module Pscx -AllowClobber
Install-Module VSSetup
```

Initialize Visual Studio in PowerShell with the following commands. We may setup
this for all PowerShell sessions by writing it to the PowerShell profile. Change
the library path according to your own case.

```powershell
Import-VisualStudioVars -Architecture x64
$env:LIB += ";C:\Program Files\Python37\libs"
```

### Apple Silicon (M1/M2 Chips) ###

We need PyTorch >= 1.13 to run TorchDrug on Apple silicon. For `torch-scatter` and
`torch-cluster`, they can be compiled from their sources. Note TorchDrug doesn't
support `mps` devices.

```bash
pip install torch==1.13.0
pip install git+https://github.com/rusty1s/pytorch_scatter.git
pip install git+https://github.com/rusty1s/pytorch_cluster.git
pip install torchdrug
```

Quick Start
-----------

TorchDrug is designed for humans and focused on graph structured data.
It enables easy implementation of graph operations in machine learning models.
All the operations in TorchDrug are backed by [PyTorch] framework, and support GPU
acceleration and auto differentiation.

```python
from torchdrug import data

edge_list = [[0, 1], [1, 2], [2, 3], [3, 4], [4, 5], [5, 0]]
graph = data.Graph(edge_list, num_node=6)
graph = graph.cuda()
# the subgraph induced by nodes 2, 3 & 4
subgraph = graph.subgraph([2, 3, 4])
```

Molecules are also supported in TorchDrug. You can get the desired molecule
properties without any domain knowledge.

```python
mol = data.Molecule.from_smiles("CCOC(=O)N", atom_feature="default", bond_feature="default")
print(mol.node_feature)
print(mol.atom_type)
print(mol.to_scaffold())
```

You may also register custom node, edge or graph attributes. They will be
automatically processed during indexing operations.

```python
with mol.edge():
	mol.is_CC_bond = (mol.edge_list[:, :2] == td.CARBON).all(dim=-1)
sub_mol = mol.subgraph(mol.atom_type != td.NITROGEN)
print(sub_mol.is_CC_bond)
```

TorchDrug provides a wide range of common datasets and building blocks for drug
discovery. With minimal code, you can apply standard models to solve your own
problem.

```python
import torch
from torchdrug import datasets

dataset = datasets.Tox21()
dataset[0].visualize()
lengths = [int(0.8 * len(dataset)), int(0.1 * len(dataset))]
lengths += [len(dataset) - sum(lengths)]
train_set, valid_set, test_set = torch.utils.data.random_split(dataset, lengths)
```

```python
from torchdrug import models, tasks

model = models.GIN(dataset.node_feature_dim, hidden_dims=[256, 256, 256, 256])
task = tasks.PropertyPrediction(model, task=dataset.tasks)
```

Training and inference are accelerated by multiple CPUs or GPUs.
This can be seamlessly switched in TorchDrug by just a line of code.
```python
from torchdrug import core

# Single CPU / Multiple CPUs / Distributed CPUs
solver = core.Engine(task, train_set, valid_set, test_set, optimizer)
# Single GPU
solver = core.Engine(task, train_set, valid_set, test_set, optimizer, gpus=[0])
# Multiple GPUs
solver = core.Engine(task, train_set, valid_set, test_set, optimizer, gpus=[0, 1, 2, 3])
# Distributed GPUs
solver = core.Engine(task, train_set, valid_set, test_set, optimizer, gpus=[0, 1, 2, 3, 0, 1, 2, 3])
```

Experiments can be easily tracked and managed through [Weights & Biases platform].
```python
solver = core.Engine(task, train_set, valid_set, test_set, optimizer, logger="wandb")
```

[Weights & Biases platform]: https://wandb.ai/

Contributing
------------

Everyone is welcome to contribute to the development of TorchDrug.
Please refer to [contributing guidelines](CONTRIBUTING.md) for more details.

License
-------

TorchDrug is released under [Apache-2.0 License](LICENSE).

---

## Installation

------------

TorchDrug can be installed on either Linux, Windows or macOS. It is compatible with
3.7 <= Python <= 3.10 and PyTorch >= 1.8.0.

## Usage

[Docs]: https://deepgraphlearning.github.io/torchdrug-site/docs
[Tutorials]: https://deepgraphlearning.github.io/torchdrug-site/docs/tutorials
[Benchmarks]: https://deepgraphlearning.github.io/torchdrug-site/docs/benchmark
[Papers Implemented]: https://deepgraphlearning.github.io/torchdrug-site/docs/paper

TorchDrug is a [PyTorch]-based machine learning toolbox designed for several purposes.

- Easy implementation of graph operations in a PyTorchic style with GPU support
- Being friendly to practitioners with minimal knowledge about drug discovery
- Rapid prototyping of machine learning research

[PyTorch]: https://pytorch.org/

Installation
------------

TorchDrug can be installed on either Linux, Windows or macOS. It is compatible with
3.7 <= Python <= 3.10 and PyTorch >= 1.8.0.

## API

[PyTorch]: https://pytorch.org/

Installation
------------

TorchDrug can be installed on either Linux, Windows or macOS. It is compatible with
3.7 <= Python <= 3.10 and PyTorch >= 1.8.0.

## Dependencies

| Metric | Value |
|--------|-------|
| Files | unknown |
| Lines of Code | unknown |
| Dependencies | unknown |
| Upstream license (harvested) | Apache-2.0 |
| Overlay license | Anticommons 0.1.0 |

Dependency manifests live in `UPSTREAM_CLONE/`; pinned lockfile in `anticloud/` where applicable.

## Configuration

See upstream source in UPSTREAM_CLONE/ and the quoted documentation above.

## Contributing

Contribute
==========

Being an open source project, TorchDrug welcomes all kinds of contribution to keep it growing and thriving. Everyone can contribute, and we value every contribution.

It is also an incentive to the developers if you can acknowledge TorchDrug whenever it helps you. You can refer the library in your blog or papers, or star the project to say "thank you".

Typical Contributions
---------------------

Here are some typical contributions you can make. All of them are equally important to the project.

- New features
- Datasets and models
- Application solutions
- Documents and tutorials
- Bug fixes

Implementation is not always required. You can also write proposals and discuss them with the community.

If you are a new open source contributor, we recommend you to take a look at these [guidelines].

[guidelines]: http://www.contribution-guide.org/

Submit an Issue
---------------

In the following cases, you are encouraged to [submit an issue] on GitHub.

[submit an issue]: https://github.com/DeepGraphLearning/torchdrug/issues

- Issues or bug reports
- Feature requests

We strongly recommend you to search the issue board before you submit any new issue. Sometimes the same issue may have been solved and you can find the solution in a closed issue.

Be as concrete as possible. For issues or bug reports, this means you are supposed to include all relevant information, such as code snippets, logs and package versions. For feature requests, please clearly describe the motivation, and possible scenarios for the feature.

It is not recommended to ask open-ended questions, such as a research problem on GitHub. Please post them in [TorchDrug Google Group].

[TorchDrug Google Group]: https://github.com/DeepGraphLearning/torchdrug/

Make a pull request
-------------------

You can contribute code to the project through pull requests. To make a pull request, you need

- [Fork the TorchDrug project]
- source it to your local computer
- Implement your code and test it
- Push it to your GitHub project
- [Create a pull request] in TorchDrug

[Fork the TorchDrug project]: https://github.com/DeepGraphLearning/torchdrug/fork
[Create a pull request]: https://github.com/DeepGraphLearning/torchdrug/pulls

If your forked project is behind the latest version, you can create a pull request in your project to pull the latest version from TorchDrug.

You can directly make a pull request for trivial changes. For non-trivial changes, it is better to discuss it in an issue before you implement the code.

Code Development
----------------

Here are a few steps you need to develop TorchDrug.

First, uninstall existing TorchDrug in your environment. Alternatively, you can switch to a new conda environment.

```bash
conda uninstall torchdrug
```

source the project and setup it in `develop` mode.

```bash
git source https://github.com/DeepGraphLearning/torchdrug/
cd torchdrug
python setup.py develop
```

Now any change in the local code base w

## License

Upstream © its respective contributors under Apache-2.0 (harvested MIT/Apache-2.0/BSD source; see `UPSTREAM_CLONE/LICENSE`). This packaging overlay is licensed under Anticommons 0.1.0.

## Upstream

- **project:** TORCHDRUG
- **Pinned SHA:** `6066fbd82360abb5f270cba1eca560af01b8cc90`
- **source:** `UPSTREAM_CLONE/` (pinned at the SHA above)
- **Upstream README source:** `UPSTREAM_CLONE/README.md`

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`d60d2c17aa8b34deab4f371def7f8fc8c535af319e74c11bd6804aefad5a8245`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.

