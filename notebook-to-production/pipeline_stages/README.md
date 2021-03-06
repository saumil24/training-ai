# Breaking our workflow up into pipeline stages

This material walks you through breaking up a workflow, contained in a Jupyter notebook, into separate, scalable pipeline stages. Once you are done with this material, you will understand which portions of a ML/AI pipeline might benefit from being managed in isolation. You will also get some experience writing code for specific stages of a data pipeline (pre-processing, training, inference).

This guide will walk you through:

1. [Discussing why we might want to split code into pipeline stages](#1-discussing-why-we-might-want-to-split-code)
2. [Creating our pre-processing stage](#2-creating-our-pre-processing-stage)
3. [Creating our training stage](#3-creating-our-training-stage)
4. [Creating our inference stage](#4-creating-our-inference-stage)

It also includes a [list of resources](#resources) for those that want to dive in a little bit deeper.

## 1. Discussing why we might want to split code

Sure, you could keep all your code together and even run it in a notebook on another machine. However, keeping all the steps of your Python workflow together may not make sense. Let's think about why. Why would splitting up our code into separate pieces of code (or "pipeline stages") be usefule in terms of:

- development
- deployment
- scaling
- workflow mangement
- integrations

By the end of this discussion we should decide:

- how we want to split our example workflow, and
- why.

The "why" is always important here. There are trade offs in splitting up your code, and we need to justify the extra complication.

## 2. Creating our pre-processing stage

Let's start at the beginning of the pipeline (pre-processing), and take our first leap out of the notebook. We need to do this because:

- We need to run this code non-interactively in the pipeline.
- We should (especially before production use) augment this code with unit/integration tests and automated CI/CD processes. (Not covered here)

The code will resemble the code from our notebook, with some key differences:

- We won't include unnecessary imports.
- We will add some command line arguments to make our life easier.
- We will need to think about how to get data out of each pipeline stage (such that it can be read as input by the next pipeline stage).

Let's take a look. Our first example pipeline stage can be found [here](example1/example1.py).

## 3. Creating our training stage

Ok, we are off to the races. Now it's your turn! 

**Exercise 1** - Create a similar Python script that will:

- read in the `x_train.csv` and `y_train.csv` from our pre-processing stage
- initialize our PyTorch model
- train our model
- save our moel to a file

You can start with [template1.py](exercise1/template1.py) and fill in the missing pieces. Check [solution1.py](exercise1/solution1.py) when you are ready. To modify `template1.py`, you can use an editor built into the terminal directly on your workshop instance (e.g., `vim` or `nano`), or you could copy over the contents of `template1.py` to your local machine and use your editor of choice. 

If you do the latter, you could copy your modified version back over to your workshop instance via `scp`, or you could copy the contents into the template file or a new file. If you need help with this please ask the instructor. You want need to do this sort of editing in subsequent sections of the course, so you can rest easy.

*Hint* - You can save the model to a file using `torch.save()`.

## 4. Creating our inference stage

We're almost there! We just need our last inference stage.

**Exercise 2** - Create a Python script that will:

- read in the saved model from exercise 1 above
- read in a file containing features, on which we need to perform inferences
- use the model to perform the inferences
- save the results

You can start with [template2.py](exercise2/template2.py) and check [solution2.py](exercise2/solution2.py) when you are ready.

## 5. Docker-izing our pipeline stages

Ok, we have our code for model training, inference, and pre-processing and we need to:

- scale this code up to larger data sets,
- run it automatically at certain times or based on certain events,
- share it with teammates so they can generate their own results, or
- connect it to other code running in our company's infrastructure.

How can we do this with a high degree of reproducibility and operational/computation efficiency? And how can we ensure that our engineering team doesn't hate the data science team because they always have to deploy data science things in a "special" way with "special" data science tools.

Docker solves many of these issues and even has additional benefits. Data scientists and AI researchers are also latching on to these because they can:

- Docker-ize an application quickly, hand it off to an engineering organization, and have them run it in a manner similar to any other application.
- Experiment with a huge number of tools (Tensorflow, PyTorch, Spark, etc.) without having to install anything other than Docker.
- Manage a diverse set of data pipeline stages in a unified way.
- Leverage the huge number of excellent infrastructure projects for containers (e.g., those powering Google scale work) to create application that auto-scale, self-heal, are fault tolerant, etc.
- Easily define and reproduce environments for experimentation.

We will be using Docker to make our pipeline stages portable. If you are interested in the specifics of the Docker images we are using, check out the [docker](docker) directory here.

## Resources

- [Python command line arguments](https://docs.python.org/3.3/library/argparse.html)
- [Saving/loading PyTorch models](http://pytorch.org/docs/0.3.1/notes/serialization.html#recommend-saving-models)

___
All material is licensed under the [Apache License Version 2.0, January 2004](http://www.apache.org/licenses/LICENSE-2.0).
