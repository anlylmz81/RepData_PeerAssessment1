---
title: "Reproducible Research: Peer Assessment 1"
author: "Anil Yilmaz Ozkeskin"
output: html_document
---

## Loading and preprocessing the data
<div class="chunk" id="unnamed-chunk-1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># load the raw data</span>
<span class="hl std">file</span><span class="hl kwb">&lt;-</span> <span class="hl str">&quot;./activity.csv&quot;</span>
<span class="hl std">data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">read.csv</span><span class="hl std">(file,</span><span class="hl kwc">header</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">,</span> <span class="hl kwc">colClasses</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;numeric&quot;</span><span class="hl std">,</span><span class="hl str">&quot;character&quot;</span><span class="hl std">,</span><span class="hl str">&quot;numeric&quot;</span><span class="hl std">))</span>

<span class="hl com"># transform the date format</span>
<span class="hl std">data</span><span class="hl opt">$</span><span class="hl std">date</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.Date</span><span class="hl std">(data</span><span class="hl opt">$</span><span class="hl std">date,</span> <span class="hl str">&quot;%Y-%m-%d&quot;</span><span class="hl std">)</span>
</pre></div>
</div></div>

## What is mean total number of steps taken per day?
<div class="chunk" id="unnamed-chunk-2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># Calculate the total number of steps taken per day</span>
<span class="hl std">aggregatedbydate</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">aggregate</span><span class="hl std">(steps</span> <span class="hl opt">~</span> <span class="hl std">date, data, sum,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>

