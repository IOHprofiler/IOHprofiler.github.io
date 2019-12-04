---
layout: page
title: Performance Indicators
permalink: /Background/performance-indicators
---

## Selected Performance Indicators

In order to assess the empirical performance of IOHs, some performance indicators should be adopted in **IOHprofiler**, ranging from simple descriptive statistics to empirical distribution functions of performance measures.

### Descriptive Statistics

Regardless of whether the fixed-target analysis or the fixed-budget one is taken, some basic descriptive statistics are considered to compare the empirical performance from different angles. Such statistics include:

* _sample mean_ of running time/function value given a target/budget value:

$$\bar{T}(v)=\frac{1}{r}\sum_{i=1}^r \min\left\{t_i(A,f,d,v), B\right\},\quad \bar{V}(t)=\frac{1}{r}\sum_{i=1}^r v_i(A,f,d,t).$$

Note that, when a run does not succeed in reaching the target $v$ (namely $t_i = \infty$), the benchmarking budget $B$ is taken to calculate the sample mean, which is also applied hereafter.
* _sample median_ of running time/function value given a target/budget value.
	\item \emph{sample standard deviation} of running time/function value.
	\item \emph{sample quantiles} of running time/function value. When the distribution of $T(A,f,d,v)$ and $V(A,f,d,t)$ shows a high skewness and/or kurtosis, the sample mean can be interpreted wrongly. Therefore \iohana also computes different quantiles of these distributions. By default, the following quantiles, $Q_{2\%}, Q_{5\%},\ldots, Q_{98\%}$ are calculated.
	\item \emph{empirical success rate} shows among all runs, the number of independent runs that an algorithm $A$ reaches the given target $v$ on some function: 
	\begin{equation}\label{eq:success-rate}
		\widehat{p}_s = \sum_{i=1}^r\mathds{1}(t_i(A,f,d,v) < \infty) / r, 
	\end{equation}
	where $\mathds{1}$ is the characteristic function. The empirical success rate estimates the \textbf{probability of success} $p_s(A,f,d,v) \coloneqq \E\mathds{1}(T(A,f,d,v) < \infty)$, that is an intrinsic characteristic of \ioh.
	\end{itemize}

### Expected Running Time

When running the \ioh, usually a benchmark budget $B$ is set and $r$ independent runs of an algorithm are conducted for each test function. Recall that, with a budget value $t$, not all the runs will successfully hit the target. Thus, the sample mean $\bar{T}$ calculated in Eq.~\eqref{eq:sample-mean} \emph{underestimates} the ``true'' mean running time required to reach a target. The true running time can be restored by considering a restarting execution: given a target $v$, an algorithm will be restarted from scratch (independent from the previous restart) repeatedly until $v$ is hit. Consider $R>0$ independent restarts are performed and the running time of each restart $T^{(1)},T^{(2)},\ldots,T^{(R)}$, where $T^{(1)},\ldots,T^{(R-1)}=\infty$ and $T^{(R)} < \infty$. The true running time is: $\widetilde{T} = T^{(R)} + B(R-1),$
where $R$ follows a geometric distribution with expectation $\widehat{p}_s$. It is shown in~\citet{auger2005restart} that the expectation $\E\widetilde{T}$ can be estimated consistently using the so-called \emph{Expected Running Time} (ERT):
\begin{equation}
	\ERT(A, f, d, v) = \frac{\sum_{i=1}^{r} \min\left\{t_i(A,f,d,v), B\right\}}{\sum_{i=1}^{r} \mathds{1}(t_i(A,f,d,v) < \infty)}. \label{eq:ERT}
\end{equation}
Note that ERT can take an infinite value when all the runs are unsuccessful to reach the target value. In \iohana, ERT is an important performance indicator in the fixed-target analysis.

### Cumulative Distribution Functions

For fixed-target and fixed-budget analysis, \iohana provides empirical probability density (mass) functions as well as empirical cumulative distribution functions (ECDFs). Probability density functions are obtained using Kernel Density Estimation (KDE) for $V(A,f,d,t)$, using \verb|density| method from \pkg{stats} package~\citep{stats}. Please see Sec.~\ref{subsec:fixed-target-results} for examples and details. For fixed-target analysis, the ECDF of running time $T$ is defined as $\widehat{F}_T(t)=\sum_{i=1}^r \mathds{1}(t_i \leq t) / r$. In addition, it is sometimes convenient to gain the overview on ECDFs over a range of target values, resulting in an overall performance profile for some algorithm $A$. In \iohana, two levels of ``aggregations'' of ECDFs are implemented:
\begin{itemize}
	\item The aggregation over \emph{target values} is defined in the following sense: for a set of target values $\mathcal{V}$, $r$ number of independent runs on each function, algorithm $A$ on function $f$, the aggregated ECDF is:
	
	\begin{equation}
		\widehat{F}_T(t\; ; \; A,f,d,\mathcal{V}) = \frac{1}{r|\mathcal{V}|}\sum_{v\in \mathcal{V}}\sum_{i=1}^{r} \mathds{1}(t_i(A,f,d,v) \leq t). \label{eq:ECDF-agg-target}
	\end{equation}  
	
	\item Given a set of test functions $\mathcal{F}$, the ECDF can be elevated to the aggregation over \emph{test functions}:
	\begin{equation}
	\widehat{F}_T(t \; ; \; A,\mathcal{F},d,\mathcal{V}) = \frac{1}{r|\mathcal{V}||\mathcal{F}|}\sum_{f\in\mathcal{F}}\sum_{v\in \mathcal{V}}\sum_{i=1}^{r} \mathds{1}(t_i(A,f,d,v) \leq t).  \label{eq:ECDF-agg-fct}
	\end{equation} 	
\end{itemize}
The aggregated ECDFs for function value $V(A,f,d,t)$ can be defined in the similar way, e.g., for aggregations over a set of budget values $\mathcal{T}$: 
\begin{align*}
    \widehat{F}_V(v\; ; \; A,f,d,\mathcal{T}) = \frac{1}{r|\mathcal{T}|}\sum_{t\in \mathcal{T}}\sum_{i=1}^{r} \mathds{1}(v_i(A,f,d,t) \leq v).
\end{align*}
