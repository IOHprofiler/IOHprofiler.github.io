---
layout: page
title: Data Format
parent: IOHanalyzer
permalink: /IOHanalyzer/data/
--- 

Specific formats are required to load your benchmark data to **IOHanalyzer**. If your data sets are generated in the format of

* **COCO/BBOB** data format as regulated in [https://hal.inria.fr/inria-00362649](https://hal.inria.fr/inria-00362649),
* **Nevergrad** data format (explained in [https://github.com/facebookresearch/nevergrad](https://github.com/facebookresearch/nevergrad)), or
* **IOHprofiler** data format, which is motivated and modified from **COCO** data format,

then you could skip this section. Needless to say, **you are encouraged to convert your own benchmark data to the format regulated here!**

## File Structure
In general, when benchmarking IOHs on several test functions/problems and dimensions, the raw data files are grouped by the test function and stored in sub-folders (the same with the **COCO/BBOB** data format). For example, one example file structure is outlined as follows:
![](/assets/fig/data.jpg)

Generally, in the data folder (<tt>./</tt> here), the following files are mandatory for **IOHanalyzer**:

* [_Meta-data_](#meta-data) (e.g., <tt>IOHprofiler_f1_i1.info</tt>) summarizes the algorithmic performance for each problem instance, with naming the following naming convention:

<p style="text-align:center"><tt>IOHprofiler_f[function ID]_i[instance number].info</tt></p>

* [_Raw-data_](#raw-data) (e.g., <tt>IOHprofiler_f1_DIM100_i1.dat</tt>) are CSV-like files that contain performance information indexed by the running time. Raw-data files are named in the similar manner as with the meta-data:

<p style="text-align:center"><tt>IOHprofiler_f[function ID]_DIM_[dimension]_i[instance number].[t|i|c]?dat</tt></p>
Note that, using **IOHexperimenter**, you could produce four types of raw data files: <tt>*.dat</tt>, <tt>*.idat</tt>, <tt>*.tdat</tt>, and <tt>*.cdat</tt>, which share exactly the same format and only differ in the [logging events](#logging-events) at which data are recorded.

## <a name="meta-data"></a>Meta-data

 The meta data are implemented in a format that is very similar to that in the **COCO/BBOB** data sets. Note that one meta-data file can consist of several dimensions. Please see the detail below. An example is provided as follows:

```{bash}
suite = 'PBO', funcId = 10, DIM = 100, algId = '(1+1) fGA'
%
data_f10/IOHprofiler_f10_DIM625.dat, 1:1953125|5.59000e+02,
1:1953125|5.59000e+02, 1:1953125|5.59000e+02, 1:1953125|5.54000e+02,
1:1953125|5.59000e+02, 1:1953125|5.64000e+02, 1:1953125|5.54000e+02,
1:1953125|5.59000e+02, 1:1953125|5.49000e+02, 1:1953125|5.54000e+02,
1:1953125|5.49000e+02
suite = 'PBO', funcId = 10, DIM = 625, algId = '(1+1) fGA'
%
data_f10/IOHprofiler_f10_DIM625.dat, 1:1953125|5.59000e+02,
1:1953125|5.59000e+02, 1:1953125|5.59000e+02, 1:1953125|5.54000e+02,
1:1953125|5.59000e+02, 1:1953125|5.64000e+02, 1:1953125|5.54000e+02,
1:1953125|5.59000e+02, 1:1953125|5.49000e+02, 1:1953125|5.54000e+02,
1:1953125|5.49000e+02
...
```

A three-line structure is used for each dimension:

* **The first line** stores some meta-information of the experiment as (key, value) pairs. Note that such pairs are separated by commas. Three keys, <tt>funcId</tt>, <tt>DIM</tt>, and <tt>algId</tt> are **case-sensitive** and **mandatory**.
* **The second line** always starts with a single <tt>%</tt>, indicating what follows this symbol should be the general comments from the user on this experiment. _By default, it is left empty_.
* **The third line** starts with the relative path to the actual data file, followed by the meta-information obtained on each instance, with the following format: 
<p style="text-align:center">$$\underbrace{1}_{\text{instance number}}:\underbrace{1953125}_{\text{running time}}|\;\underbrace{5.59000\text{e+02}}_{\text{best-so-far f(x)}}$$</p>

## <a name="raw-data"></a>Raw-data

The format of raw data is illustrated by the example below (with dummy data records):

```{bash}
"function evaluation"  "current f(x)" "best-so-far f(x)"  "current af(x)+b" "best af(x)+b" "parameter name"  ...
1  +2.95000e+02  +2.95000e+02  +2.95000e+02  +2.95000e+02  0.000000  ...
2  +2.96000e+02  +2.96000e+02  +2.96000e+02  +2.96000e+02  0.001600  ...
4  +3.07000e+02  +3.07000e+02  +3.07000e+02  +3.07000e+02  0.219200  ...
9  +3.11000e+02  +3.11000e+02  +3.11000e+02  +3.11000e+02  0.006400  ...
12  +3.12000e+02  +3.12000e+02  +3.12000e+02  +3.12000e+02  0.001600  ...
16  +3.16000e+02  +3.16000e+02  +3.16000e+02  +3.16000e+02  0.006400  ...
20  +3.17000e+02  +3.17000e+02  +3.17000e+02  +3.17000e+02  0.001600  ...
23  +3.28000e+02  +3.28000e+02  +3.28000e+02  +3.28000e+02  0.027200  ...
27  +3.39000e+02  +3.39000e+02  +3.39000e+02  +3.39000e+02  0.059200  ...
"function evaluation"  "current f(x)" "best-so-far f(x)"  "current af(x)+b" "best af(x)+b" "parameter name"  ...
1   +3.20000e+02  +3.20000e+02  +3.20000e+02  +3.20000e+02  1.000000  ...
24  +3.44000e+02  +3.44000e+02  +3.44000e+02  +3.44000e+02  2.000000  ...
60  +3.64000e+02  +3.64000e+02  +3.64000e+02  +3.64000e+02  3.000000  ...
"function evaluation"  "current f(x)" "best-so-far f(x)"  "current af(x)+b" "best af(x)+b" "parameter name"  ...
...  ... ... ... ...  ...  ...
```

Note that, the columns and header of this format is regulated as follows:

1. **[mandatory]** each _separation line_ (the line that starts with <tt>"function evaluation"</tt>) serves as a separator among different independent runs of the same algorithm. Therefore, it is clear that the data block between two separation lines corresponds to a single run a triplet of (dimension, function, instance). The _double quotation_ (<tt>"</tt>) in the separation line shall always be kept, and it cannot be replaced with single quotation (<tt>'</tt>).
2. **[mandatory]** <tt>"function evaluation"</tt> the current number of function evaluations. In the target-based tracking file (<tt>*.dat</tt>), each block of records (as divided by the separation line) **must** end with the last function evaluation. This is critical for calculating the correct _expected running time_ (ERT) value: please see the [logging events](#logging-events) section for the detail.
3. **[mandatory]** <tt>"best-so-far f(x)"</tt> keeps track of the best function value observed since the beginning of one run.
4. **[optional]** <tt>"current f(x)"</tt> stands for the function value observed when the corresponding number of function evaluations is consumed.
5. **[optional]** The value stored under <tt>"current af(x)+b"</tt> and <tt>"best af(x)+b"</tt>, are so-called _transformed_ function values obtained on each function instances that are generated by translating the original function in its domain and co-domain.
6. **[optional]** In addition, a parameter value (named <tt>"parameter"</tt>) is also tracked in this example, and recording more parameter value is also facilitated. In case the quotation symbol is needed in the parameter name, please use the single quotation (<tt>'</tt>).
7. Each data line should contain a complete record. Incomplete data records will be dropped when loading the data into **IOHanalyzer**.
8. _A single space or tab_ can be used (only one of them should be used consistently in a single data file), to separate the columns.

### Two-Column Format

The raw data file comes in its simplest form, if we only keep the mandatory items from the list above:

```{bash}
"function evaluation" "best-so-far f(x)"
1  +2.95000e+02
2  +2.96000e+02
4  +3.07000e+02  
23  +3.28000e+02
27  +3.39000e+02
"function evaluation" "best-so-far f(x)"  
1   +3.20000e+02
...  ...
```

### <a name="logging-events"></a>Logging Events
It is important to decide on which occasions data of the running algorithm should be recorded. On the one hand, it is straightforward to register the information for every function evaluation. However, this might lead to a huge amount of data to store, where some degree of data redundancy would occur since an iterative optimization heuristic would not make progress on every function evaluation. On the other hand, it is also possible to bookkeep the information of the algorithm once in a while, when an "interesting" event happens during the optimization process, e.g., the best function value found so far is improved, or the algorithm yields a numerical error. To make a trade-off here, four different data files (one mandatory and three optional ones) are provided in **IOHexperimenter**:

* **[Mandatory]** _Target-based tracking_ (<tt>*.dat</tt>): a record is collected if an improvement on the <tt>"best-so-far f(x)"</tt> value is observed. Note that this data file is always generated. In this case, each block of records in the raw data **must** end with the last function evaluation. This is critical for calculating the correct _expected running time_ (ERT) value: since a new line of record would be written if an improvement on the <tt>"best-so-far f(x)"</tt> is seen, the actual running time of an algorithm is never registered if this algorithm fails to reach the final target and terminates before depleting the budget of running time.
* **[Optional]** _Interval tracking_ (<tt>*.idat</tt>): a record is collected every $\tau$-th function evaluation, where the interval $\tau$ can be controlled by the user. By default $\tau$ is set to zero and the interval tracking functionality is turned off.
* **[Optional]** _Time-based tracking_ (<tt>*.tdat</tt>): a record is collected when the user-specified checkpoints on the running time are reached. These checkpoints are evenly spaced in the $\log_{10}$ scale, taking the following two forms: 1) $\{v10^i \mid i = 0, 1,2, \ldots\}$ with $v\in \{1,2,5\}$ by default and 2) $\{10^{i / t} \mid i=0, 1,2,\ldots\}$ with $t=3$ by default.
* **[Optional]** _Complete tracking_ (<tt>*.cdat</tt>): a record is collected for every function evaluation, providing the highest granularity. This functionality is also turned off by default, as it might incur a large volume of data.
