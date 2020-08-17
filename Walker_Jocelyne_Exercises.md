Visual story telling part 1: green buildings
--------------------------------------------

Predicting expected profitability is essential for investments,
especially large capital projects like a $100M 250,000 sq.ft. mixed-use
building. From the initial analysis, it appears that the 5% expected
premium for green certification is a good investment, as costs may be
recovered in less than 8 years.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/1.1-1.png)

We see from the graph above that the medians (black lines on the
boxplots) and means (represented by the red stars) of green and
non-green building revenue per square foot are different. The not green
buildings have a mean of $24.50, while the green buildings have a mean
of $27.00. This shows that from both means and medians, there is
additional revenue on average for green buildings.

However, there are a number of issues with the original staff member’s
analysis:

-   The staff member used the median in his analysis rather than the
    mean. While he is correct that the median is more robust to
    outliers, it is important to consider outliers in this analysis.
    Thus, the mean is a better measure of spread for comparing the two
    groups, and the **treatment of outliers** should be considered in
    more detail.

-   Green and non-green buildings are **inherently different.** Green
    buildings are more likely to be newer, bigger, Class A developments.
    Variables like age, size, and class of the building are confounding
    variables that impact both whether or not a building is green and
    its revenue per square foot. Because these building groups have
    confounding variables, we cannot simply compare their means.

-   We must consider the **time value of money.** Performing an NPV
    analysis and talking to the construction company about reducing the
    cost of the project will supplement our analysis.

First, instead of dropping all buildings with leasing rates less than
10%, we chose to drop only those with leasing rates less than 1% to
consider more of the outliers.

**What variables appear to be confounding?**

First, I built a correlation matrix to identify how each of the building
predictors are related.

-   We see that a building’s Green Rating is slightly positively
    correlated with Class A, more desirable buildings and negatively
    correlated with Age.

-   Building Revenue per Square Foot, Rent, and Cluster Rent are all
    positively correlated. This makes sense as buildings with a higher
    rent per square foot should have a higher revenue per square foot.
    Cluster rent and rent tend to be positively correlated as well, as
    buildings will have similar rents to other buildings in their local
    markets.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/1.1.0-1.png)

I then compared boxplots of each of the predictor variables for Not
Green versus Green buildings, and the charts for the predictors that may
be confounding variables are shown below.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/1.1.a-1.png)

-   Green Buildings tend to be **larger in size** and **younger in age**
    on average.

-   Green Buildings are also **less likely to have been renovated** and
    **have more amenities.**

-   Green Buildings are more likely to be **Class A** buildings, the
    highest quality properties, while Not Green Buildings are more
    likely to be the lower **Class B** buildings.

-   Green Buildings more often situated in warmer climates that have
    **higher cooling degree days** and fewer **heating degree days.**

**So, is the increased Revenue Per Square Foot in Green buildings
*really* due to their Green Rating?** Or, instead, is it because they
are newer, larger, more desirable buildings that just happen to be green
certified? To answer this question, we must “adjust” for these
confounding variables in order to compare buildings where the *only
difference* is a green certification.

Theoretically, to answer this developer’s question about the economic
impact of “going green,” we’d like to have two identical 250,000 square
foot buildings on East Cesar Chavez: one Green Rated and one Not Green
Rated. Only then could we assess whether the 5% expected premium for
green certification would hold true. However, we can’t do this in East
Austin. Instead, we *can* **match our data in order to balance the green
and non-green building groups.**

The goal of our analysis is to adjust for those 8 confounding variables
described above like age, size, and class. The matching process entails:

-   For each green building, finding a non-green building that is very
    similar in confounding variables. For example, they have similar
    ages, stories, and amenities.

-   Pairing the data up into a new dataset so each green building has a
    “matched” non-green building.