<span class="hl com"># draw histogram</span>
<span class="hl kwd">library</span><span class="hl std">(ggplot2)</span>
<span class="hl kwd">hist</span><span class="hl std">(aggregatedbydate</span><span class="hl opt">$</span><span class="hl std">steps,</span><span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Histogram of total steps per day&quot;</span><span class="hl std">,</span><span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Total steps per day&quot;</span><span class="hl std">,</span><span class="hl kwc">col</span><span class="hl std">=</span><span class="hl str">&quot;red&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-2-1.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl com"># Calculate and report the mean and median of the total number of steps taken per day</span>
<span class="hl std">mean</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mean</span><span class="hl std">(aggregatedbydate</span><span class="hl opt">$</span><span class="hl std">steps)</span>
<span class="hl std">median</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">median</span><span class="hl std">(aggregatedbydate</span><span class="hl opt">$</span><span class="hl std">steps)</span>
<span class="hl kwd">print</span><span class="hl std">(mean)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 10766.19
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(median)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 10765
</pre></div>
</div></div>

## What is the average daily activity pattern?
<div class="chunk" id="unnamed-chunk-3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># Compute the means of steps accross all days for each interval</span>
<span class="hl std">mean_data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">aggregate</span><span class="hl std">(data</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">by</span><span class="hl std">=</span><span class="hl kwd">list</span><span class="hl std">(data</span><span class="hl opt">$</span><span class="hl std">interval),</span> <span class="hl kwc">FUN</span><span class="hl std">=mean,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl kwd">colnames</span><span class="hl std">(mean_data)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;mean_steps&quot;</span><span class="hl std">)</span>

<span class="hl com"># convert interval to integers</span>
<span class="hl std">mean_data</span><span class="hl opt">$</span><span class="hl std">interval</span> <span class="hl kwb">&lt;-</span><span class="hl kwd">as.integer</span><span class="hl std">(mean_data</span><span class="hl opt">$</span><span class="hl std">interval)</span>

<span class="hl com"># make a time-series plot</span>
<span class="hl kwd">plot</span><span class="hl std">(mean_data</span><span class="hl opt">$</span><span class="hl std">interval, mean_data</span><span class="hl opt">$</span><span class="hl std">mean_steps,</span> <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">,</span> <span class="hl kwc">col</span><span class="hl std">=</span><span class="hl str">&quot;blue&quot;</span><span class="hl std">,</span> <span class="hl kwc">lwd</span><span class="hl std">=</span><span class="hl num">2</span><span class="hl std">,</span>
     <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Interval (minutes)&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;Average number of steps&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Average Daily Activity Pattern&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl com"># find which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps</span>
<span class="hl std">max_interval</span> <span class="hl kwb">&lt;-</span> <span class="hl std">mean_data[</span><span class="hl kwd">which.max</span><span class="hl std">(mean_data</span><span class="hl opt">$</span><span class="hl std">mean_steps),]</span>
<span class="hl kwd">print</span><span class="hl std">(max_interval)</span>
</pre></div>
<div class="output"><pre class="knitr r">##     interval mean_steps
## 104      835   206.1698
</pre></div>
</div></div>

835th interval has the maximum average steps which is 206 steps.

## Imputing missing values
<div class="chunk" id="unnamed-chunk-4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># Calculate and report the total number of missing values in the dataset</span>
<span class="hl std">total_na</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">sum</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(data</span><span class="hl opt">$</span><span class="hl std">steps))</span>
<span class="hl kwd">print</span><span class="hl std">(total_na)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 2304
</pre></div>
</div></div>

The total number of missing values is 2304.
<div class="chunk" id="unnamed-chunk-5"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># merge original data with mean_data.</span>
<span class="hl std">newdata</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">merge</span><span class="hl std">(data, mean_data,</span> <span class="hl kwc">by</span> <span class="hl std">=</span> <span class="hl str">'interval'</span><span class="hl std">,</span> <span class="hl kwc">all.y</span> <span class="hl std">= F)</span>

<span class="hl com"># fill NA values with averages rounding up for integers</span>
<span class="hl std">newdata</span><span class="hl opt">$</span><span class="hl std">steps[</span><span class="hl kwd">is.na</span><span class="hl std">(newdata</span><span class="hl opt">$</span><span class="hl std">steps)]</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.integer</span><span class="hl std">(</span>
        <span class="hl kwd">round</span><span class="hl std">(newdata</span><span class="hl opt">$</span><span class="hl std">mean_steps[</span><span class="hl kwd">is.na</span><span class="hl std">(newdata</span><span class="hl opt">$</span><span class="hl std">steps)]))</span>

<span class="hl com"># drop and reorder columns to match original data</span>
<span class="hl std">header</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">names</span><span class="hl std">(data)</span>
<span class="hl std">newdata</span> <span class="hl kwb">&lt;-</span> <span class="hl std">newdata[header]</span>

<span class="hl com"># Calculate the total number of steps taken per day in the new dataset</span>
<span class="hl std">new_aggregatedbydate</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">aggregate</span><span class="hl std">(steps</span> <span class="hl opt">~</span> <span class="hl std">date, newdata, sum,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>

<span class="hl com"># draw histogram for the new dataset</span>
<span class="hl kwd">hist</span><span class="hl std">(new_aggregatedbydate</span><span class="hl opt">$</span><span class="hl std">steps,</span><span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Histogram of total steps per day (NAs replaced with averages)&quot;</span><span class="hl std">,</span><span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Total steps per day&quot;</span><span class="hl std">,</span><span class="hl kwc">col</span><span class="hl std">=</span><span class="hl str">&quot;red&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-5-1.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl com"># Calculate and report the mean and median of the total number of steps taken per day in the new dataset</span>
<span class="hl std">new_mean</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mean</span><span class="hl std">(new_aggregatedbydate</span><span class="hl opt">$</span><span class="hl std">steps)</span>
<span class="hl std">new_median</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">median</span><span class="hl std">(new_aggregatedbydate</span><span class="hl opt">$</span><span class="hl std">steps)</span>
<span class="hl kwd">print</span><span class="hl std">(new_mean)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 10765.64
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(new_median)</span>
</pre></div>
<div class="output"><pre class="knitr r">## [1] 10762
</pre></div>
</div></div>

#### Do these values differ from the estimates from the first part of the assignment?
They do differ slightly.
Original mean was 10766.19, new mean is 10765.64.
Original median was 10765, new median is 10762.

#### What is the impact of imputing missing data on the estimates of the total daily number of steps?
It depends on the strategy that NAs are replaced. In this case, since I replaced them with average steps in the interval, it made a minor impact.

## Are there differences in activity patterns between weekdays and weekends?
<div class="chunk" id="unnamed-chunk-6"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com"># Create a new factor variable in the dataset with two levels - &quot;weekday&quot; and &quot;weekend&quot; indicating whether a given date is a weekday or weekend day.</span>

<span class="hl std">weekend</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">weekdays</span><span class="hl std">(</span><span class="hl kwd">as.Date</span><span class="hl std">(newdata</span><span class="hl opt">$</span><span class="hl std">date))</span> <span class="hl opt">%in%</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;Saturday&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;Sunday&quot;</span><span class="hl std">)</span>
<span class="hl std">newdata</span><span class="hl opt">$</span><span class="hl std">daytype</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;weekday&quot;</span>
<span class="hl std">newdata</span><span class="hl opt">$</span><span class="hl std">daytype[weekend</span> <span class="hl opt">==</span> <span class="hl num">TRUE</span><span class="hl std">]</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;weekend&quot;</span>

<span class="hl com"># convert new character column to factor</span>
<span class="hl std">newdata</span><span class="hl opt">$</span><span class="hl std">daytype</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.factor</span><span class="hl std">(newdata</span><span class="hl opt">$</span><span class="hl std">daytype)</span>

<span class="hl com"># Compute the average number of steps by interval accross all weekday days or weekend days</span>
<span class="hl std">mean_newdata</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">aggregate</span><span class="hl std">(newdata</span><span class="hl opt">$</span><span class="hl std">steps,</span> <span class="hl kwc">by</span><span class="hl std">=</span><span class="hl kwd">list</span><span class="hl std">(newdata</span><span class="hl opt">$</span><span class="hl std">interval,newdata</span><span class="hl opt">$</span><span class="hl std">daytype),</span>
<span class="hl kwc">FUN</span><span class="hl std">=mean)</span>
<span class="hl kwd">colnames</span><span class="hl std">(mean_newdata)</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;interval&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;daytype&quot;</span><span class="hl std">,</span><span class="hl str">&quot;mean_steps&quot;</span><span class="hl std">)</span>

<span class="hl com"># Make a panel plot containing a time series plot (i.e. type = &quot;l&quot;) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis)</span>
<span class="hl kwd">library</span><span class="hl std">(lattice)</span>
<span class="hl kwd">xyplot</span><span class="hl std">(mean_steps</span> <span class="hl opt">~</span> <span class="hl std">interval</span><span class="hl opt">|</span><span class="hl std">daytype, mean_newdata,</span> <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span><span class="hl std">,</span> <span class="hl kwc">col</span><span class="hl std">=</span><span class="hl str">&quot;blue&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">layout</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">1</span><span class="hl std">,</span><span class="hl num">2</span><span class="hl std">),</span>
     <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;5-Minute Interval&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;Average number of steps&quot;</span><span class="hl std">,</span>
     <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;5-Minute Interval versus \nAverage Number of Steps,\nAveraged Across All Weekday Days or Weekend Days&quot;</span><span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/unnamed-chunk-6-1.png" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" class="plot" /></div>
</div></div>





