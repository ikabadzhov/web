---
title: Machine learning with ROOT
layout: single
sidebar:
  nav: "manual"
toc: true
toc_sticky: true
---

Machine learning plays an important role in a variety of HEP use-cases. ROOT
offers native support for supervised learning techniques, such as multivariate
classification (both binary and multi class) and regression. It also allows easy
interoperability with commonly used machine learning libraries.

## TMVA

TMVA is the ROOT library that provides the interfaces and implementations of the
above mentioned machine learning techniques. The package includes:

- Neural networks
- Deep networks
- Multilayer perceptron
- Boosted/Bagged decision trees
- Function discriminant analysis (FDA)
- Multidimensional probability density estimation (PDE - range-search approach)
- Multidimensional k-nearest neighbour classifier
- Predictive learning via rule ensembles (RuleFit)
- Projective likelihood estimation (PDE approach)
- Rectangular cut optimisation
- Support Vector Machine (SVM)

{% include tutorials name="TMVA" url="tmva" %}

### Ingesting a dataset for training in TMVA

A ROOT dataset can be easily ingested for training using a {% include ref
class="DataLoader" namespace="TMVA" %}. A common use-case in HEP is to have the
information of physics events split in signal and background, as in the example
below:

_**Example**_

{% highlight C++ %}
void load_data(){
   // open file and retrieve trees
   auto inputfile = TFile::Open("http://root.cern/files/tmva_class_example.root");
   auto signaltree = inputfile->Get<TTree>("TreeS");
   auto backgroundtree = inputfile->Get<TTree>("TreeB");

   // load trees into TMVA
   TMVA::DataLoader loader{"mydataloader"};
   loader.AddSignalTree(signaltree);
   loader.AddBackgroundTree(backgroundtree);
}
{% endhighlight %}

The loaded data can then be passed to the {% include ref class="Factory"
namespace="TMVA" %} class for training, as shown in
[this tutorial](https://root.cern/doc/master/TMVAClassification_8C.html){:target="_blank"}.

## Interoperability with machine learning libraries

Machine learning is a widely researched topic. Many libraries implement
classification and regression techniques, with a broader scope or in more
specific fields. These libraries also vary in the programming languages offered
by their APIs, although it is true that often Python defines a common ground.
Notable examples include [Keras](https://keras.io/){:target="_blank"},
[PyTorch](https://pytorch.org/){:target="_blank"} and
[scikit-learn](https://scikit-learn.org){:target="_blank"}.

### Training in TMVA using a model created with an external library

Each library has its own API to create a model. With TMVA, you can create the
model that should be trained with an external library you may be already familiar
with (e.g. Keras). Then, you can save the model to a file and load it in a TMVA
application to train it and test it. In particular, the entire application could
be written as a single Python script thanks to
[PyROOT]({{ '/manual/python' | relative_url }}). A good example of this can be
found in [this tutorial](https://root.cern/doc/master/ClassificationKeras_8py.html){:target="_blank"}.

### ROOT data to Numpy arrays for further processing

Most of the external machine learning libraries will accept (or expect) a
collection of Numpy arrays as the input dataset, either for training or testing.
It is possible to seamlessly export data stored in ROOT files (e.g. as a {%
include ref class="TTree" %}) into Numpy arrays through {% include ref
class="RDataFrame" namespace="ROOT" %}. The data can then be used, for example,
to train an [XGBoost](https://xgboost.ai/){:target="_blank"} model. A recipe can
be found [here](https://root.cern/doc/master/tmva101__Training_8py.html){:target="_blank"}.

> **Where to go from here**
>
> An in-depth explanation of the algorithms and interfaces in the TMVA library
> can be found in its [topical manual]({{ '/topical/#tmva' | relative_url }}).

## Training examples

- {% include tutorial name="TMVAClassification" %} provides examples for the training and testing of TMVA classifiers.

- {% include tutorial name="TMVAMulticlass" %} provides an example for the training and testing of a TMVA multi-class classification.

- {% include tutorial name="TMVARegression" %} provides examples for the training and testing of TMVA classifiers.

## Application examples

- {% include tutorial name="TMVAClassificationApplication" %} provides an example on how to use the trained classifiers within an analysis module.

- {% include tutorial name="TMVAMulticlassApplication" %} provides an example on how to use the trained multi-class classifiers within an analysis module.

- {% include tutorial name="TMVARegressionApplication" %} provides an example on how to use the trained regression MVAs within an analysis module.

_**Example**_

{% include tutorial name="TMVAClassification" %} uses an academic toy data set for training and testing. It consists of four linearly correlated, Gaussian distributed discriminating input variables, with different sample means for signal and background.

The training job provides a formatted output logging that contains the following information:
- Linear correlation matrices for the input variables.
- Correlation ratios and mutual information between input variables and regression targets.
- Variable ranking.
- Summaries of the MVA configurations.
- Goodness-of-fit evaluation for PDFs.
- Signal and background (or regression target) correlations between the various MVA methods.
- Decision overlaps.
- Signal efficiencies at benchmark background rejection rates.
- Other performance estimators.

After a successful training, TMVA provides so called "weight"-files (here in the `TMVA.root` ROOT file) that contain all information necessary to recreate the method without retraining.

In addition, a GUI is provided to execute macros for displaying training, test and evaluation results.

{% include figure_image
   img="tmva-gui.png"
   caption="GUI for TMVA."
%}

You can, for example, plot input variable distributions.

{% include figure_image
   img="input-variables.png"
   caption="Example plots for input variable distributions."
%}