The output below shows the summary of the matched and unmatched
datasets.

    ## 
    ## Call:
    ## matchit(formula = green_rating ~ size + age + class_a + class_b + 
    ##     renovated + amenities + cd_total_07 + hd_total07, data = green_cl)
    ## 
    ## Summary of balance for all data:
    ##             Means Treated Means Control  SD Control  Mean Diff     eQQ Med
    ## distance           0.1742        0.0803      0.0825     0.0939      0.0993
    ## size          326286.8171   230299.3257 299437.1103 95987.4914 107554.5000
    ## age               23.9336       49.1663     32.3225   -25.2327     14.0000
    ## class_a            0.7965        0.3680      0.4823     0.4285      0.0000
    ## class_b            0.1932        0.4854      0.4998    -0.2922      0.0000
    ## renovated          0.2139        0.3971      0.4893    -0.1832      0.0000
    ## amenities          0.7271        0.5173      0.4997     0.2098      0.0000
    ## cd_total_07     1427.6844     1202.1522   1089.0898   225.5321    134.0000
    ## hd_total07      2768.6932     3479.6950   1970.0977  -711.0017    668.0000
    ##                eQQ Mean      eQQ Max
    ## distance         0.0939       0.1399
    ## size        102480.9749 2059803.0000
    ## age             25.2861      63.0000
    ## class_a          0.4277       1.0000
    ## class_b          0.2920       1.0000
    ## renovated        0.1829       1.0000
    ## amenities        0.2094       1.0000
    ## cd_total_07    243.5118    1482.0000
    ## hd_total07     711.9159    2044.0000
    ## 
    ## 
    ## Summary of balance for matched data:
    ##             Means Treated Means Control  SD Control  Mean Diff    eQQ Med
    ## distance           0.1742        0.1741      0.0776     0.0000     0.0001
    ## size          326286.8171   291389.9661 315069.1079 34896.8510 49234.0000
    ## age               23.9336       23.2021     14.7229     0.7316     0.0000
    ## class_a            0.7965        0.7802      0.4144     0.0162     0.0000
    ## class_b            0.1932        0.1991      0.3996    -0.0059     0.0000
    ## renovated          0.2139        0.1917      0.3940     0.0221     0.0000
    ## amenities          0.7271        0.6917      0.4621     0.0354     0.0000
    ## cd_total_07     1427.6844     1584.8245   1323.9999  -157.1401     0.0000
    ## hd_total07      2768.6932     2623.4366   1754.1965   145.2566     0.0000
    ##               eQQ Mean     eQQ Max
    ## distance        0.0002      0.0026
    ## size        47369.8215 828758.0000
    ## age             0.8968     20.0000
    ## class_a         0.0162      1.0000
    ## class_b         0.0059      1.0000
    ## renovated       0.0221      1.0000
    ## amenities       0.0354      1.0000
    ## cd_total_07   158.9985   1696.0000
    ## hd_total07    172.6106   1183.0000
    ## 
    ## Percent Balance Improvement:
    ##             Mean Diff.  eQQ Med eQQ Mean  eQQ Max
    ## distance       99.9803  99.9367  99.8336  98.1680
    ## size           63.6444  54.2241  53.7770  59.7652
    ## age            97.1007 100.0000  96.4536  68.2540
    ## class_a        96.2136   0.0000  96.2069   0.0000
    ## class_b        97.9807   0.0000  97.9798   0.0000
    ## renovated      87.9244   0.0000  87.9032   0.0000
    ## amenities      83.1271   0.0000  83.0986   0.0000
    ## cd_total_07    30.3247 100.0000  34.7060 -14.4399
    ## hd_total07     79.5701 100.0000  75.7541  42.1233
    ## 
    ## Sample sizes:
    ##           Control Treated
    ## All          6976     678
    ## Matched       678     678
    ## Unmatched    6298       0
    ## Discarded       0       0

The graphs below represent the compared boxplots of the data *before*
matching and the data *after* matching. Each left graph is before
matching, and each graph on the right shows the difference in the
matched data. There is now much smaller difference in confounding
variables.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/1.1.c-1.png)![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/1.1.c-2.png)![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/1.1.c-3.png)

Balancing the data allows me to compare like with like. This eliminates
the effect of the confounding variables because now the confounders are
equal between the two different groups. It is now reasonable to compare
the Revenue per Square Foot of Green and Non-Green buildings and make a
conclusion about the impact of green rating because green rating is the
only thing that is different between the two groups.

This is a better approach because we know that green rating is the only
variable that is impacting differences in revenue per square foot. In
the first situation, the differences in groups could have been caused by
a number of confounding variables.

