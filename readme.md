# AMPLE - Implementation
## Vulnerability Detection with Graph Simplification and Enhanced Graph Representation Learning

## Introduction
Prior studies have demonstrated the effectiveness of Deep Learning (DL) in automated software vulnerability detection. Graph Neural Networks (GNNs) have proven effective in learning the graph representations of source code and are commonly adopted by existing DL-based vulnerability detection methods. However, the existing methods are still limited by the fact that GNNs are essentially difficult to handle the connections between long-distance nodes in a code structure graph. Besides, they do not well exploit the multiple types of edges in a code structure graph (such as edges representing data flow and control flow). Consequently, despite achieving state-of-the-art performance, the existing GNN-based methods tend to fail to capture global information (ie, long-range dependencies among nodes) of code graphs. 

To mitigate these issues, in this paper, we propose a novel vulnerability detection framework with grAph siMplification and enhanced graph rePresentation LEarning, named AMPLE. AMPLE mainly contains two parts: 1) graph simplification, which aims at reducing the distances between nodes by shrinking the node sizes of code structure graphs; 2) enhanced graph representation learning, which involves one edge-aware graph convolutional network module for fusing heterogeneous edge information into node representations and one kernel-scaled representation moule for well capturing the relations between distant graph nodes. Experiments on three public benchmark datasets show that AMPLE outperforms the state-of-the-art methods by 0.39%-35.32% and 7.64%-199.81% with respect to the accuracy and F1 score metrics, respectively. The results demonstrate the effectiveness of AMPLE in learning global information of code graphs for vulnerability detection.

## Dataset
To investigate the effectiveness of AMPLE, we adopt three vulnerability datasets from these paper: 
* Fan et al. [1]: <https://drive.google.com/file/d/1-0VhnHBp9IGh90s2wCNjeCMuy70HPl8X/view?usp=sharing>
* Reveal [2]: https://drive.google.com/drive/folders/1KuIYgFcvWUXheDhT--cBALsfy1I4utOy
* FFMPeg+Qemu [3]: https://drive.google.com/file/d/1x6hoF7G-tSYxg8AFybggypLZgMGDNHfF

## Requirement
Our code is based on Python3 (>= 3.7). There are a few dependencies to run the code. The major libraries are listed as follows:
* torch  (==1.9.0)
* dgl  (==0.7.2)
* numpy  (==1.22.3)
* sklearn  (==0.0)
* pandas  (==1.4.1)
* tqdm

**Default settings in AMPLE**:
* Training configs: 
    * batch_size = 64, lr = 0.0001, epoch = 100, patience = 20
    * opt ='RAdam', weight_decay=1e-6

## Preprocessing
We use Joern to generate the code structure graph and we provide a compiled version of joern [here](https://zenodo.org/record/7323504#.Y3OQL3ZByUk). It should be noted that the AST and graphs generated by different versions of Joern may have significant differences. So if using the newer versions of Joern to generate code structure graph, the model may have a different performance compared with the results we reported in the paper.

After parsing the functions with joern, the code for graph construction and simplification is under the ```data_processing\``` folder. ```data_processing\word2vec.py``` is used to train word2vec model. ```data_processing\utils.py``` includes the tokenizer. 

## Running the model
The model implementation code is under the ``` model\``` folder. The model can be runned from ```model\main.py```.

## Attention weight
We provide all the attention weights learned by our proposed model AMPLE for the test samples. Each dataset corresponds to a json file under ```attention weight\``` folder.

## Experiment results
#### PR-AUC & MCC metrics
<table>
   <tr>
      <td></td>
      <td>              Devign</td>
      <td></td>
      <td></td>
      <td>     Reveal</td>
      <td></td>
      <td></td>
      <td>                                 Fan</td>
      <td></td>
      <td></td>
   </tr>
   <tr>
      <td></td>
      <td>       PR-AUC</td>
      <td>MCC      G-mean</td>
      <td>PR-AUC</td>
      <td>  MCC</td>
      <td>G-mean</td>
      <td>PR-AUC</td>
      <td>MCC      G-mean</td>
   </tr>
   <tr>
      <td>Reveal</td>
      <td>0.6972</td>
      <td>0.2398</td>
      <td>0.6127</td>
      <td>0.4841</td>
      <td>0.3457</td>
      <td>0.7176</td>
      <td>0.2748</td>
      <td>0.1783</td>
      <td>0.5544</td>
   </tr>
   <tr>
      <td>AMPLE</td>
      <td>0.7347</td>
      <td>0.2995</td>
      <td>0.6069</td>
      <td>0.5061</td>
      <td>0.4464</td>
      <td>0.6672</td>
      <td>0.3383</td>
      <td>0.286</td>
      <td>0.5762</td>
   </tr>
   <tr>
      <td></td>
   </tr>
   <tr>
      <td></td>
   </tr>
</table>


## References
[1] Jiahao Fan, Yi Li, Shaohua Wang, and Tien Nguyen. 2020. A C/C++ Code Vulnerability Dataset with Code Changes and CVE Summaries. In The 2020 International Conference on Mining Software Repositories (MSR). IEEE.

[2] Saikat Chakraborty, Rahul Krishna, Yangruibo Ding, and Baishakhi Ray. 2020. Deep Learning based Vulnerability Detection: Are We There Yet? arXiv preprint arXiv:2009.07235 (2020).

[3] Yaqin Zhou, Shangqing Liu, Jingkai Siow, Xiaoning Du, and Yang Liu. 2019. Devign: Effective vulnerability identification by learning comprehensive program semantics via graph neural networks. In Advances in Neural Information Processing Systems. 10197–10207.

