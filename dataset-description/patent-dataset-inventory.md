---
title: Patent Dataset Inventory
description: Patent workspace 的集中数据集清单、来源、适用任务和缺口。
---

# Patent Dataset Inventory

最后核验日期：2026-07-01

集中数据根目录：`/home/raptorx-2/project/patents/data`

本文总结 patent workspace 目前已经集中管理的数据集。真实数据 payload 不存放在本 handbook repo 中。各 patent repo 通过本地 `data` symlink 指向上面的集中数据根目录。

## 当前状态

集中数据根目录目前约 12G，共 53 个文件。按逻辑数据集去重后，目前有 11 个数据集：

- `Elliptic Bitcoin`
- `DGraph-Fin`
- `T-Finance`
- `FraudAmazon`
- `YelpChi` / `FraudYelp`
- `Reddit`
- `Wikipedia`
- `Questions`
- `Tolokers`
- `IBM AML`
- `synthetic AML-like`

如果把 repo 专属副本和 cache group 也算进去，目前有 15 个非空 payload group。其中 `Elliptic`、`FraudAmazon`、`synthetic AML-like` 存在面向特定 patent repo 的重复本地副本。

文件类型统计：

| Extension | Count |
| --- | ---: |
| `.pt` | 17 |
| `.csv` | 12 |
| `.npy` | 6 |
| `.zip` | 4 |
| `.mat` | 4 |
| `.json` | 4 |
| `.npz` | 3 |
| `.bin` | 2 |
| `.md` | 1 |

## 数据集清单