**Now that we’ve adjusted for confounding variables, the differences in
mean revenue in the matched groups are smaller.** The mean revenue per
square foot of the **non-green matched buildings is $26.24**, while the
**mean of the green buildings is $26.97**, resulting in a difference of
**$0.73** per square foot. This is much smaller than original estimate
of $2.60 increased revenue per square foot. This would translate into an
additional 250,000 \* 0.73 = $182,500 of extra revenue per year. If the
expected baseline construction costs are $100 million, with an expected
5% premium for green certification, that means we should expect to spend
an extra $5 million on the green building. It would take
5,000,000/182,500 = 27.4 years to recuperate the costs, without
accounting for the time value of money. **This does not seem like a good
financial move to build the green building; it is much riskier than the
original estimate.**

**Next, we need to estimate how “stable” our estimate of $0.77 increased
revenue per square foot of a green building is.** We ran a bootstrapped
linear model to understand how the coefficient for **green rating**
changes as we resample with replacement. The histogram of these
coefficients is shown below, and the 95% confidence interval for the
coefficient for green rating is -$1.18 to +$1.91 with a mean of $0.46
per square foot.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/1.b.1-1.png)

From the bootstrapped regression, our estimate for the coefficient of
green rating falls again, and we are not sure that it is greater than 0,
as 0 is included in the confidence interval. This shows that there’s
**no significant difference** in rental revenue between green and
not-green buildings, and the green construction certification is not a
profitable investment.

The upper 95% confidence interval is $1.91 Revenue per Square Foot.
Assuming a lift at the 95% coefficient level, 6% discount rate, and 90%
leasing rate for the next 30 years, the NPV of this investment is
$915,436.

Assuming the mean coefficient of $0.46, 6% discount rate, and 90%
leasing rate for the next 30 years, the **NPV of this investment is
-$3,575,340.** This negative NPV shows that this is not a profitable
investment, and **the $5,000,000 could be better spent on more
development space or amenities that may be more likely to increase
revenue per square foot.**

Visual story telling part 2: flights at ABIA
--------------------------------------------

The most frustrating part of travel is delays. Whether they’re due to a
late arriving plane, a weather delay, or a mechanical issue with the
plane, no traveler likes to wait to board their plane and get to their
destination. Not only do plane delays frustrate travelers, they
represent extreme costs for both the airport and the airlines who rely
on on-time schedules.

So, where were the most common flight delays at ABIA in 2008? What’s the
relationship between the frequency of a flight route and its average
delays? What are the most common causes of delay? Analyzing delay data
will enable ABIA and airlines to better anticipate delays and change
flight paths or schedules to account for frequent delays.

First, it’s important to consider what are the most common flight paths
through Austin. This allows us to scale the delay data by the frequency
of each flight.

### Most Common Flight Routes Passing through Austin-Bergstrom International Airport, 2008

*The width and opaqueness of the lines increase by the frequency of the
flight path. The most frequent routes are represented by the green
color, at about 5000 flights per year or ~15 flights per day. The least
frequent flights are shown in yellow, less than 1000 flights per year,
or less than 3 flights per day.*

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/2.1-1.png)

The most frequent flight routes are to Dallas and Houston. Other notable
frequent routes shown on the map are Phoenix, Denver, Chicago, and
Atlanta.

From intuition, frequent flight routes should have fewer average delays
than less frequently flown routes. Next, we’ll dive into the mapping of
most frequent flight delays on ABIA flights.

### Most Common Flight Departure Delays from ABIA

*The width of the lines increase by the frequency of the flight path.
The routes with the most delays are more opaque and represented by the
green color, while the routes with the fewest delays are represented by
yellow color*

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/2.2-1.png)

This graph is quite good for visualizing aggregate average departure
delays, showing only high green color (average delays &gt;60 minutes)
for routes to Des Moines, Iowa and Nashville, Tennessee. However, this
graph doesn’t tell us much more than that, so we’ll have to filter out
some of the noise.

So what if we just filter the routes with very small average delays?

### Routes with Average Delays &lt;5 minutes from ABIA, 2008

