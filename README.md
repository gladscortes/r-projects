## 1. Meet Dr. Ignaz Semmelweis
<p><img style="float: left;margin:5px 20px 5px 1px" src="https://assets.datacamp.com/production/project_49/img/ignaz_semmelweis_1860.jpeg"></p>
<!--
<img style="float: left;margin:5px 20px 5px 1px" src="https://assets.datacamp.com/production/project_49/datasets/ignaz_semmelweis_1860.jpeg">
-->

<p>This is Dr. Ignaz Semmelweis, a Hungarian physician born in 1818 and active at the Vienna General Hospital. If Dr. Semmelweis looks troubled it's probably because he's thinking about <em>childbed fever</em>: A deadly disease affecting women that just have given birth. He is thinking about it because in the early 1840s at the Vienna General Hospital as many as 10% of the women giving birth die from it. He is thinking about it because he knows the cause of childbed fever: It's the contaminated hands of the doctors delivering the babies. And they won't listen to him and <em>wash their hands</em>!</p>
<p>In this notebook, we're going to reanalyze the data that made Semmelweis discover the importance of <em>handwashing</em>. Let's start by looking at the data that made Semmelweis realize that something was wrong with the procedures at Vienna General Hospital.</p>


```R
# Load in the tidyverse package
library(tidyverse)

# Read datasets/yearly_deaths_by_clinic.csv into yearly
yearly <- read_csv('datasets/yearly_deaths_by_clinic.csv')

# Print out yearly
yearly
```



<table>
<caption>A spec_tbl_df: 12 x 4</caption>
<thead>
	<tr><th scope=col>year</th><th scope=col>births</th><th scope=col>deaths</th><th scope=col>clinic</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1841</td><td>3036</td><td>237</td><td>clinic 1</td></tr>
	<tr><td>1842</td><td>3287</td><td>518</td><td>clinic 1</td></tr>
	<tr><td>1843</td><td>3060</td><td>274</td><td>clinic 1</td></tr>
	<tr><td>1844</td><td>3157</td><td>260</td><td>clinic 1</td></tr>
	<tr><td>1845</td><td>3492</td><td>241</td><td>clinic 1</td></tr>
	<tr><td>1846</td><td>4010</td><td>459</td><td>clinic 1</td></tr>
	<tr><td>1841</td><td>2442</td><td> 86</td><td>clinic 2</td></tr>
	<tr><td>1842</td><td>2659</td><td>202</td><td>clinic 2</td></tr>
	<tr><td>1843</td><td>2739</td><td>164</td><td>clinic 2</td></tr>
	<tr><td>1844</td><td>2956</td><td> 68</td><td>clinic 2</td></tr>
	<tr><td>1845</td><td>3241</td><td> 66</td><td>clinic 2</td></tr>
	<tr><td>1846</td><td>3754</td><td>105</td><td>clinic 2</td></tr>
</tbody>
</table>



## 2. The alarming number of deaths
<p>The table above shows the number of women giving birth at the two clinics at the Vienna General Hospital for the years 1841 to 1846. You'll notice that giving birth was very dangerous; an <em>alarming</em> number of women died as the result of childbirth, most of them from childbed fever.</p>
<p>We see this more clearly if we look at the <em>proportion of deaths</em> out of the number of women giving birth. </p>


```R
# Adding a new column to yearly with proportion of deaths per no. births
yearly <- yearly  %>% 
    mutate(proportion_deaths = deaths/births)

# Print out yearly
yearly
```