| Dataset | 本地路径 | 数据类型 | 网站源 | 下载源 | 数据描述 | 可用于 task |
| --- | --- | --- | --- | --- | --- | --- |
| Elliptic Bitcoin | `/home/raptorx-2/project/patents/data/gadbench/pyg/elliptic`，另有 P21 副本 | Bitcoin transaction graph，CSV 和 PyG `.pt` | [PyG Elliptic docs](https://pytorch-geometric.readthedocs.io/en/2.5.2/generated/torch_geometric.datasets.EllipticBitcoinDataset.html)，[Kaggle original](https://www.kaggle.com/datasets/ellipticco/elliptic-data-set) | PyG 使用 `https://data.pyg.org/datasets/elliptic/*.zip` | 203,769 笔交易，234,355 条 directed payment edges，标签包括 licit、illicit、unknown，共 49 个 timestep。 | AML fraud detection、temporal split、node classification、point-in-time leakage test、GNN benchmark。 |
| DGraph-Fin | `/home/raptorx-2/project/patents/data/gadbench/DGraphFin` | 大规模 directed financial graph，`.npz` 和 timestamp `.npy` | [DGraph-Fin site](https://dgraph.xinye.com/dataset)，[PyG DGraphFin docs](https://pytorch-geometric.readthedocs.io/en/2.7.0/generated/torch_geometric.datasets.DGraphFin.html) | 从 DGraph-Fin 网站手动下载 | 3,700,550 个 node，4,300,999 条 edge，17 维 node feature，包含 edge type、edge timestamp、fraud label 和 background class。 | large-scale graph anomaly detection、fraud user classification、temporal edge filtering、scalability test。 |
| T-Finance | `/home/raptorx-2/project/patents/data/gadbench/T-Finance` | Financial graph，PyTorch `.pt` | [Rethinking-Anomaly-Detection](https://github.com/squareRoot3/Rethinking-Anomaly-Detection) | 源 repo 中的 Google Drive link | 金融交易图数据，包含 node feature 和 anomaly label，已打包用于 graph anomaly detection benchmark。 | supervised graph anomaly detection、static financial fraud benchmark、GNN baseline comparison。 |
| FraudAmazon | `/home/raptorx-2/project/patents/data/gadbench/Amazon`、`_dglcache`，另有两个 P20 副本 | Multi-relational review fraud graph，`.pt`、`.mat`、DGL `.bin` | [DGL FraudAmazonDataset](https://www.dgl.ai/dgl_docs/generated/dgl.data.FraudAmazonDataset.html)，[GADBench](https://github.com/squareRoot3/GADBench) | `https://data.dgl.ai/dataset/FraudAmazon.zip` | Amazon Musical Instruments review user graph，包含 handcrafted features、fraud labels 和多种 review relation。 | fraudulent user detection、heterophily test、relation-aware GNN、patent-20 risk-ranked shard 实验。 |
| YelpChi / FraudYelp | `/home/raptorx-2/project/patents/data/gadbench/YelpChi`、`_dglcache` | Multi-relational review spam graph，`.pt`、`.mat`、DGL `.bin` | [DGL FraudYelpDataset](https://www.dgl.ai/dgl_docs/generated/dgl.data.FraudYelpDataset.html)，[GADBench](https://github.com/squareRoot3/GADBench) | `https://data.dgl.ai/dataset/FraudYelp.zip` | Yelp review spam graph，包含 review features、spam labels 和多种 review relation。 | spam review detection、graph fraud detection、relation-aware GNN benchmark。 |
| Reddit | `/home/raptorx-2/project/patents/data/gadbench/Reddit` | Temporal interaction CSV 和 processed `.pt` | [JODIE SNAP page](https://snap.stanford.edu/jodie/)，[JODIE GitHub](https://github.com/claws-lab/jodie) | [reddit.csv](https://snap.stanford.edu/jodie/reddit.csv) | 672,447 条 temporal user-item interaction，字段包含 timestamp、state label 和 feature columns。 | temporal interaction modeling、dynamic embedding、state-change prediction、anomaly detection。 |
| Wikipedia | `/home/raptorx-2/project/patents/data/gadbench/Wikipedia` | Temporal interaction CSV 和 processed `.pt` | [JODIE SNAP page](https://snap.stanford.edu/jodie/)，[JODIE GitHub](https://github.com/claws-lab/jodie) | [wikipedia.csv](https://snap.stanford.edu/jodie/wikipedia.csv) | 157,474 条 temporal user-item interaction，schema 与 Reddit 相同。 | temporal link prediction、dynamic node classification、anomaly 和 state-change detection。 |
| Questions | `/home/raptorx-2/project/patents/data/gadbench/pyg/questions` | Heterophilous graph，`.npz` 和 PyG `.pt` | [Yandex heterophilous-graphs](https://github.com/yandex-research/heterophilous-graphs)，[PyG source](https://pytorch-geometric.readthedocs.io/en/latest/_modules/torch_geometric/datasets/heterophilous_graph_dataset.html) | `https://github.com/yandex-research/heterophilous-graphs/raw/main/data/questions.npz` | 48,921 个 node，153,540 条 edge，301 维 node feature，binary label。 | heterophily GNN evaluation、node classification、architecture stress test。 |
| Tolokers | `/home/raptorx-2/project/patents/data/gadbench/pyg/tolokers` | Heterophilous graph，`.npz` 和 PyG `.pt` | [Yandex heterophilous-graphs](https://github.com/yandex-research/heterophilous-graphs)，[PyG source](https://pytorch-geometric.readthedocs.io/en/latest/_modules/torch_geometric/datasets/heterophilous_graph_dataset.html) | `https://github.com/yandex-research/heterophilous-graphs/raw/main/data/tolokers.npz` | 11,758 个 node，519,000 条 edge，10 维 node feature，binary label。 | heterophily benchmark、node classification、robustness check。 |
| IBM AML | `/home/raptorx-2/project/patents/data/patent-4-graphrnn-p4-minimal-repro/ibm_aml` | Tabular AML transaction CSV 和 embedding `.npy` cache | [IBM/AML-Data](https://github.com/IBM/AML-Data)，[Kaggle dataset](https://www.kaggle.com/datasets/ealtman2019/ibm-transactions-for-anti-money-laundering-aml) | Kaggle slug `ealtman2019/ibm-transactions-for-anti-money-laundering-aml`，original [IBM Box](https://ibm.box.com/v/AML-Anti-Money-Laundering-Data) | Synthetic financial transaction data。本地 raw files 包括 `HI-Small_Trans.csv`，5,078,345 行，以及 `HI-Medium_Trans.csv`，31,898,238 行。 | AML transaction classification、account-transfer graph construction、tabular baseline、embedding 和 LLM feature generation。 |
| synthetic AML-like | `/home/raptorx-2/project/patents/data/patent-4-graphrnn`，另有 P4 minimal 副本 | 小型本地 CSV | 本地生成数据 | local only | 720 行 account-to-account toy transaction，包含 latent risk 和 transaction label。 | smoke test、unit test、integration test、快速 reproducibility check。 |

## 已知缺口

集中数据根目录覆盖的是迁移时本机实际存在且可恢复的数据。它还没有覆盖每个 patent repo 代码里所有 optional dataset reference。

| Repo | 当前状态 | 缺口 |
| --- | --- | --- |
| `submitted/patent-17-llm-feature-gen` | `data` symlink 有效，但目标目录为空。 | 脚本预期 `data/ibm_aml` 和 `data/home_credit`。IBM AML 已存在于 P4 minimal data folder，但尚未链接到这里。Home Credit 本地不存在。 |
| `submitted/patent-18-ring` | `data` symlink 有效，但目标目录为空。 | 脚本预期 `data/Elliptic`。Elliptic 已存在于 `gadbench/pyg/elliptic`，但尚未链接到这个 repo。 |
| `submitted/patent-19-adaptive-ppr` | `data` 目前是 broken symlink，指向 `/home/raptorx-2/project/patents/data/patent-19-adaptive-ppr`。 | 脚本预期 `Planetoid`、`WebKB`、`WikiCS`、`Actor`、`HeterophilousGraphDataset`、`Flickr`、`OGB` 等 cache。这些本地还没有。 |
| `submitted/patent-4-graphrnn` | repo-specific central folder 只有 `synthetic_aml_like.csv`。 | 真实 IBM AML 数据在 `patent-4-graphrnn-p4-minimal-repro`，但尚未链接到此 repo。 |

## 建议后续动作

1. 对共享数据加 symlink，不再创建新的物理副本：
   - `patent-17-llm-feature-gen/ibm_aml` 指向 P4 minimal 的 IBM AML folder。
   - `patent-18-ring/Elliptic` 指向现有 Elliptic folder。
   - `patent-4-graphrnn/ibm_aml` 指向 P4 minimal 的 IBM AML folder。
2. 创建缺失的 `patent-19-adaptive-ppr` central data directory。
3. 只下载或重新生成真正缺失的数据：
   - Patent 17 所需的 `home_credit`。
   - Patent 19 所需的 PyG 和 OGB benchmark cache。
4. 所有 repo script 都指向共享 symlink 后，再考虑删除重复的 `FraudAmazon` 和 `Elliptic` 物理副本。