*The width of the lines increase by the frequency of the flight path.
The routes with the most delays are more opaque and represented by the
green color, while the routes with the fewest delays are represented by
yellow color*

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/2.3-1.png)

The rotues with the **shortest average delays** are to every West-Coast
hub (Los Angeles, San Francisco, Seattle), and also include short routes
in the southern half of the United States.

Next, we’ll examine the routes with medium average delays, between 5 and
30 minutes on average.

### Routes with Average Delays Between &gt;5 and &lt; 20 minutes from ABIA, 2008

*The width of the lines increase by the frequency of the flight path.
The routes with the most delays are more opaque and represented by the
green color, while the routes with the fewest delays are represented by
yellow color*

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/2.4-1.png)

Our routes with medium delays expand to our east-coast hubs, that all
tend to have higher average delays than the west-coast hubs. Namely,
flights to Chicago, Illinois, Atlanta, Georgia, and New York City tend
to have higher average delays closer to 20 minutes.

**So what are the routes with the most room for improvement?**

### Routes with Average Delays &gt; 20 minutes from ABIA, 2008

*The width of the lines increase by the frequency of the flight path.
The routes with the most delays are more opaque and represented by the
green color, while the routes with the fewest delays are represented by
yellow color*

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/2.5-1.png)

From this chart, we see two **frequently flown routes** with **highest
room for improvement.** We checked which routes these were by going into
the data table and viewing the two most frequently flown routes with
delays &gt;20 minutes.

-   A Mesa Airlines (American Airlines operated) route from IAD to AUS,
    flown 631 times in 2008, with an average delay of 27 minutes.

-   A Southwest Airlines route from BNA to AUS, flown 795 times in 2008,
    with an average delay of 20 minutes.

The types of delays on these routes are shown below.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/2.6-1.png)

The Southwest Nashville flight is most often delayed due to Late
Aircraft Delays. The Mesa Washington DC flight is most often delayed due
to Carrier Delays. **So, what can these airlines do with this
information?**

-   Southwest Airlines should analyze the aircrafts coming into
    Nashville that are used on this Nashville-Austin flight. It may be
    necessary to have more time scheduled in between the flight paths in
    Nashville because the incoming flight is often delayed. Because the
    Arrival Delay on this flight is still 13 minutes, the planes are not
    “making up” the delay in the air. This speaks to the need to
    **revisit the timing of this scheduled BNA-AUS route.**

-   Mesa/American Airlines should dive into its historical data for this
    route that is consistently having Carrier Delays of nearly 20
    minutes. This IAD-AUS route is also not “making up” its delay in the
    air, as the average arrival delay is also 27 minutes. These delays
    may be caused by a crew that takes longer to load passengers, the
    use of an older aircraft with more mechanical issues, or a slower
    baggage loading or fueling crew in Washington DC. With more
    information about the **breakdown of carrier delays,** Mesa will be
    able to improve its route with the maximum number of delays and
    reduce impact on ABIA delays.

Portfolio Modeling
------------------

We are building three different ETF-based portfolios, each based on a
different investment risk strategy. These three portfolios each are
built on different ideas of tracking market movement. For each
portfolio, we’ll use the last five years of daily data to calculate the
5% value at risk using 20 trading day bootstrap resampling on a $100,000
capital investment. Each of these portfolios is redistributed at the end
of the day to maintain the stated portfolio weights.

### Portfolio 1: “In it for the long run”

Portfolio 1 is built on the idea of tracking the S&P 500 movement. These
ETFs are indicators of the entire market trends. Here’s a breakdown of
the portfolio:

-   **20% VOO** Vanguard 500 Index Fund, built to track the S&P 500

-   **20% VTWO** Vanguard Russell 2000 ETF, the next 2,000 diversified
    stocks after excluding the largest US public companies

-   **20% MGC** Vanguard Mega Cap ETF, diversified large blend domestic
    ETF

-   **20% SPYG** SPDR Portfolio S&P 500 Growth ETF, high growth large
    cap companies

-   **20% VTI** Vanguard Total Stock Market ETF, built to track S&P 500,
    but more mid-cap exposure than VOO

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/3.1-1.png)