<table>
<caption>A spec_tbl_df: 12 x 5</caption>
<thead>
	<tr><th scope=col>year</th><th scope=col>births</th><th scope=col>deaths</th><th scope=col>clinic</th><th scope=col>proportion_deaths</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1841</td><td>3036</td><td>237</td><td>clinic 1</td><td>0.07806324</td></tr>
	<tr><td>1842</td><td>3287</td><td>518</td><td>clinic 1</td><td>0.15759051</td></tr>
	<tr><td>1843</td><td>3060</td><td>274</td><td>clinic 1</td><td>0.08954248</td></tr>
	<tr><td>1844</td><td>3157</td><td>260</td><td>clinic 1</td><td>0.08235667</td></tr>
	<tr><td>1845</td><td>3492</td><td>241</td><td>clinic 1</td><td>0.06901489</td></tr>
	<tr><td>1846</td><td>4010</td><td>459</td><td>clinic 1</td><td>0.11446384</td></tr>
	<tr><td>1841</td><td>2442</td><td> 86</td><td>clinic 2</td><td>0.03521704</td></tr>
	<tr><td>1842</td><td>2659</td><td>202</td><td>clinic 2</td><td>0.07596841</td></tr>
	<tr><td>1843</td><td>2739</td><td>164</td><td>clinic 2</td><td>0.05987587</td></tr>
	<tr><td>1844</td><td>2956</td><td> 68</td><td>clinic 2</td><td>0.02300406</td></tr>
	<tr><td>1845</td><td>3241</td><td> 66</td><td>clinic 2</td><td>0.02036409</td></tr>
	<tr><td>1846</td><td>3754</td><td>105</td><td>clinic 2</td><td>0.02797017</td></tr>
</tbody>
</table>



## 3. Death at the clinics
<p>If we now plot the proportion of deaths at both clinic 1 and clinic 2  we'll see a curious pattern…</p>


```R
# Setting the size of plots in this notebook
options(repr.plot.width=7, repr.plot.height=4)

# Plot yearly proportion of deaths at the two clinics
ggplot(yearly, aes(x=year, y=proportion_deaths, color=clinic)) + 
    geom_line() +
    expand_limits(y=0)
```


    
![png](output_5_0.png)
    


## 4. The handwashing begins
<p>Why is the proportion of deaths constantly so much higher in Clinic 1? Semmelweis saw the same pattern and was puzzled and distressed. The only difference between the clinics was that many medical students served at Clinic 1, while mostly midwife students served at Clinic 2. While the midwives only tended to the women giving birth, the medical students also spent time in the autopsy rooms examining corpses. </p>
<p>Semmelweis started to suspect that something on the corpses, spread from the hands of the medical students, caused childbed fever. So in a desperate attempt to stop the high mortality rates, he decreed: <em>Wash your hands!</em> This was an unorthodox and controversial request, nobody in Vienna knew about bacteria at this point in time. </p>
<p>Let's load in monthly data from Clinic 1 to see if the handwashing had any effect.</p>


```R
# Read datasets/monthly_deaths.csv into monthly
monthly <- read_csv('datasets/monthly_deaths.csv')

# Adding a new column with proportion of deaths per no. births
monthly <- monthly %>%
    mutate(proportion_deaths = deaths/births)

# Print out the first rows in monthly
head(monthly)
```



<table>
<caption>A tibble: 6 x 4</caption>
<thead>
	<tr><th scope=col>date</th><th scope=col>births</th><th scope=col>deaths</th><th scope=col>proportion_deaths</th></tr>
	<tr><th scope=col>&lt;date&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1841-01-01</td><td>254</td><td>37</td><td>0.145669291</td></tr>
	<tr><td>1841-02-01</td><td>239</td><td>18</td><td>0.075313808</td></tr>
	<tr><td>1841-03-01</td><td>277</td><td>12</td><td>0.043321300</td></tr>
	<tr><td>1841-04-01</td><td>255</td><td> 4</td><td>0.015686275</td></tr>
	<tr><td>1841-05-01</td><td>255</td><td> 2</td><td>0.007843137</td></tr>
	<tr><td>1841-06-01</td><td>200</td><td>10</td><td>0.050000000</td></tr>
</tbody>
</table>



## 5. The effect of handwashing
<p>With the data loaded we can now look at the proportion of deaths over time. In the plot below we haven't marked where obligatory handwashing started, but it reduced the proportion of deaths to such a degree that you should be able to spot it!</p>


