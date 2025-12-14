## 🧰 Tools and Libraries
- Python 3.11.14
<!-- - transformers 4.57.1
- torch 2.9.1
- sentencepiece 0.2.1
- pandas 2.3.3
- jupytrlab 4.4.7
- ipywidgets 8.1.7
- ipykernel 6.31.0 -->

## Setup Walkthrough
- set up a new environment (referred to here as "rag_local")
```shell
conda activate rag_local
```
- install jupyter and ipython kernel package inside new env:
```shell
conda install jupyter ipykernel -y 
```
- register the env as a kernel:
```shell
python -m ipykernel install --user --name=rag_local --display-name "RAG (Local)"
```
- re-run environment in Anaconda to see install updates
- launch JupyterLabs to verify "rag_local" is in the kernel menu
- install core packages:
```shell
conda update -n base -c defaults conda -y
pip install --upgrade pip
```
(CUDA support)
```shell
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
```
(what's an Ada GPU?)
```shell
pip install sentence-transformers chromadb transformers accelerate bitsandbytes
```
optional:
```shell
pip install langchain jinja2 tqdm
```
install JupyterLab extensions:
```shell
conda install jupyterlab -y
pip install jupyterlab_widgets ipywidgets
```