From the pairs correlation matrix above, you can see that the
Close-to-Close earnings of each of these ETFs are highly correlated.
Thus, when one goes up, the others also go up. When one goes down, they
all go down. This is evidence that all of these ETFs are tracking market
movement and largely moving in the same direction.

Then, we simulate the 20-day trading period of this portfolio.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/3.1.a-1.png)![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/3.1.a-2.png)

    ##        5% 
    ## -8453.319

From the histograms of the portfolio values and earnings, we see that
negative earnings are a possibility. The 5% value at risk based on a
20-day bootstrapped period is **-$8155.05** for this portfolio.

### Portfolio 2: “Thrive on Volatility”

Portfolio 2 is built on the idea of thriving off market volatility.
These ETFs are designed to thrive in volatile market conditions by
increasing their diversity. Here’s a breakdown of the portfolio:

-   **25% QQQ** Invesco QQQ Trust, tracks the Nasdaq 100 index,
    dominated by big tech names

-   **25% BTAL** AGFiQ U.S. Market Neutral Anti-Beta Fund, profiting off
    a spread between high and low beta stocks, performing well when low
    beta stocks are in favor in stormy market conditions

-   **25% SDY** SPDR S&P Dividend ETF, focusing on dividend growth
    stocks. Dividends represent a safer return as firms will reduce
    buybacks before cutting dividends.

-   **25% XLP** SPDR Consumer Staples Select Sector, tracks consumer
    household stable products like Walmart and Proctor and Gamble that
    will not fall significantly during volatile markets

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/3.2-1.png)

From the pairs plot, we see that these four ETFs are much less strongly
correlated than the ETFs in Portfolio 1. Thus, I expect this portfolio
to be “safer” and have a higher Value at Risk.

Then, we simulate the 20-day trading period of this portfolio.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/3.2.a-1.png)![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/3.2.a-2.png)

    ##        5% 
    ## -5012.301

From the histograms of the portfolio values and earnings, we see that
negative earnings are a possibility. The 5% value at risk based on a
20-day bootstrapped period is **-$5,047.68** for this portfolio. This is
a much “safer” portfolio compared to the portfolio that simply tracks
the S&P 500 above.

### Portfolio 3: “Safety and Risk-Aversion with Bonds”

While Portfolio 2 was built on the idea of thriving off market
volatility, Portfolio 3 attempts to avoid market volatility altogether.
These “safe” ETFs are designed to withstand market volatility, but have
a smaller potential return. Here’s a breakdown of the portfolio:

-   **33% FBND** Fidelity Total Bond ETF, tracks Barclays US Universal
    Bond Index with diversified sector allocation

-   **33% BLV** Vanguard Long-Term Bond, tracks US government and
    corporate bonds that have maturities of greater than 10 years

-   **33% BSV** Vanguard Short-Term Bond, built 71% off AAA-rated bonds
    and 13% in bonds rated BBB.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/3.3-1.png)

From the pairs correlation matrix, we see that these Bond ETFs are less
correlated than the ETFs in Portfolio 1 and Portfolio 2.

Then, we simulate the 20-day trading period of this portfolio.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/3.3.a-1.png)![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/3.3.a-2.png)

    ##        5% 
    ## -2539.063

From the histograms of the portfolio values and earnings, we see that
negative earnings are **still** a possibility. The 5% value at risk
based on a 20-day bootstrapped period is **-$2,553.16** for this
portfolio. This is a much “safer” portfolio compared to **both**
Portfolio 1 and Portfolio 2.

### In summary

-   Portfolio 1 is best suited for longer-term investment. (In fact,
    some of these ETFs are in our team’s own IRA portfolios.) The 5% VaR
    is the lowest at **-$8155.05**

-   Portfolio 2 is a better short-term risk fund. Instead of simply
    tracking the market, it attempts to hedge against market volatility
    in other ways. These strategies include focusing on high-value tech
    firms, dividend funds, consumer staples, and low beta stocks. It
    increases VaR to **-$5,047.68**.

-   Portfolio 3 is the best short-term risk fund as it increases VaR
    further to **-$2,553.16**. Because it is based on more stable bond
    indexes, this means that the Value at Risk is higher than the
    previous two portfolios.