```R
# Plot monthly proportion of deaths
ggplot(monthly, aes(x=date, y=proportion_deaths)) +
    geom_line() +
    expand_limits(y=0) +
    labs(x='Date', y='Proportion of Deaths')
```


    
![png](output_9_0.png)
    


## 6. The effect of handwashing highlighted
<p>Starting from the summer of 1847 the proportion of deaths is drastically reduced and, yes, this was when Semmelweis made handwashing obligatory. </p>
<p>The effect of handwashing is made even more clear if we highlight this in the graph.</p>


```R
# From this date handwashing was made mandatory
handwashing_start = as.Date('1847-06-01')

# Add a TRUE/FALSE column to monthly called handwashing_started
monthly <- monthly %>%
    mutate(handwashing_started = date >= handwashing_start)

# Plot monthly proportion of deaths before and after handwashing
ggplot(monthly, aes(x=date, y=proportion_deaths, color=handwashing_started)) +
    geom_line() +
    expand_limits(y=0) +
    labs(x='Date', y='Proportion of Deaths')
```


    
![png](output_11_0.png)
    


## 7. More handwashing, fewer deaths?
<p>Again, the graph shows that handwashing had a huge effect. How much did it reduce the monthly proportion of deaths on average?</p>


```R
# Calculating the mean proportion of deaths 
# before and after handwashing.

monthly_summary <- monthly %>%
    group_by(handwashing_started) %>%
    summarize(mean_proportion_deaths = mean(proportion_deaths))

# Printing out the summary.
monthly_summary
```

    `summarise()` ungrouping output (override with `.groups` argument)



<table>
<caption>A tibble: 2 x 2</caption>
<thead>
	<tr><th scope=col>handwashing_started</th><th scope=col>mean_proportion_deaths</th></tr>
	<tr><th scope=col>&lt;lgl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>FALSE</td><td>0.10504998</td></tr>
	<tr><td> TRUE</td><td>0.02109338</td></tr>
</tbody>
</table>



## 8. A statistical analysis of Semmelweis handwashing data
<p>It reduced the proportion of deaths by around 8 percentage points! From 10% on average before handwashing to just 2% when handwashing was enforced (which is still a high number by modern standards). 
To get a feeling for the uncertainty around how much handwashing reduces mortalities we could look at a confidence interval (here calculated using a t-test).</p>


```R
# Calculating a 95% Confidence intrerval using t.test 
test_result <- t.test(proportion_deaths ~ handwashing_started, data = monthly)
test_result
```


    
    	Welch Two Sample t-test
    
    data:  proportion_deaths by handwashing_started
    t = 9.6101, df = 92.435, p-value = 1.445e-15
    alternative hypothesis: true difference in means is not equal to 0
    95 percent confidence interval:
     0.06660662 0.10130659
    sample estimates:
    mean in group FALSE  mean in group TRUE 
             0.10504998          0.02109338 



## 9. The fate of Dr. Semmelweis
<p>That the doctors didn't wash their hands increased the proportion of deaths by between 6.7 and 10 percentage points, according to a 95% confidence interval. All in all, it would seem that Semmelweis had solid evidence that handwashing was a simple but highly effective procedure that could save many lives.</p>
<p>The tragedy is that, despite the evidence, Semmelweis' theory — that childbed fever was caused by some "substance" (what we today know as <em>bacteria</em>) from autopsy room corpses — was ridiculed by contemporary scientists. The medical community largely rejected his discovery and in 1849 he was forced to leave the Vienna General Hospital for good.</p>
<p>One reason for this was that statistics and statistical arguments were uncommon in medical science in the 1800s. Semmelweis only published his data as long tables of raw data, but he didn't show any graphs nor confidence intervals. If he would have had access to the analysis we've just put together he might have been more successful in getting the Viennese doctors to wash their hands.</p>


```R
# The data Semmelweis collected points to that:
doctors_should_wash_their_hands <- TRUE
```
