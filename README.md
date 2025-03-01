<div align="center">

# Tracking with ML

<figure>
    <img src="https://raw.githubusercontent.com/HSF-reco-and-software-triggers/Tracking-ML-Exa.TrkX/master/docs/media/final_wide.png" width="250"/>
</figure>
    
### Exa.TrkX Collaboration

## Introduction
This is a fork of a repository from the Exa.TrkX collaboration which contains a collection of ML pipelines. My contributions to this project were rewriting an algorithm for performing edge contraction on a graph to use vectorized PyTorch functions instead of straight-line functions. This rewritten algorithm can be found in `src/Pipelines/TrackML_Example/LightningModules/GNN/EdgePooling.py`. Additionally, I gave a presentation on this algorithm which can be found [here](https://iris-hep.org/fellows/JeremyFerguson.html)

[Documentation](https://hsf-reco-and-software-triggers.github.io/Tracking-ML-Exa.TrkX/)

[![make_docs](https://github.com/HSF-reco-and-software-triggers/Tracking-ML-Exa.TrkX/actions/workflows/make_docs.yml/badge.svg)](https://github.com/HSF-reco-and-software-triggers/Tracking-ML-Exa.TrkX/actions/workflows/make_docs.yml) [![ci](https://github.com/HSF-reco-and-software-triggers/Tracking-ML-Exa.TrkX/actions/workflows/ci.yml/badge.svg)](https://github.com/HSF-reco-and-software-triggers/Tracking-ML-Exa.TrkX/actions/workflows/ci.yml) [![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)


</div>

Welcome to repository and documentation for ML pipelines and techniques by the ExatrkX Collaboration. 

## Objectives

1. To present the ExaTrkX Project pipeline for track reconstruction of TrackML and ITk data in a clear and simple way
2. To present a set of templates, best practices and results gathered from significant trial and error, to speed up the development of others in the domain of machine learning for high energy physics. We focus on applications specific to detector physics, but many tools can be applied to other areas, and these are collected in an application-agnostic way in the [Tools](https://hsf-reco-and-software-triggers.github.io/Tracking-ML-Exa.TrkX/tools/overview/) section.

### Disclaimer:

This repository has been functional, but ugly. It is moving to an "alpha" version whereby the abstract engineering for running the pipeline has been moved to a new library [TrainTrack](https://github.com/murnanedaniel/train-track/), and keeping physics-specific research code in this repository. This transition is expected before May 2021. Please be a little patient if using before then, and if something is broken, pull first to make sure it's not already solved, then post an issue second.

## Intro

To start as quickly as possible, clone the repository, [Install](https://hsf-reco-and-software-triggers.github.io/Tracking-ML-Exa.TrkX/pipelines/quickstart) and follow the steps in [Quickstart](https://hsf-reco-and-software-triggers.github.io/Tracking-ML-Exa.TrkX/pipelines/quickstart). This will get you generating toy tracking data and running inference immediately. Many of the choices of structure will be made clear there. If you already have a particle physics problem in mind, you can apply the [Template](https://hsf-reco-and-software-triggers.github.io/Tracking-ML-Exa.TrkX/pipelines/choosingguide.md) that is most suitable to your use case.

Once up and running, you may want to consider more complex ML [Models](https://hsf-reco-and-software-triggers.github.io/Tracking-ML-Exa.TrkX/models/overview/). Many of these are built on other libraries (for example [Pytorch Geometric](https://github.com/rusty1s/pytorch_geometric)).

<div align="center">
<figure>
  <img src="https://raw.githubusercontent.com/HSF-reco-and-software-triggers/Tracking-ML-Exa.TrkX/master/docs/media/application_diagram_1.png" width="600"/>
</figure>
</div>

## Install

It's recommended to start a conda environment before installation:

```
conda create --name exatrkx-tracking python=3.8
conda activate exatrkx-tracking
pip install pip --upgrade
```

If you have a CUDA GPU available, load the toolkit or [install it](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) now. You should check that this is done by running `nvcc --version`. Then, running:

```
python install.py
```

will **attempt** to negotiate a path through the packages required, using `nvcc --version` to automatically find the correct wheels. **Warning**: If you are installing with cpu, this may take up to 15 minutes due to an unfortunately slow installation of Pytorch3D from source.

You should be ready for the [Quickstart](https://hsf-reco-and-software-triggers.github.io/Tracking-ML-Exa.TrkX/pipelines/quickstart)!

If this doesn't work, you can step through the process manually:

<table style="border: 1px solid gray; border-collapse: collapse">
<tr style="border-bottom: 1px solid gray">
<th style="border-bottom: 1px solid gray"> CPU </th>
<th style="border-left: 1px solid gray"> GPU </th>
</tr>
<tr>
<td style="border-bottom: 1px solid gray">

1. Run 
`export CUDA=cpu`
    
</td>
<td style="border-left: 1px solid gray">

1a. Find the GPU version cuda XX.X with `nvcc --version`
    
1b. Run `export CUDA=cuXXX`, with `XXX = 92, 101, 102, 110`

</td>
</tr>
<tr style="border-bottom: 1px solid gray">
<td colspan="2">

2. Install Pytorch and dependencies 

```
    pip install --user -r requirements.txt
```

</td>
</tr>
<tr style="border-bottom: 1px solid gray">
<td colspan="2">

3. Install local packages

```pip install -e .```
    
</td>
</tr>
<tr>
<td style="border-bottom: 1px solid gray">

4. Install CPU-optimized packages

```
pip install faiss-cpu
pip install "git+https://github.com/facebookresearch/pytorch3d.git@stable" 
``` 
    
    
</td>
<td style="border-left: 1px solid gray">

    
4. Install GPU-optimized packages

```pip install faiss-gpu cupy-cudaXXX```, with `XXX`    

```
pip install pytorch3d -f https://dl.fbaipublicfiles.com/pytorch3d/packaging/wheels/py3{Y}_cu{XXX}_pyt{ZZZ}/download.html
```
    
where `{Y}` is the minor version of Python 3.{Y}, `{XXX}` is as above, and `{ZZZ}` is the version of Pytorch {Z.ZZ}.

    e.g. `py36_cu101_pyt170` is Python 3.6, Cuda 10.1, Pytorch 1.70.
   
    
</td>
</tr>
</table>

### Vintage Errors

A very possible error will be
```
OSError: libcudart.so.XX.X: cannot open shared object file: No such file or directory
```
This indicates a mismatch between CUDA versions. Identify the library that called the error, and ensure there are no versions of this library installed in parallel, e.g. from a previous `pip --user` install.