For those most risk-averse investors over a 20-day trading period,
Portfolio 3 is the safest choice. Over a longer trading period,
investors may be more likely to choose the portfolios that track the S&P
500 or thrive off market volatility in the long run.

Market segmentation
-------------------

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/4.1-1.png)

Author attribution
------------------

In order to predict the author of an article on the basis of the
article’s textual content, we had to first build a training model to
give a baseline dictionary to predict “new” testing articles.

First, we read in the 50 training articles for each of the 50 different
authors.

    ## <<DocumentTermMatrix (documents: 2500, terms: 32669)>>
    ## Non-/sparse entries: 619802/81052698
    ## Sparsity           : 99%
    ## Maximal term length: 40
    ## Weighting          : term frequency (tf)

    ## <<DocumentTermMatrix (documents: 2500, terms: 32570)>>
    ## Non-/sparse entries: 537861/80887139
    ## Sparsity           : 99%
    ## Maximal term length: 40
    ## Weighting          : term frequency (tf)

    ## <<DocumentTermMatrix (documents: 2500, terms: 3393)>>
    ## Non-/sparse entries: 422971/8059529
    ## Sparsity           : 95%
    ## Maximal term length: 40
    ## Weighting          : term frequency (tf)

After reading in the data, we pre-processed the text in the articles.

-   Converting all text to lowercase

-   Remove numbers

-   Remove punctuation

-   Remove excess white space

After these four steps, we’re down to **2500 documents** with **32,669
terms.**

-   Remove stop and filler words, based on the “basic English” stop
    words

After removing filler words, we’re down to **32,570 terms.**

-   Removed words that have count 0 in &gt; 99% of documents

Thus cuts the long tail significantly to only **3393 terms.**

-   Finally, we converted the raw counts of words in each document to
    TF-IDF weights.

**Then, we replicated the same process to read in the 50 testing
articles for the authors. There are 3448 terms in the testing data,
compared to only 3393 terms in the training data.**

    ## <<DocumentTermMatrix (documents: 2500, terms: 33472)>>
    ## Non-/sparse entries: 628611/83051389
    ## Sparsity           : 99%
    ## Maximal term length: 45
    ## Weighting          : term frequency (tf)

    ## <<DocumentTermMatrix (documents: 2500, terms: 33373)>>
    ## Non-/sparse entries: 545286/82887214
    ## Sparsity           : 99%
    ## Maximal term length: 45
    ## Weighting          : term frequency (tf)

    ## <<DocumentTermMatrix (documents: 2500, terms: 3448)>>
    ## Non-/sparse entries: 428509/8191491
    ## Sparsity           : 95%
    ## Maximal term length: 39
    ## Weighting          : term frequency (tf)

**Now, we have to ensure the words in the training set are identical to
the words in the testing set. We’ve chosen to ignore words that are in
the testing set and not found in the training set. This removes the 55
“new” terms from the training data, less than 2% of the training terms.
Now, both the training and testing groups have 3393 terms.**

    ## <<DocumentTermMatrix (documents: 2500, terms: 3393)>>
    ## Non-/sparse entries: 379314/8103186
    ## Sparsity           : 96%
    ## Maximal term length: 40
    ## Weighting          : term frequency - inverse document frequency (normalized) (tf-idf)

### At this point, we have a training set with 3393 predictors. In order to simplify our predictors, we perform a Principal Component Analysis (PCA) to reduce the number of predictors.

First, this requires us to eliminate columns that have 0 entries. This
reduces us to eliminate columns where the term is not found in any data
in the test or train set. Then, we ensure the term lists are identical
by using only the intersecting columns of the train and test data,
leaving us with 8,317,500 elements in both the train and test matrices.

Once the data is in the same format, we use PCA analysis to choose the
number of principal components.

![](Walker_Jocelyne_Exercises_files/figure-markdown_strict/5.4.1-1.png)

We’ve chosen to stop at 1000 principal components that explain ~80% of
the variance. After these components were chosen, the data cleaning and
pre-processing is complete and we are ready to run models to predict
authors.

### KNN

    ## [1] 815

    ## [1] 0.326

### Random Forest

    ## [1] 0.7192

### Naive Bayes

    ## [1] 0.3144

### Logistic Regression

Association rule mining
-----------------------
