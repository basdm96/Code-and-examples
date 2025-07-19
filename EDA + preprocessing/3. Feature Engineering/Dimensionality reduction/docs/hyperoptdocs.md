# Master Document Index

This document combines multiple markdown files. Below is the index of the included files.

- [docs\README.md](#docs-readme)
- [docs\templates\getting-started\minimizing_functions.md](#docs-templates-getting-started-minimizing_functions)
- [docs\templates\getting-started\overview.md](#docs-templates-getting-started-overview)
- [docs\templates\getting-started\search_spaces.md](#docs-templates-getting-started-search_spaces)
- [docs\templates\index.md](#docs-templates-index)
- [docs\templates\interfacing-languages.md](#docs-templates-interfacing-languages)
- [docs\templates\related-work.md](#docs-templates-related-work)
- [docs\templates\scaleout\mongodb.md](#docs-templates-scaleout-mongodb)
- [docs\templates\scaleout\spark.md](#docs-templates-scaleout-spark)
- [docs\templates\scipy_submission.md](#docs-templates-scipy_submission)
- [docs\templates\setup\installation-notes.md](#docs-templates-setup-installation-notes)
- [docs\templates\setup\running-tests.md](#docs-templates-setup-running-tests)
- [docs\templates\tutorials\01.BasicTutorial.md](#docs-templates-tutorials-01.basictutorial)
- [docs\templates\tutorials\02.MultipleParameterTutorial.md](#docs-templates-tutorials-02.multipleparametertutorial)


---

## <a name="docs-readme"></a>File: docs\README.md

# Hyperopt documentation

To build and run Hyperopt documentation locally, you should install `mkdocs` first,
then auto-generate files from templates, build your `mkdocs` site and then run it:

```bash
pip install mkdocs
python autogen.py
mkdocs build && mkdocs serve
```

To deploy a new version of docs run:
```bash
mkdocs gh-deploy
```


---

## <a name="docs-templates-getting-started-minimizing_functions"></a>File: docs\templates\getting-started\minimizing_functions.md

# Defining a Function to Minimize

Hyperopt provides a few levels of increasing flexibility / complexity when it comes to specifying an objective function to minimize.
The questions to think about as a designer are

* Do you want to save additional information beyond the function return value, such as other statistics and diagnostic information collected during the computation of the objective?
* Do you want to use optimization algorithms that require more than the function value?
* Do you want to communicate between parallel processes? (e.g. other workers, or the minimization algorithm)

The next few sections will look at various ways of implementing an objective
function that minimizes a quadratic objective function over a single variable.
In each section, we will be searching over a bounded range from -10 to +10,
which we can describe with a *search space*:

```python
space = hp.uniform('x', -10, 10)
```

Refer to [this page](search_spaces.md) for information on how to specify search spaces that are more complicated.

## The Simplest Case

The simplest protocol for communication between hyperopt's optimization
algorithms and your objective function, is that your objective function
receives a valid point from the search space, and returns the floating-point
*loss* (aka negative utility) associated with that point.

```python
from hyperopt import fmin, tpe, hp
best = fmin(fn=lambda x: x ** 2,
            space=hp.uniform('x', -10, 10),
            algo=tpe.suggest,
            max_evals=100)
print(best)
```

This protocol has the advantage of being extremely readable and quick to
type. As you can see, it's nearly a one-liner.
The disadvantages of this protocol are
(1) that this kind of function cannot return extra information about each evaluation into the trials database,
and
(2) that this kind of function cannot interact with the search algorithm or other concurrent function evaluations.
You will see in the next examples why you might want to do these things.

## Attaching Extra Information via the Trials Object

If your objective function is complicated and takes a long time to run, you will almost certainly want to save more statistics
and diagnostic information than just the one floating-point loss that comes out at the end.
For such cases, the fmin function is written to handle dictionary return values.
The idea is that your loss function can return a nested dictionary with all the statistics and diagnostics you want.
The reality is a little less flexible than that though: when using mongodb for example,
the dictionary must be a valid JSON document.
Still, there is lots of flexibility to store domain specific auxiliary results.

When the objective function returns a dictionary, the fmin function looks for some special key-value pairs
in the return value, which it passes along to the optimization algorithm.
There are two mandatory key-value pairs:

* `status` - one of the keys from `hyperopt.STATUS_STRINGS`, such as 'ok' for
  successful completion, and 'fail' in cases where the function turned out to
  be undefined.
* `loss` - the float-valued function value that you are trying to minimize, if
  the status is 'ok' then this has to be present.

The fmin function responds to some optional keys too:

* `attachments` -  a dictionary of key-value pairs whose keys are short
  strings (like filenames) and whose values are potentially long strings (like
  file contents) that should not be loaded from a database every time we
  access the record. (Also, MongoDB limits the length of normal key-value
  pairs so once your value is in the megabytes, you may *have* to make it an
  attachment.)
* `loss_variance` - float - the uncertainty in a stochastic objective function
* `true_loss` - float -
  When doing hyper-parameter optimization, if you store the generalization error of your model with this name, then you can sometimes get spiffier output from the built-in plotting routines.
* `true_loss_variance` - float - the uncertainty in the generalization error

Since dictionary is meant to go with a variety of back-end storage
mechanisms, you should make sure that it is JSON-compatible.  As long as it's
a tree-structured graph of dictionaries, lists, tuples, numbers, strings, and
date-times, you'll be fine.

**HINT:** To store numpy arrays, serialize them to a string, and consider storing
them as attachments.

**HINT:** If you need to replicate the results of the stochastic search (e.g. for a demonstration),
pass an object of type `np.random.Generator` into the `fmin` function, using the `rstate` optional parameter.

Writing the function above in dictionary-returning style, it
would look like this:

```python
from hyperopt import fmin, tpe, hp, STATUS_OK


def objective(x):
    return {'loss': x ** 2, 'status': STATUS_OK }

best = fmin(objective,
            space=hp.uniform('x', -10, 10),
            algo=tpe.suggest,
            max_evals=100)

print(best)
```

## The Trials Object

To really see the purpose of returning a dictionary,
let's modify the objective function to return some more things,
and pass an explicit `trials` argument to `fmin`.

```python
import pickle
import time
from hyperopt import fmin, tpe, hp, STATUS_OK, Trials


def objective(x):
    return {
        'loss': x ** 2,
        'status': STATUS_OK,
        # -- store other results like this
        'eval_time': time.time(),
        'other_stuff': {'type': None, 'value': [0, 1, 2]},
        # -- attachments are handled differently
        'attachments':
            {'time_module': pickle.dumps(time.time)}
        }
trials = Trials()
best = fmin(objective,
            space=hp.uniform('x', -10, 10),
            algo=tpe.suggest,
            max_evals=100,
            trials=trials)

print(best)
```

In this case the call to fmin proceeds as before, but by passing in a trials object directly,
we can inspect all of the return values that were calculated during the experiment.

So for example:

* `trials.trials` - a list of dictionaries representing everything about the search
* `trials.results` - a list of dictionaries returned by 'objective' during the search
* `trials.losses()` - a list of losses (float for each 'ok' trial)
* `trials.statuses()` - a list of status strings

This trials object can be saved, passed on to the built-in plotting routines,
or analyzed with your own custom code.
Here is a simple example of one way to save and subsequently load a trials object.

```python
import pickle
from hyperopt import fmin, tpe, hp, Trials, STATUS_OK


def objective(x):
    return {'loss': x ** 2, 'status': STATUS_OK }

# Initialize an empty trials database
trials = Trials()

# Perform 100 evaluations on the search space
best = fmin(objective,
            space=hp.uniform('x', -10, 10),
            algo=tpe.suggest,
            trials=trials,
            max_evals=100)

# The trials database now contains 100 entries, it can be saved/reloaded with pickle or another method
pickle.dump(trials, open("my_trials.pkl", "wb"))
trials = pickle.load(open("my_trials.pkl", "rb"))

# Perform an additional 100 evaluations
# Note that max_evals is set to 200 because 100 entries already exist in the database
best = fmin(objective,
    space=hp.uniform('x', -10, 10),
    algo=tpe.suggest,
    trials=trials,
    max_evals=200)

print(best)
```

The *attachments* are handled by a special mechanism that makes it possible to use the same code
for both `Trials` and `MongoTrials`.

You can retrieve a trial attachment like this, which retrieves the 'time_module' attachment of the 5th trial:

```python
msg = trials.trial_attachments(trials.trials[5])['time_module']
time_module = pickle.loads(msg)
```

The syntax is somewhat involved because the idea is that attachments are large strings,
so when using MongoTrials, we do not want to download more than necessary.
Strings can also be attached globally to the entire trials object via `trials.attachments`,
which behaves like a string-to-string dictionary.

**N.B.** Currently, the trial-specific attachments to a Trials object are tossed into the same global trials attachment dictionary, but that may change in the future and it is not true of MongoTrials.

## The `Ctrl` Object for Realtime Communication with MongoDB

It is possible for `fmin()` to give your objective function a handle to the mongodb used by a parallel experiment. This mechanism makes it possible to update the database with partial results, and to communicate with other concurrent processes that are evaluating different points.
Your objective function can even add new search points, just like `rand.suggest`.

The basic technique involves:

* Using the `fmin_pass_expr_memo_ctrl` decorator
* call `pyll.rec_eval` in your own function to build the search space point
  from `expr` and `memo`.
* use `ctrl`, an instance of `hyperopt.Ctrl` to communicate with the live
  trials object.

It's normal if this doesn't make a lot of sense to you after this short tutorial,
but I wanted to give some mention of what's possible with the current code base,
and provide some terms to grep for in the hyperopt source, the unit test,
and example projects, such as [hyperopt-convnet](https://github.com/hyperopt/hyperopt-convnet).
Email me or file a github issue if you'd like some help getting up to speed with this part of the code.


---

## <a name="docs-templates-getting-started-overview"></a>File: docs\templates\getting-started\overview.md

# Getting started with Hyperopt

Hyperopt's job is to find the best value of a scalar-valued, possibly-stochastic function over a set of possible arguments to that function.

Whereas many optimization packages will assume that these inputs are drawn from a vector space,
Hyperopt is different in that it encourages you to describe your search space in more detail.
By providing more information about where your function is defined, and where you think the best values are, you allow algorithms in hyperopt to search more efficiently.

The way to use hyperopt is to describe:

* the objective function to minimize
* the space over which to search
* the database in which to store all the point evaluations of the search
* the search algorithm to use

This (most basic) tutorial will walk through how to write functions and search spaces,
using the default `Trials` database, and the dummy `rand` (random) search algorithm.
Section (1) is about the different calling conventions for communication between an objective function and hyperopt.
Section (2) is about describing search spaces.

Parallel search is possible when replacing the `Trials` database with
a `MongoTrials` one;
there is another wiki page on the subject of [using mongodb for parallel search](Parallelizing-Evaluations-During-Search-via-MongoDB).

Choosing the search algorithm is as simple as passing `algo=hyperopt.tpe.suggest` instead of `algo=hyperopt.rand.suggest`.
The search algorithms are actually callable objects, whose constructors
accept configuration arguments, but that's about all there is to say about the
mechanics of choosing a search algorithm.


---

## <a name="docs-templates-getting-started-search_spaces"></a>File: docs\templates\getting-started\search_spaces.md

# Defining a Search Space

A search space consists of nested function expressions, including stochastic expressions.
The stochastic expressions are the hyperparameters.
Sampling from this nested stochastic program defines the random search algorithm.
The hyperparameter optimization algorithms work by replacing normal "sampling" logic with
adaptive exploration strategies, which make no attempt to actually sample from the distributions specified in the search space.

It's best to think of search spaces as stochastic argument-sampling programs. For example

```python
from hyperopt import hp
space = hp.choice('a',
    [
        ('case 1', 1 + hp.lognormal('c1', 0, 1)),
        ('case 2', hp.uniform('c2', -10, 10))
    ])
```

The result of running this code fragment is a variable `space` that refers to a graph of expression identifiers and their arguments.
Nothing has actually been sampled, it's just a graph describing *how* to sample a point.
The code for dealing with this sort of expression graph is in `hyperopt.pyll` and I will refer to these graphs as *pyll graphs* or *pyll programs*.

If you like, you can evaluate a sample space by sampling from it.

```python
import hyperopt.pyll.stochastic
print(hyperopt.pyll.stochastic.sample(space))
```

This search space described by `space` has 3 parameters:

* 'a' - selects the case
* 'c1' - a positive-valued parameter that is used in 'case 1'
* 'c2' - a bounded real-valued parameter that is used in 'case 2'

One thing to notice here is that every optimizable stochastic expression has a *label* as the first argument.
These labels are used to return parameter choices to the caller, and in various ways internally as well.

A second thing to notice is that we used tuples in the middle of the graph (around each of 'case 1' and 'case 2').
Lists, dictionaries, and tuples are all upgraded to "deterministic function expressions" so that they can be part of the search space stochastic program.

A third thing to notice is the numeric expression `1 + hp.lognormal('c1', 0, 1)`, that is embedded into the description of the search space.
As far as the optimization algorithms are concerned, there is no difference between adding the 1 directly in the search space
and adding the 1 within the logic of the objective function itself.
As the designer, you can choose where to put this sort of processing to achieve the kind modularity you want.
Note that the intermediate expression results within the search space can be arbitrary Python objects, even when optimizing in parallel using mongodb.
It is easy to add new types of non-stochastic expressions to a search space description, see below (Section 2.3) for how to do it.

A fourth thing to note is that 'c1' and 'c2' are examples what we will call *conditional parameters*.
Each of 'c1' and 'c2' only figures in the returned sample for a particular value of 'a'.
If 'a' is 0, then 'c1' is used but not 'c2'.
If 'a' is 1, then 'c2' is used but not 'c1'.
Whenever it makes sense to do so, you should encode parameters as conditional ones this way,
rather than simply ignoring parameters in the objective function.
If you expose the fact that 'c1' sometimes has no effect on the objective function (because it has no effect on the argument to the objective function) then search can be more efficient about credit assignment.

## Parameter Expressions

The stochastic expressions currently recognized by hyperopt's optimization algorithms are:

* `hp.choice(label, options)`
  * Returns one of the options, which should be a list or tuple.
       The elements of `options` can themselves be [nested] stochastic expressions.
       In this case, the stochastic choices that only appear in some of the options become *conditional* parameters.

* `hp.pchoice(label, p_list)`
  * Returns one of the options, where p_list is a list of (probability, option) pairs.
        The elements of options can themselves be [nested] stochastic expressions. 
        In this case, the stochastic choices that only appear in some of the options become conditional parameters.
    
* `hp.randint(label[, low], upper)`
  * Returns a random integer in the range [low, upper). The default low value is 0. The semantics of this
       distribution is that there is *no* more correlation in the loss function between nearby integer values,
       as compared with more distant integer values.  This is an appropriate distribution for describing random seeds    for example.
       If the loss function is probably more correlated for nearby integer values, then you should probably use one of the "quantized" continuous distributions, such as either `quniform`, `qloguniform`, `qnormal` or `qlognormal`.

* `hp.uniform(label, low, high)`
  * Returns a value uniformly between `low` and `high`.
  * When optimizing, this variable is constrained to a two-sided interval.

* `hp.quniform(label, low, high, q)`
  * Returns a value like round(uniform(low, high) / q) * q
  * Suitable for a discrete value with respect to which the objective is still somewhat "smooth", but which should be bounded both above and below.

* `hp.uniformint(label, low, high)`
  * Returns a integer value uniformly between `low` and `high`
  * Equivalent to `hp.quniform(label, low, high, 1.0)`
  * Suitable for a discrete integer value with respect to which the objective is still somewhat "smooth", but which should be bounded both above and below.

* `hp.loguniform(label, low, high)`
  * Returns a value drawn according to exp(uniform(low, high)) so that the logarithm of the return value is uniformly distributed.
  * When optimizing, this variable is constrained to the interval [exp(low), exp(high)].

* `hp.qloguniform(label, low, high, q)`
  * Returns a value like round(exp(uniform(low, high)) / q) * q
  * Suitable for a discrete variable with respect to which the objective is "smooth" and gets smoother with the size of the value, but which should be bounded both above and below.

* `hp.normal(label, mu, sigma)`
  * Returns a real value that's normally-distributed with mean mu and standard deviation sigma. When optimizing, this is an unconstrained variable.

* `hp.qnormal(label, mu, sigma, q)`
  * Returns a value like round(normal(mu, sigma) / q) * q
  * Suitable for a discrete variable that probably takes a value around mu, but is fundamentally unbounded.

* `hp.lognormal(label, mu, sigma)`
  * Returns a value drawn according to exp(normal(mu, sigma)) so that the logarithm of the return value is normally distributed.
        When optimizing, this variable is constrained to be positive.

* `hp.qlognormal(label, mu, sigma, q)`
  * Returns a value like round(exp(normal(mu, sigma)) / q) * q
  * Suitable for a discrete variable with respect to which the objective is smooth and gets smoother with the size of the variable, which is bounded from one side.

## A Search Space Example: scikit-learn

To see all these possibilities in action, let's look at how one might go about describing the space of hyperparameters of classification algorithms in scikit-learn.
(This idea is being developed in [hyperopt-sklearn](https://github.com/hyperopt/hyperopt-sklearn))

```python
from hyperopt import hp
space = hp.choice('classifier_type', [
    {
        'type': 'naive_bayes',
    },
    {
        'type': 'svm',
        'C': hp.lognormal('svm_C', 0, 1),
        'kernel': hp.choice('svm_kernel', [
            {'ktype': 'linear'},
            {'ktype': 'RBF', 'width': hp.lognormal('svm_rbf_width', 0, 1)},
            ]),
    },
    {
        'type': 'dtree',
        'criterion': hp.choice('dtree_criterion', ['gini', 'entropy']),
        'max_depth': hp.choice('dtree_max_depth',
                     [None, hp.qlognormal('dtree_max_depth_int', 3, 1, 1)]),
        'min_samples_split': hp.qlognormal('dtree_min_samples_split', 2, 1, 1),
    },
    ])
```

## Adding Non-Stochastic Expressions with pyll

You can use such nodes as arguments to pyll functions (see pyll).
File a github issue if you want to know more about this.

In a nutshell, you just have to decorate a top-level (i.e. pickle-friendly) function so
that it can be used via the `scope` object.

```python
import hyperopt.pyll
from hyperopt.pyll import scope


@scope.define
def foo(a, b=0):
     print('runing foo', a, b)
     return a + b / 2

# -- this will print 0, foo is called as usual.
print(foo(0))

# In describing search spaces you can use `foo` as you
# would in normal Python. These two calls will not actually call foo,
# they just record that foo should be called to evaluate the graph.

space1 = scope.foo(hp.uniform('a', 0, 10))
space2 = scope.foo(hp.uniform('a', 0, 10), hp.normal('b', 0, 1))

# -- this will print an pyll.Apply node
print(space1)

# -- this will draw a sample by running foo()
print(hyperopt.pyll.stochastic.sample(space1))
```

## Adding New Kinds of Hyperparameter

Adding new kinds of stochastic expressions for describing parameter search spaces should be avoided if possible.
In order for all search algorithms to work on all spaces, the search algorithms must agree on the kinds of hyperparameter that describe the space.
As the maintainer of the library, I am open to the possibility that some kinds of expressions should be added from time to time, but like I said, I would like to avoid it as much as possible.
Adding new kinds of stochastic expressions is not one of the ways hyperopt is meant to be extensible.


---

## <a name="docs-templates-index"></a>File: docs\templates\index.md

# Hyperopt: Distributed Asynchronous Hyper-parameter Optimization

{{autogenerated}}

---

## <a name="docs-templates-interfacing-languages"></a>File: docs\templates\interfacing-languages.md

# Interfacing Hyperopt with other programming languages

There are basically two ways to interface hyperopt with other languages:

1. you can write a Python wrapper around your cost function that is not written in Python, or
2. you can replace the `hyperopt-mongo-worker` program and communicate with MongoDB directly using JSON.

## Wrapping a call to non-Python code

The easiest way to use hyperopt to optimize the arguments to a non-python function, such as for example an external executable, is to write a Python function wrapper around that external executable. Supposing you have an executable `foo` that takes an integer command-line argument `--n` and prints out a score, you might wrap it like this:

```python
import subprocess


def foo_wrapper(n):
    # Optional: write out a script for the external executable
    # (we just call foo with the argument proposed by hyperopt)
    proc = subprocess.Popen(['foo', '--n', n], stdout=subprocess.PIPE)
    proc_out, proc_err = proc.communicate()
    # <you might have to do some more elaborate parsing of foo's output here>
    score = float(proc_out)
    return score
```

Of course, to optimize the `n` argument to `foo` you also need to call hyperopt.fmin, and define the search space. I can only imagine that you will want to do this part in Python.

```python
from hyperopt import fmin, hp, rand

best_n = fmin(foo_wrapper, hp.quniform('n', 1, 100, 1), algo=rand.suggest)

print(best_n)
```

When the search space is larger than the simple one here, you might want or need the wrapper function to translate its argument into some kind of configuration file/script for the external executable.

This approach is perfectly compatible with MongoTrials.

## Communicating with MongoDB Directly

It is possible to interface more directly with the search process (when using MongoTrials) by communicating with MongoDB directly, just like `hyperopt-mongo-worker` does. It's beyond the scope of a tutorial to explain how to do this, but Hannes Schultz (@[temporaer](https://github.com/temporaer)) got hyperopt working with his MDBQ project, which is a standalone mongodb-based task queue:

[Hyperopt C++ Client](https://github.com/temporaer/MDBQ/blob/master/src/example/hyperopt_client.cpp)

Have a look at that code, as well as the contents of [hyperopt/mongoexp.py](https://github.com/jaberg/hyperopt/blob/master/hyperopt/mongoexp.py) to understand how worker processes are expected to reserve jobs in the work queue, and store results back to MongoDB.


---

## <a name="docs-templates-related-work"></a>File: docs\templates\related-work.md

# Related work

Links to software related to Hyperopt, and Bayesian Optimization in general.

## Software using Hyperopt

* [hyperopt-sklearn](https://github.com/hyperopt/hyperopt-sklearn) - using hyperopt to optimize across [sklearn](http://scikit-learn.org) estimators.
* [hyperopt-convnet](https://github.com/hyperopt/hyperopt-convnet) - optimize convolutional architectures for image classification
  * used in Bergstra, Yamins, and Cox in (ICML 2013).
* [hyperopt-dbn](https://github.com/hyperopt/hyperopt-nnet) - optimize Deep Belief Networks (Coming Soon)
  * used in [Bergstra, Bardenet, Bengio, and Kegl (NIPS 2011)](http://www.eng.uwaterloo.ca/~jbergstr/files/pub/11_nips_hyperopt.pdf)
  * used in [Bergstra and Bengio (JMLR 2012)](http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf)
* [hyperas](https://github.com/maxpumperla/hyperas) - hyperopt wrapper for [keras](https://keras.io)

## Other Software for Bayesian Optimization

* [SMAC](http://www.cs.ubc.ca/labs/beta/Projects/SMAC/#software) - Sequential Model-based Algorithm Configuration (based on regression trees).
* [Spearmint](http://www.cs.toronto.edu/~jasper/software.html) - Gaussian-process SMBO in Python.
* [BayesOpt](http://rmcantin.bitbucket.org/html/) - Bayesian optimization toolbox

Should other software be listed here? File a github issue to add it.


---

## <a name="docs-templates-scaleout-mongodb"></a>File: docs\templates\scaleout\mongodb.md

# Parallelizing Evaluations During Search via MongoDB

Hyperopt is designed to support different kinds of trial databases.
The default trial database (`Trials`) is implemented with Python lists and dictionaries.
The default implementation is a reference implementation and it is easy to work with,
but it does not support the asynchronous updates required to evaluate trials in parallel.
For parallel search, hyperopt includes a `MongoTrials` implementation that supports asynchronous updates.

To run a parallelized search, you will need to do the following (after [installing mongodb](Installation-Notes)):

1. Start a mongod process somewhere network-visible.

2. Modify your call to `hyperopt.fmin` to use a MongoTrials backend connected to that mongod process.

3. Start one or more `hyperopt-mongo-worker` processes that will also connect to the mongod process,
    and carry out the search while `fmin` blocks.

## Start a mongod process

Once mongodb is installed, starting a database process (mongod) is as easy as typing e.g.

```bash
mongod --dbpath . --port 1234
# or storing each db its own directory is nice:
mongod --dbpath . --port 1234 --directoryperdb --journal --nohttpinterface
# or consider starting mongod as a daemon:
mongod --dbpath . --port 1234 --directoryperdb --fork --journal --logpath log.log --nohttpinterface
```

Mongo has a habit of pre-allocating a few GB of space (you can disable this with --noprealloc) for better performance, so think a little about where you want to create this database.
Creating a database on a networked filesystem may give terrible performance not only to your database but also to everyone else on your network, be careful about it.

Also, if your machine is visible to the internet, then either bind to the loopback interface and connect via ssh or read mongodb's documentation on password protection.

The rest of the tutorial is based on mongo running on **port 1234** of the **localhost**.

## Use MongoTrials

Suppose, to keep things really simple, that you wanted to minimize the `math.sin` function with hyperopt.
To run things in-process (serially) you could type things out like this:

```python
import math
from hyperopt import fmin, tpe, hp, Trials

trials = Trials()
best = fmin(math.sin, hp.uniform('x', -2, 2), trials=trials, algo=tpe.suggest, max_evals=10)
```

To use the mongo database for persistent storage of the experiment, use a `MongoTrials` object instead of `Trials` like this:

```python
import math
from hyperopt import fmin, tpe, hp
from hyperopt.mongoexp import MongoTrials

trials = MongoTrials('mongo://localhost:1234/foo_db/jobs', exp_key='exp1')
best = fmin(math.sin, hp.uniform('x', -2, 2), trials=trials, algo=tpe.suggest, max_evals=10)
```

The first argument to MongoTrials tells it what mongod process to use, and which *database* (here 'foo_db') within that process to use.
The second argument (`exp_key='exp_1'`) is useful for tagging a particular set of trials *within* a database.
The exp_key argument is technically optional.

**N.B.** There is currently an implementation requirement that the database name be followed by '/jobs'.

Whether you always put your trials in separate databases or whether you use the exp_key mechanism to distinguish them is up to you.
In favour of databases: they can be manipulated from the shell (they appear as distinct files) and they ensure greater independence/isolation of experiments.
In favour of exp_key: hyperopt-mongo-worker processes (see below) poll at the database level so they can simultaneously support multiple experiments that are using the same database.

## Run `hyperopt-mongo-worker`

If you run the code fragment above, you will see that it blocks (hangs) at the call fmin.
MongoTrials describes itself internally to fmin as an *asynchronous* trials object, so fmin
does not actually evaluate the objective function when a new search point has been suggested.
Instead, it just sits there, patiently waiting for another process to do that work and update the mongodb with the results.
The `hyperopt-mongo-worker` script included in the `bin` directory of hyperopt was written for this purpose.
It should have been installed on your `$PATH` when you installed hyperopt.

While the `fmin` call in the script above is blocked, open a new shell and type

```bash
hyperopt-mongo-worker --mongo=localhost:1234/foo_db --poll-interval=0.1
```

It will dequeue a work item from the mongodb, evaluate the `math.sin` function, store the results back to the database.
After the `fmin` function has tried enough points it will return and the script above will terminate.
The `hyperopt-mongo-worker` script will then sit around for a few minutes waiting for more work to appear, and then terminate too.

We set the poll interval explicitly in this case because the default timings are set up for jobs (search point evaluations) that take at least a minute or two to complete.

## MongoTrials is a Persistent Object

If you run the example above a second time,

```python
best = fmin(math.sin, hp.uniform('x', -2, 2), trials=trials, algo=tpe.suggest, max_evals=10)
```

you will see that it returns right away and nothing happens.
That's because the database you are connected to already has enough trials in it; you already computed them when you ran the first experiment.
If you want to do another search, you can change the database name or the `exp_key`.
If you want to extend the search, then you can call fmin with a higher number for `max_evals`.
Alternatively, you can launch other processes that create the MongoTrials specifically to analyze the results that are already in the database. Those other processes do not need to call fmin at all.


---

## <a name="docs-templates-scaleout-spark"></a>File: docs\templates\scaleout\spark.md

# Scaling out search with Apache Spark

With the new class `SparkTrials`, you can tell Hyperopt to distribute a tuning job across an Apache Spark cluster. Initially developed within Databricks, this API has now been contributed to Hyperopt.

Hyperparameter tuning and model selection often involve training hundreds or thousands of models.  `SparkTrials` runs batches of these training tasks in parallel, one on each Spark executor, allowing massive scale-out for tuning.  To use `SparkTrials` with Hyperopt, simply pass the `SparkTrials` object to Hyperopt’s `fmin()` function:

```python
import hyperopt

best_hyperparameters = hyperopt.fmin(
  fn = training_function,
  space = search_space,
  algo = hyperopt.tpe.suggest,
  max_evals = 64,
  trials = hyperopt.SparkTrials())
```

Under the hood, `fmin()` will generate new hyperparameter settings to test and pass them to `SparkTrials`, which runs these tasks asynchronously on a cluster as follows:

- Hyperopt’s primary logic runs on the Spark driver, computing new hyperparameter settings.
- When a worker is ready for a new task, Hyperopt kicks off a single-task Spark job for that hyperparameter setting.
- Within that task, which runs on one Spark executor, user code will be executed to train and evaluate a new ML model.
- When done, the Spark task will return the results, including the loss, to the driver.  

These new results are used by Hyperopt to compute better hyperparameter settings for future tasks.

Since `SparkTrials` fits and evaluates each model on one Spark worker, it is limited to tuning single-machine ML models and workflows, such as scikit-learn or single-machine TensorFlow.  For distributed ML algorithms such as Apache Spark MLlib or Horovod, you can use Hyperopt’s default Trials class.

## SparkTrials API

`SparkTrials` may be configured via 3 arguments, all of which are optional:

**`parallelism`**
The maximum number of trials to evaluate concurrently. Greater parallelism allows scale-out testing of more hyperparameter settings. Defaults to Spark `SparkContext.defaultParallelism`.

* Trade-offs: The `parallelism` parameter can be set in conjunction with the `max_evals` parameter in `fmin()`. Hyperopt will test `max_evals` total settings for your hyperparameters, in batches of size `parallelism`.  If `parallelism = max_evals`, then Hyperopt will do Random Search: it will select all hyperparameter settings to test independently and then evaluate them in parallel.  If `parallelism = 1`, then Hyperopt can make full use of adaptive algorithms like Tree of Parzen Estimators (TPE) which iteratively explore the hyperparameter space: each new hyperparameter setting tested will be chosen based on previous results.  Setting `parallelism` in between `1` and `max_evals` allows you to trade off scalability (getting results faster) and adaptiveness (sometimes getting better models).
* Limits: There is currently a hard cap on parallelism of 128. `SparkTrials` will also check the cluster’s configuration to see how many concurrent tasks Spark will allow; if parallelism exceeds this maximum, `SparkTrials` will reduce parallelism to this maximum.

**`timeout`**
Maximum time in seconds which `fmin()` is allowed to take, defaulting to None.
Timeout provides a budgeting mechanism, allowing a cap on how long tuning can take.  When the timeout is hit, runs are terminated if possible, and `fmin()` exits, returning the current set of results.

**`spark_session`**
`SparkSession` instance for `SparkTrials` to use.  If this is not given, `SparkTrials` will look for an existing `SparkSession`.

The `SparkTrials` API may also be viewed by calling `help(SparkTrials)`.


## Example workflow with SparkTrials

Below, we give an example workflow which tunes a scikit-learn model using SparkTrials.	 This example was adapted from the [scikit-learn doc example](https://scikit-learn.org/stable/auto_examples/linear_model/plot_sparse_logistic_regression_mnist.html) for sparse logistic regression with MNIST.

```python
from sklearn.datasets import fetch_openml
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.utils import check_random_state

from hyperopt import fmin, hp, tpe
from hyperopt import SparkTrials, STATUS_OK

# Load MNIST data, and preprocess it by standarizing features.
X, y = fetch_openml('mnist_784', version=1, return_X_y=True)

random_state = check_random_state(0)
permutation = random_state.permutation(X.shape[0])
X = X[permutation]
y = y[permutation]
X = X.reshape((X.shape[0], -1))

X_train, X_test, y_train, y_test = train_test_split(
    X, y, train_size=5000, test_size=10000)

scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# First, set up the scikit-learn workflow, wrapped within a function.
def train(params):
  """
  This is our main training function which we pass to Hyperopt.
  It takes in hyperparameter settings, fits a model based on those settings,
  evaluates the model, and returns the loss.
  
  :param params: map specifying the hyperparameter settings to test
  :return: loss for the fitted model
  """
  # We will tune 2 hyperparameters:
  #  regularization and the penalty type (L1 vs L2).
  regParam = float(params['regParam'])
  penalty = params['penalty']

  # Turn up tolerance for faster convergence
  clf = LogisticRegression(C=1.0 / regParam,
                           multi_class='multinomial',
                           penalty=penalty, solver='saga', tol=0.1)
  clf.fit(X_train, y_train)
  score = clf.score(X_test, y_test)

  return {'loss': -score, 'status': STATUS_OK}

# Next, define a search space for Hyperopt.
search_space = {
  'penalty': hp.choice('penalty', ['l1', 'l2']),
  'regParam': hp.loguniform('regParam', -10.0, 0),
}

# Select a search algorithm for Hyperopt to use.
algo=tpe.suggest  # Tree of Parzen Estimators, a Bayesian method

# We can run Hyperopt locally (only on the driver machine)
# by calling `fmin` without an explicit `trials` argument.
best_hyperparameters = fmin(
  fn=train,
  space=search_space,
  algo=algo,
  max_evals=32)
best_hyperparameters

# We can distribute tuning across our Spark cluster
# by calling `fmin` with a `SparkTrials` instance.
spark_trials = SparkTrials()
best_hyperparameters = fmin(
  fn=train,
  space=search_space,
  algo=algo,
  trials=spark_trials,
  max_evals=32)
best_hyperparameters
```


---

## <a name="docs-templates-scipy_submission"></a>File: docs\templates\scipy_submission.md

# Hyperopt: A Python library for optimizing the hyperparameters of machine learning algorithms

[SciPy2013 Abstract submission](http://conference.scipy.org/scipy2013/speaking_submission.php)

## Authors

James Bergstra, Dan Yamins, and David D. Cox

## Bios

James Bergstra is an NSERC Banting Fellow at the University of Waterloo's Centre for Theoretical Neuroscience.
His research interests include visual system models and learning algorithms, deep learning, Bayesian optimization, high performance computing, and music information retrieval.
Previously he was a member of Professor David Cox's Computer and Biological Vision Lab in the Rowland Institute for Science at Harvard University.
He completed doctoral studies at the University of Montreal in July 2011 under the direction of Professor Yoshua Bengio with a dissertation on how to incorporate complex cells into deep learning models.
As part of his doctoral work he co-developed Theano, a popular meta-programming system for Python that can target GPUs for high-performance computation.

Dan Yamins is a post-doctoral research fellow in Brain and Cognitive Sciences at the Massachusetts Institute of Technology.  His research interests include computational models of the ventral visual stream, and high-performance computing for neuroscience and computer vision applications.  Previously, he developed python-language software tools for large-scale data analysis and workflow management.  He completed his PhD at Harvard University under the direction of Radhika Nagpal, with a dissertation on computational models of spatially distributed multi-agent systems.

David Cox is an Assistant Professor of Molecular and Cellular Biology and of Computer Science, and is a member of the Center for Brain Science at Harvard University. He completed his Ph.D. in the Department of Brain and Cognitive Sciences at MIT with a specialization in computational neuroscience. Prior to joining MCB/CBS, he was a Junior Fellow at the Rowland Institute at Harvard, a multidisciplinary institute focused on high-risk, high-reward scientific research at the boundaries of traditional fields.

## Talk Summary

Most machine learning algorithms have hyperparameters that have a great impact on end-to-end system performance, and adjusting hyperparameters to optimize end-to-end performance can be a daunting task.
Hyperparameters come in many varieties--continuous-valued ones with and without bounds, discrete ones that are either ordered or not, and conditional ones that do not even always apply
(e.g., the parameters of an optional pre-processing stage)--so
conventional continuous and combinatorial optimization algorithms either do not directly apply, or else operate without leveraging structure in the search space.
Typically, the optimization of hyperparameters is carried out before-hand by  domain experts on unrelated problems, or manually for the problem at hand with the assistance of grid search.
However, when dealing with more than a few hyperparameters (e.g. 5), the standard practice of manual search with grid refinement is so inefficient that even random search has been shown to be competitive with domain experts [1].

There is a strong need for better hyperparameter optimization algorithms (HOAs) for two reasons:

1. HOAs formalize the practice of model evaluation, so that benchmarking experiments can be reproduced at later dates, and by different people.

2. Learning algorithm designers can deliver flexible fully-configurable implementations to non-experts (e.g. deep learning systems), so long as they also provide a corresponding HOA.

Hyperopt provides serial and parallelizable HOAs via a Python library [2, 3].
Fundamental to its design is a protocol for communication between
(a) the description of a hyperparameter search space,
(b) a hyperparameter evaluation function (machine learning system), and
(c) a hyperparameter search algorithm.
This protocol makes it possible to make generic HOAs (such as the bundled "TPE" algorithm) work for a range of specific search problems.
Specific machine learning algorithms (or algorithm families) are implemented as hyperopt _search spaces_ in related projects:
Deep Belief Networks [4],
convolutional vision architectures [5],
and scikit-learn classifiers [6].
My presentation will explain what problem hyperopt solves, how to use it, and how it can deliver accurate models from data alone, without operator intervention.

## Submission References

[1] J. Bergstra and Y. Bengio (2012).  Random Search for Hyper-Parameter Optimization.  Journal of Machine Learning Research 13:281–305.
http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf

[2] J. Bergstra, D. Yamins and D. D. Cox (2013).  Making a Science of Model Search: Hyperparameter Optimization in Hundreds of Dimensions for Vision Architectures.  Proc. 30th International Conference on Machine Learning (ICML-13).
http://jmlr.csail.mit.edu/proceedings/papers/v28/bergstra13.pdf

[3] Hyperopt: http://hyperopt.github.com/hyperopt

[4] ... for Deep Belief Networks: https://github.com/hyperopt/hyperopt-nnet

[5] ... for convolutional vision architectures: https://github.com/hyperopt/hyperopt-convnet

[6] ... for scikit-learn classifiers: https://github.com/hyperopt/hyperopt-sklearn

More information about the presenting author can be found on his academic website: http://www.eng.uwaterloo.ca/~jbergstr/


---

## <a name="docs-templates-setup-installation-notes"></a>File: docs\templates\setup\installation-notes.md

# Installation notes for hyperopt

## MongoDB

Hyperopt requires [mongodb](http://www.mongodb.org) (sometimes "mongo" for short) to perform parallel search. As far as I know, hyperopt is compatible with all versions in the 2.x.x series, which is the current one ([download the latest version here](http://www.mongodb.org/downloads)). It might even be compatible with all versions ever of mongodb, I don't know of any particular version requirements on mongo.

On linux and OSX, once you have downloaded mongodb and unpacked it, simply symlink it into the `bin/` subdirectory of your virtualenv and your installation is complete.

```bash
# from the root of your virtualenv
# (or basically any folder with an active bin/ subdirectory)
(cd bin && { for F in ../mongodb-linux-x86_64-2.2.2/bin/* ; do echo "linking $F" ; ln -s $F ; done } )
```

Verify that hyperopt can use mongod by running either the full unit test suite, or just the mongo file

```bash
# cd to the hyperopt project root
pytest hyperopt/tests/test_mongoexp.py
```

## Spark

We have a little [script](https://github.com/hyperopt/hyperopt/blob/master/download_spark_dependencies.sh) that will
help you download all necessary dependencies.

---

## <a name="docs-templates-setup-running-tests"></a>File: docs\templates\setup\running-tests.md

# Running unit tests

To run the unit tests, run the `run-tests.sh` script.  You will need to set these environment variables:

- `SPARK_HOME`: your local copy of Apache Spark. Look at `.travis.yml` and `download_spark_dependencies.sh` for details on how to download Apache Spark.
- `HYPEROPT_FMIN_SEED`: the random seed. You need to get its value from `.travis.yml`.

For example:

```bash
hyperopt$ HYPEROPT_FMIN_SEED=3 SPARK_HOME=/usr/local/lib/spark-2.4.4-bin-hadoop2.7 ./run_tests.sh
```

To run the unit test for one file, you can add the file name as the parameter, e.g:

```bash
hyperopt$ HYPEROPT_FMIN_SEED=3 SPARK_HOME=/usr/local/lib/spark-2.4.4-bin-hadoop2.7 ./run_tests.sh hyperopt/tests/test_spark.py
```

To run all unit tests except `test_spark.py`, add the `--no-spark` flag, e.g:

```bash
hyperopt$ HYPEROPT_FMIN_SEED=3 ./run_tests.sh --no-spark
```

To run the unit test for one file other than `test_spark.py`, add the file name as the parameter after the `--no-spark` flag, e.g:

```bash
hyperopt$ HYPEROPT_FMIN_SEED=3 ./run_tests.sh --no-spark test_base.py
```


---

## <a name="docs-templates-tutorials-01.basictutorial"></a>File: docs\templates\tutorials\01.BasicTutorial.md

# 01. Basic Tutorial

In this tutorial, you can learn how to:

* Define Search Space
* Optimize Objective Function

This tutorial describes how to optimize Hyperparameters using HyperOpt without having a mathematical understanding of any algorithm implemented in HyperOpt.


```python
# Import HyperOpt Library
from hyperopt import tpe, hp, fmin
```

Declares a purpose function to optimize. In this tutorial, we will optimize a simple function called `objective`, which is a simple quadratic function.

$$ y = (x-3)^2 + 2 $$


```python
objective = lambda x: (x-3)**2 + 2
```

Now, let's visualize this objective function.


```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(-10, 10, 100)
y = objective(x)

fig = plt.figure()
plt.plot(x, y)
plt.show()
```


![png](01.BasicTutorial_files/01.BasicTutorial_6_0.png)


We are trying to optimize the objective function by changing the HyperParameter $x$. That's why we will declare a search space for $x$. The functions related to the search space are implemented in `hyperopt.hp`. The list is as follows.

* `hp.randint(label, upper)` or `hp.randint(label, low, high)`
* `hp.uniform(label, low, high)`
* `hp.loguniform(label, low, high)`    
* `hp.normal(label, mu, sigma)`
* `hp.lognormal(label, mu, sigma)`
* `hp.quniform(label, low, high, q)`
* `hp.qloguniform(label, low, high, q)`
* `hp.qnormal(label, mu, sigma, q)`
* `hp.qlognormal(label, mu, sigma, q)`
* `hp.choice(label, list)`
* `hp.pchoice(label, p_list)` with `p_list` as a list of `(probability, option)` pairs
* `hp.uniformint(label, low, high, q)` or `hp.uniformint(label, low, high)` since `q = 1.0`


We will use the most basic `hp.uniform` in this tutorial.
    


```python
# Define the search space of x between -10 and 10.
space = hp.uniform('x', -10, 10)
```

Now, there's only one last step left. So far, we have defined a function of purpose, and we have defined a search space for $x$. Now we can search through the search space $x$ and find the value of $x$ that can optimize the objective function. HyperOpt performs it using `fmin`.


```python
best = fmin(
    fn=objective, # Objective Function to optimize
    space=space, # Hyperparameter's Search Space
    algo=tpe.suggest, # Optimization algorithm
    max_evals=1000 # Number of optimization attempts
)
print(best)
```

    100%|██████████| 1000/1000 [00:04<00:00, 228.56trial/s, best loss: 2.000001036046408]
    {'x': 3.0010178636491283}


The optimal $x$ value found by HyperOpt is approximately 3.0. This is very close to a solution of $y=(x-3)^2+2$.


---

## <a name="docs-templates-tutorials-02.multipleparametertutorial"></a>File: docs\templates\tutorials\02.MultipleParameterTutorial.md

# 02. MultipleParameterTutorial

In this tutorial, you will learn how to:

* Optimize the Objective Function with Multiple HyperParameters
* Define different types of Search Space


```python
# Import HyperOpt Library
from hyperopt import tpe, hp, fmin
import numpy as np
```

Declares a objective function to optimize. Unlike last time, we will optimize the function with two Hyperparameters, $x$ and $y$.

$$ z = sin\sqrt{x^2 + y^2} $$


```python
def objective(params):
    x, y = params['x'], params['y']
    return np.sin(np.sqrt(x**2 + y**2))
```

Just like last time, let's try visualizing it. But unlike last time, there are two Hyperparameters, so we need to visualize them in 3D space.


```python
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D

x = np.linspace(-6, 6, 30)
y = np.linspace(-6, 6, 30)
x, y = np.meshgrid(x, y)

z = objective({'x': x, 'y': y})

fig = plt.figure()
ax = plt.axes(projection='3d')
ax.plot_surface(x, y, z, cmap=cm.coolwarm)

ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')

plt.show()
```


![png](02.MultipleParameterTutorial_files/02.MultipleParameterTutorial_5_0.png)


Likewise, let's define the search space. However, this time, you need to define two search spaces($x, y$), so you put each of them in the `dict()`.


```python
space = {
    'x': hp.uniform('x', -6, 6),
    'y': hp.uniform('y', -6, 6)
}
```

Perfect! Now you can do exactly what you did at BasicTutorial!


```python
best = fmin(
    fn=objective, # Objective Function to optimize
    space=space, # Hyperparameter's Search Space
    algo=tpe.suggest, # Optimization algorithm (representative TPE)
    max_evals=1000 # Number of optimization attempts
)
print(best)
```

    100%|██████████| 1000/1000 [00:07<00:00, 127.90trial/s, best loss: -0.9999976342002768]
    {'x': 4.278018218372159, 'y': 1.97095757186186}


## Define different types of Search Space

* `hp.randint(label, upper)` searches the integer in the [0, upper) interval.
* `hp.choice(label, list)` searches for elements in the list.


```python
def f(params):
    x1, x2 = params['x1'], params['x2']
    if x1 == 'james':
        return -1 * x2
    if x1 == 'max':
        return 2 * x2
    if x1 == 'wansoo':
        return -3 * x2

search_space = {
    'x1': hp.choice('x1', ['james', 'max', 'wansoo']),
    'x2': hp.randint('x2', -5, 5)
}

best = fmin(
    fn=f,
    space=search_space,
    algo=tpe.suggest,
    max_evals=100
)

print(best)
```

    100%|██████████| 100/100 [00:00<00:00, 396.61trial/s, best loss: -12.0]
    {'x1': 2, 'x2': 4}

