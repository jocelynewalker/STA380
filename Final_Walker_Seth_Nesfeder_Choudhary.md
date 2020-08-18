Visual story telling part 1: green buildings
--------------------------------------------

Predicting expected profitability is essential for investments,
especially large capital projects like a $100M 250,000 sq.ft. mixed-use
building. From the initial analysis, it appears that the 5% expected
premium for green certification is a good investment, as costs may be
recovered in less than 8 years.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/1.1-1.png)

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

First, we built a correlation matrix to identify how each of the
building predictors are related.

-   We see that a building’s Green Rating is slightly positively
    correlated with Class A, more desirable buildings and negatively
    correlated with Age.

-   Building Revenue per Square Foot, Rent, and Cluster Rent are all
    positively correlated. This makes sense as buildings with a higher
    rent per square foot should have a higher revenue per square foot.
    Cluster rent and rent tend to be positively correlated as well, as
    buildings will have similar rents to other buildings in their local
    markets.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/1.1.0-1.png)

I then compared boxplots of each of the predictor variables for Not
Green versus Green buildings, and the charts for the predictors that may
be confounding variables are shown below.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/1.1.a-1.png)

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

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/1.1.c-1.png)![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/1.1.c-2.png)![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/1.1.c-3.png)

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

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/1.b.1-1.png)

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

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/2.1-1.png)

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

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/2.2-1.png)

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

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/2.3-1.png)

The routes with the **shortest average delays** are to every West-Coast
hub (Los Angeles, San Francisco, Seattle), and also include short routes
in the southern half of the United States.

Next, we’ll examine the routes with medium average delays, between 5 and
30 minutes on average.

### Routes with Average Delays Between &gt;5 and &lt; 20 minutes from ABIA, 2008

*The width of the lines increase by the frequency of the flight path.
The routes with the most delays are more opaque and represented by the
green color, while the routes with the fewest delays are represented by
yellow color*

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/2.4-1.png)

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

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/2.5-1.png)

From this chart, we see two **frequently flown routes** with **highest
room for improvement.** We checked which routes these were by going into
the data table and viewing the two most frequently flown routes with
delays &gt;20 minutes. These are:

-   A Mesa Airlines (American Airlines operated) route from IAD to AUS,
    flown 631 times in 2008, with an average delay of 27 minutes.

-   A Southwest Airlines route from BNA to AUS, flown 795 times in 2008,
    with an average delay of 20 minutes.

The types of delays on these routes are shown below.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/2.6-1.png)

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

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/3.1-1.png)

From the pairs correlation matrix above, you can see that the
Close-to-Close earnings of each of these ETFs are highly correlated.
Thus, when one goes up, the others also go up. When one goes down, they
all go down. This is evidence that all of these ETFs are tracking market
movement and largely moving in the same direction.

Then, we simulate the 20-day trading period of this portfolio.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/3.1.a-1.png)![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/3.1.a-2.png)

    ##        5% 
    ## -8453.319

From the histograms of the portfolio values and earnings, we see that
negative earnings are a possibility. The 5% value at risk based on a
20-day bootstrapped period is **about -$8155.05** for this portfolio.

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

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/3.2-1.png)

From the pairs plot, we see that these four ETFs are much less strongly
correlated than the ETFs in Portfolio 1. Thus, I expect this portfolio
to be “safer” and have a higher Value at Risk.

Then, we simulate the 20-day trading period of this portfolio.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/3.2.a-1.png)![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/3.2.a-2.png)

    ##        5% 
    ## -5012.301

From the histograms of the portfolio values and earnings, we see that
negative earnings are a possibility. The 5% value at risk based on a
20-day bootstrapped period is **about -$5,047.68** for this portfolio.
This is a much “safer” portfolio compared to the portfolio that simply
tracks the S&P 500 above.

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

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/3.3-1.png)

From the pairs correlation matrix, we see that these Bond ETFs are less
correlated than the ETFs in Portfolio 1 and Portfolio 2.

Then, we simulate the 20-day trading period of this portfolio.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/3.3.a-1.png)![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/3.3.a-2.png)

    ##        5% 
    ## -2539.063

From the histograms of the portfolio values and earnings, we see that
negative earnings are **still** a possibility. The 5% value at risk
based on a 20-day bootstrapped period is **about -$2,553.16** for this
portfolio. This is a much “safer” portfolio compared to **both**
Portfolio 1 and Portfolio 2.

### In summary

-   Portfolio 1 is best suited for longer-term investment. (In fact,
    some of these ETFs are in our team’s own IRA portfolios.) The 5% VaR
    is the lowest at **about -$8155.05**

-   Portfolio 2 is a better short-term risk fund. Instead of simply
    tracking the market, it attempts to hedge against market volatility
    in other ways. These strategies include focusing on high-value tech
    firms, dividend funds, consumer staples, and low beta stocks. It
    increases VaR to **about -$5,047.68**.

-   Portfolio 3 is the best short-term risk fund as it increases VaR
    further to **about -$2,553.16**. Because it is based on more stable
    bond indexes, this means that the Value at Risk is higher than the
    previous two portfolios.

For those most risk-averse investors over a 20-day trading period,
Portfolio 3 is the safest choice. Over a longer trading period,
investors may be more likely to choose the portfolios that track the S&P
500 or thrive off market volatility in the long run.

Market segmentation
-------------------

Understanding a company’s market is crucial to being successful and
innovative in an ever-changing society. In this case, the company,
NutrientH20, is attempting to understand their social-media audience a
little bit better.

### Models

-   K-Means with K-means++ Initialization

-   Hierarchical Clustering

In order to run K-means clustering, it is important to try and find an
optimal number of clusters to maximize the value of these models and
increase the interpretability of the model.

<img src="Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/4.2-1.png" style="display: block; margin: auto;" />

Although this elbow plot does not provide us with too much information,
by testing various values for K, we have decided 4 clusters will have
the most benefit for us. Also, the variables spam, chatter, and adult
have been removed from the data as NutrientH20 does not care about
appealing to “spam” or “bot” accounts.

#### **K-Means Clusters**

Let’s first look at the clusters broken into their 4 groups.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/4.4-1.png)

#### **K-Means with K++ Initialization**

Next, let’s dive into the four clusters and which characteristics stand
out the most in each.

<img src="Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/4.6-1.png" width="50%" /><img src="Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/4.6-2.png" width="50%" /><img src="Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/4.6-3.png" width="50%" /><img src="Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/4.6-4.png" width="50%" />

**Our Identified Market Segments**

1.  Health Nutrition, Cooking, Photo Sharing, and Personal Fitness

2.  Sports Fandom, Religion, and Food

3.  Politics, Travel, and News

4.  Photo Sharing, Current Events, Health Nutrition, and College
    Universities

Through K-Means Clustering with K-Means++ Initialization, we have been
able to define some market segments for NutrientH20. Cluster 1 is filled
with people who love to cook and live healthy…and share it through
photos! Cluster 2 appears to be filled with sports fanatics who love
food and might be saying a prayer or two for their teams to win. Cluster
3 is full of people who enjoy talking about politics and the news while
loving to travel as well. Finally, Cluster 4 seems like a younger
demographic - college kids who like to discuss and share pictures about
current events.

#### **Hierarchical Clustering Results**

<img src="Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/4.8-1.png" width="80%" style="display: block; margin: auto;" />

**Our Identified Market Segments**

1.  College Kids

2.  Parents

3.  Current Events

4.  Active / Self-Care

While Hierarchical Clustering does not show specific sizes of
characteristics within each cluster, it still gives us a good idea of
what kind of people each cluster is filled with. For example, Cluster 1,
is populated by sports playing, online gaming, college universities and
TV film among others and seems to be college kids. In contrast, Cluster
2 appears to be more mature people such as parents as news, parenting,
family, and politics are some of the characteristics within the cluster.
Cluster 3 does not give us much as the only characteristic in it is
current events, but that doesn’t necessarily hurt our clustering either.
Finally, Cluster 4 appears to be active people who take good care of
themselves with characteristics ranging from health nutrition to beauty
to outdoors.

Market segmentation is important in order to specifically appeal to
different types of audiences. Performing segmentation often like this
can help immensely as people’s opinions are always changing, especially
on social media!

Additionally, it’s important to check which clusters are responding to
targeted messages. For example, if NutrientH20 were to post something on
twitter targeting the young, college student cluster, we would expect
the users in that cluster to interact with the tweet. This both checks
our understanding of the clustering *and* the changing interests of
NutrientH20’s twitter following.

Author attribution
------------------

In order to predict the author of an article on the basis of the
article’s textual content, we had to first build a training model to
give a baseline dictionary to predict “new” testing articles.

First, we read in the Reuters 50 training articles using a for loop for
each of the 50 different authors. We combined these 2500 training
articles into a single text analysis training corpus.

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
This included:

-   Converting all text to lowercase

-   Removing numbers

-   Removing punctuation

-   Removing excess white space

After these four steps, we’re down to **2500 documents** with **32,669
terms.**

-   Removing stop and filler words, based on the “basic English” stop
    words

After removing filler words, we’re down to **32,570 terms.**

-   Removing words that have count = 0 in &gt; 99% of documents

This cuts the long tail significantly to only **3393 terms.**

-   Finally, we converted the raw counts of words in each document to
    TF-IDF weights.

**Then, we replicated the same process to read in the 50 testing
articles for the 50 authors. There are 3448 terms in the testing data,
compared to only 3393 terms in the training data. We must consider how
to treat these “new” words.**

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
the words in the testing set. We’ve chosen to *ignore new words* that
are in the testing set and not found in the training set. This removes
the 55 “new” terms from the training data, less than 2% of the training
terms. Now, both the training and testing groups have 3393 terms.**

    ## <<DocumentTermMatrix (documents: 2500, terms: 3393)>>
    ## Non-/sparse entries: 379314/8103186
    ## Sparsity           : 96%
    ## Maximal term length: 40
    ## Weighting          : term frequency - inverse document frequency (normalized) (tf-idf)

### At this point, we have a training set with 3393 predictors. In order to simplify our predictors, we perform a Principal Component Analysis (PCA) to reduce the number of predictors.

First, this requires us to eliminate columns that have 0 entries. This
eliminates columns where the term is not found in any data in the test
or train set. Then, we ensure the term lists are identical by using only
the intersecting columns of the train and test data, leaving us with
8,317,500 elements in both the train and test matrices.

Once the data is in the same format, we use PCA analysis to choose the
number of principal components.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/5.4.1-1.png)

We’ve chosen to stop at 1000 principal components that explain ~80% of
the variance. After these components were chosen, the data cleaning and
pre-processing is complete and we are ready to run models to predict
authors.

These were the four models we ran to predict author attribution:

-   KNN

-   Random Forest

-   Naive Bayes

-   Multinomial logistic regression

### KNN

We ran a KNN model with k = 1, as other larger values of k did not
improve the testing or training accuracy.

    ## [1] 0.326

The KNN-model has only 32.6% accuracy on the testing set.

### Random Forest

The random forest model was ran with the maximum number of tries equal
to 31 (this is the square root of 1000, the number of variables).

    ## [1] 0.784

This model shows a high degree of improvement over KNN, with an accuracy
rate of 78.4%.

### Naive Bayes

We then used a Naive Bayes model to predict the testing data from a
training model.

    ## [1] 0.3144

The Naive Bayes model performed worse than the KNN model with only 31.4%
accuracy.

### Multinomial Logistic Regression

Next, we used multinomial logistic regression with only the first 18
Principal Components. The model will not run with greater than 18
principal components.

    ## # weights:  1000 (931 variable)
    ## initial  value 9780.057514 
    ## iter  10 value 3978.610720
    ## iter  20 value 3027.473914
    ## iter  30 value 2744.784042
    ## iter  40 value 2654.854912
    ## iter  50 value 2552.955018
    ## iter  60 value 2433.970187
    ## iter  70 value 2345.527733
    ## iter  80 value 2305.565551
    ## iter  90 value 2285.592449
    ## iter 100 value 2269.532712
    ## final  value 2269.532712 
    ## stopped after 100 iterations

    ## [1] 0.4652

The multinomial logistic regression is better than the KNN and Naive
Bayes models with an accuracy rate of 46.52%. However, the random forest
model still has the highest accuracy.

From the table below, we see the four models accuracy compared.

    ##                models accuracy
    ## 1       Random Forest    78.40
    ## 2 Logistic Regression    46.52
    ## 3                 KNN    32.60
    ## 4         Naive Bayes    31.40

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/5.6-1.png)

**In summary, the random forest model has the highest classification
accuracy of about ~78.4% on the testing dataset.**

Association rule mining
-----------------------

In this question, we’re discovering relationships among commonly
purchased together grocery items.

As it is the analysis of grocery transaction items, we use the arules
package. The data is provided in comma separated values. To refine the
data we follow the below steps:

1.  We convert the txt format to a data frame

2.  We remove the blank values

3.  Convert the shape to two columns, with the second columns containing
    items in the basket of the user

<!-- -->

    ## 'data.frame':    43367 obs. of  2 variables:
    ##  $ User : int  1 1 1 1 2 2 2 3 4 4 ...
    ##  $ value: chr  "citrus fruit" "semi-finished bread" "margarine" "ready soups" ...

    ##       User          value          
    ##  Min.   :    1   Length:43367      
    ##  1st Qu.: 3814   Class :character  
    ##  Median : 7620   Mode  :character  
    ##  Mean   : 7650                     
    ##  3rd Qu.:11482                     
    ##  Max.   :15296

We further plot the bar graph of 20 most frequent items in the overall
dataset. We find out that **whole milk, other vegetables, rolls/buns,
soda, and yogurt** are the top 5 frequently occurring shopping items. We
should find out if these items occur frequently together as well or on
their own which might help us identify some shopping patterns among
customers.

    ##    Length     Class      Mode 
    ##     43367 character character

    ##  [1] "citrus fruit"             "semi-finished bread"     
    ##  [3] "margarine"                "ready soups"             
    ##  [5] "tropical fruit"           "yogurt"                  
    ##  [7] "coffee"                   "whole milk"              
    ##  [9] "pip fruit"                "yogurt"                  
    ## [11] "cream cheese "            "meat spreads"            
    ## [13] "other vegetables"         "whole milk"              
    ## [15] "condensed milk"           "long life bakery product"
    ## [17] "whole milk"               "butter"                  
    ## [19] "yogurt"                   "rice"

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/6.3-1.png)

To apply the apriori method, we first split the dataset into items that
can be plugged into the algorithm. User is turned into a factor for this
approach, and the data set is split into X and Y (between which the rule
parameters are compared)

We begin with observing results for few combinations of support and
confidence. We start with support &gt; 0.01, confidence &gt; 0.1 and
maxlen = 4 (max items per list). As expected, we see that the top
frequency standalone items(whole milk, soda, other vegetables,
roll/buns) are featured in our list. We see 45 combinations that match
the criteria.

We can further tune the parameters to reduce the combinations. We will
generate many more rules if we lower the support and confidence
thresholds.

    ## transactions as itemMatrix in sparse format with
    ##  15296 rows (elements/itemsets/transactions) and
    ##  169 columns (items) and a density of 0.01677625 
    ## 
    ## most frequent items:
    ##       whole milk other vegetables       rolls/buns             soda 
    ##             2513             1903             1809             1715 
    ##           yogurt          (Other) 
    ##             1372            34055 
    ## 
    ## element (itemset/transaction) length distribution:
    ## sizes
    ##    1    2    3    4 
    ## 3485 2630 2102 7079 
    ## 
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   1.000   2.000   3.000   2.835   4.000   4.000 
    ## 
    ## includes extended item information - examples:
    ##             labels
    ## 1 abrasive cleaner
    ## 2 artif. sweetener
    ## 3   baby cosmetics
    ## 
    ## includes extended transaction information - examples:
    ##   transactionID
    ## 1             1
    ## 2             2
    ## 3             3

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##         0.1    0.1    1 none FALSE            TRUE       5    0.01      1
    ##  maxlen target  ext
    ##       4  rules TRUE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 152 
    ## 
    ## set item appearances ...[0 item(s)] done [0.00s].
    ## set transactions ...[169 item(s), 15296 transaction(s)] done [0.00s].
    ## sorting and recoding items ... [71 item(s)] done [0.00s].
    ## creating transaction tree ... done [0.00s].
    ## checking subsets of size 1 2 3 done [0.00s].
    ## writing ... [45 rule(s)] done [0.00s].
    ## creating S4 object  ... done [0.00s].

    ##      lhs                     rhs                support    confidence
    ## [1]  {}                   => {soda}             0.11212082 0.1121208 
    ## [2]  {}                   => {rolls/buns}       0.11826621 0.1182662 
    ## [3]  {}                   => {other vegetables} 0.12441161 0.1244116 
    ## [4]  {}                   => {whole milk}       0.16429132 0.1642913 
    ## [5]  {curd}               => {whole milk}       0.01261768 0.3683206 
    ## [6]  {butter}             => {whole milk}       0.01438285 0.4036697 
    ## [7]  {whipped/sour cream} => {whole milk}       0.01144090 0.2482270 
    ## [8]  {pip fruit}          => {tropical fruit}   0.01268305 0.2607527 
    ## [9]  {tropical fruit}     => {pip fruit}        0.01268305 0.1879845 
    ## [10] {pip fruit}          => {other vegetables} 0.01091789 0.2244624 
    ## [11] {pip fruit}          => {whole milk}       0.01255230 0.2580645 
    ## [12] {pastry}             => {rolls/buns}       0.01019874 0.1782857 
    ## [13] {citrus fruit}       => {tropical fruit}   0.01248692 0.2346437 
    ## [14] {tropical fruit}     => {citrus fruit}     0.01248692 0.1850775 
    ## [15] {citrus fruit}       => {other vegetables} 0.01281381 0.2407862 
    ## [16] {other vegetables}   => {citrus fruit}     0.01281381 0.1029953 
    ## [17] {citrus fruit}       => {whole milk}       0.01281381 0.2407862 
    ## [18] {sausage}            => {rolls/buns}       0.01078713 0.1785714 
    ## [19] {sausage}            => {other vegetables} 0.01261768 0.2088745 
    ## [20] {other vegetables}   => {sausage}          0.01261768 0.1014188 
    ## [21] {sausage}            => {whole milk}       0.01255230 0.2077922 
    ## [22] {bottled water}      => {soda}             0.01464435 0.2060718 
    ## [23] {soda}               => {bottled water}    0.01464435 0.1306122 
    ## [24] {tropical fruit}     => {root vegetables}  0.01098326 0.1627907 
    ## [25] {root vegetables}    => {tropical fruit}   0.01098326 0.1567164 
    ## [26] {tropical fruit}     => {other vegetables} 0.01549425 0.2296512 
    ## [27] {other vegetables}   => {tropical fruit}   0.01549425 0.1245402 
    ## [28] {tropical fruit}     => {whole milk}       0.01830544 0.2713178 
    ## [29] {whole milk}         => {tropical fruit}   0.01830544 0.1114206 
    ## [30] {root vegetables}    => {other vegetables} 0.02536611 0.3619403 
    ## [31] {other vegetables}   => {root vegetables}  0.02536611 0.2038886 
    ## [32] {root vegetables}    => {whole milk}       0.02262029 0.3227612 
    ## [33] {whole milk}         => {root vegetables}  0.02262029 0.1376840 
    ## [34] {yogurt}             => {rolls/buns}       0.01189854 0.1326531 
    ## [35] {rolls/buns}         => {yogurt}           0.01189854 0.1006081 
    ## [36] {yogurt}             => {other vegetables} 0.01588651 0.1771137 
    ## [37] {other vegetables}   => {yogurt}           0.01588651 0.1276931 
    ## [38] {yogurt}             => {whole milk}       0.02425471 0.2704082 
    ## [39] {whole milk}         => {yogurt}           0.02425471 0.1476323 
    ## [40] {soda}               => {rolls/buns}       0.01425209 0.1271137 
    ## [41] {rolls/buns}         => {soda}             0.01425209 0.1205086 
    ## [42] {rolls/buns}         => {whole milk}       0.01830544 0.1547816 
    ## [43] {whole milk}         => {rolls/buns}       0.01830544 0.1114206 
    ## [44] {other vegetables}   => {whole milk}       0.04086036 0.3284288 
    ## [45] {whole milk}         => {other vegetables} 0.04086036 0.2487067 
    ##      coverage   lift     count
    ## [1]  1.00000000 1.000000 1715 
    ## [2]  1.00000000 1.000000 1809 
    ## [3]  1.00000000 1.000000 1903 
    ## [4]  1.00000000 1.000000 2513 
    ## [5]  0.03425732 2.241875  193 
    ## [6]  0.03563023 2.457036  220 
    ## [7]  0.04609048 1.510895  175 
    ## [8]  0.04864017 3.864800  194 
    ## [9]  0.06746862 3.864800  194 
    ## [10] 0.04864017 1.804191  167 
    ## [11] 0.04864017 1.570774  192 
    ## [12] 0.05720450 1.507495  156 
    ## [13] 0.05321653 3.477820  191 
    ## [14] 0.06746862 3.477820  191 
    ## [15] 0.05321653 1.935400  196 
    ## [16] 0.12441161 1.935400  196 
    ## [17] 0.05321653 1.465605  196 
    ## [18] 0.06040795 1.509911  165 
    ## [19] 0.06040795 1.678898  193 
    ## [20] 0.12441161 1.678898  193 
    ## [21] 0.06040795 1.264779  192 
    ## [22] 0.07106433 1.837944  224 
    ## [23] 0.11212082 1.837944  224 
    ## [24] 0.06746862 2.322805  168 
    ## [25] 0.07008368 2.322805  168 
    ## [26] 0.06746862 1.845898  237 
    ## [27] 0.12441161 1.845898  237 
    ## [28] 0.06746862 1.651444  280 
    ## [29] 0.16429132 1.651444  280 
    ## [30] 0.07008368 2.909216  388 
    ## [31] 0.12441161 2.909216  388 
    ## [32] 0.07008368 1.964566  346 
    ## [33] 0.16429132 1.964566  346 
    ## [34] 0.08969665 1.121648  182 
    ## [35] 0.11826621 1.121648  182 
    ## [36] 0.08969665 1.423611  243 
    ## [37] 0.12441161 1.423611  243 
    ## [38] 0.08969665 1.645907  371 
    ## [39] 0.16429132 1.645907  371 
    ## [40] 0.11212082 1.074810  218 
    ## [41] 0.11826621 1.074810  218 
    ## [42] 0.11826621 0.942117  280 
    ## [43] 0.16429132 0.942117  280 
    ## [44] 0.12441161 1.999064  625 
    ## [45] 0.16429132 1.999064  625

Upon increasing the support to 0.015, we see 20 pairs in our output.
Some combinations which show high co-occurrence are root vegetables and
other vegetables and root vegetables and whole milk. Other vegetables
and whole milk also make one such pair.

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##         0.1    0.1    1 none FALSE            TRUE       5   0.015      1
    ##  maxlen target  ext
    ##       4  rules TRUE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 229 
    ## 
    ## set item appearances ...[0 item(s)] done [0.00s].
    ## set transactions ...[169 item(s), 15296 transaction(s)] done [0.00s].
    ## sorting and recoding items ... [56 item(s)] done [0.00s].
    ## creating transaction tree ... done [0.00s].
    ## checking subsets of size 1 2 3 done [0.00s].
    ## writing ... [20 rule(s)] done [0.00s].
    ## creating S4 object  ... done [0.00s].

    ##      lhs                   rhs                support    confidence coverage  
    ## [1]  {}                 => {soda}             0.11212082 0.1121208  1.00000000
    ## [2]  {}                 => {rolls/buns}       0.11826621 0.1182662  1.00000000
    ## [3]  {}                 => {other vegetables} 0.12441161 0.1244116  1.00000000
    ## [4]  {}                 => {whole milk}       0.16429132 0.1642913  1.00000000
    ## [5]  {tropical fruit}   => {other vegetables} 0.01549425 0.2296512  0.06746862
    ## [6]  {other vegetables} => {tropical fruit}   0.01549425 0.1245402  0.12441161
    ## [7]  {tropical fruit}   => {whole milk}       0.01830544 0.2713178  0.06746862
    ## [8]  {whole milk}       => {tropical fruit}   0.01830544 0.1114206  0.16429132
    ## [9]  {root vegetables}  => {other vegetables} 0.02536611 0.3619403  0.07008368
    ## [10] {other vegetables} => {root vegetables}  0.02536611 0.2038886  0.12441161
    ## [11] {root vegetables}  => {whole milk}       0.02262029 0.3227612  0.07008368
    ## [12] {whole milk}       => {root vegetables}  0.02262029 0.1376840  0.16429132
    ## [13] {yogurt}           => {other vegetables} 0.01588651 0.1771137  0.08969665
    ## [14] {other vegetables} => {yogurt}           0.01588651 0.1276931  0.12441161
    ## [15] {yogurt}           => {whole milk}       0.02425471 0.2704082  0.08969665
    ## [16] {whole milk}       => {yogurt}           0.02425471 0.1476323  0.16429132
    ## [17] {rolls/buns}       => {whole milk}       0.01830544 0.1547816  0.11826621
    ## [18] {whole milk}       => {rolls/buns}       0.01830544 0.1114206  0.16429132
    ## [19] {other vegetables} => {whole milk}       0.04086036 0.3284288  0.12441161
    ## [20] {whole milk}       => {other vegetables} 0.04086036 0.2487067  0.16429132
    ##      lift     count
    ## [1]  1.000000 1715 
    ## [2]  1.000000 1809 
    ## [3]  1.000000 1903 
    ## [4]  1.000000 2513 
    ## [5]  1.845898  237 
    ## [6]  1.845898  237 
    ## [7]  1.651444  280 
    ## [8]  1.651444  280 
    ## [9]  2.909216  388 
    ## [10] 2.909216  388 
    ## [11] 1.964566  346 
    ## [12] 1.964566  346 
    ## [13] 1.423611  243 
    ## [14] 1.423611  243 
    ## [15] 1.645907  371 
    ## [16] 1.645907  371 
    ## [17] 0.942117  280 
    ## [18] 0.942117  280 
    ## [19] 1.999064  625 
    ## [20] 1.999064  625

We further tweak the parameters to get a more fine tuned set of pairs.
The confidence is kept at 0.2 while the support is 0.015. Now we get 8
items in our list. Co-occurrences among vegetables, whole milk and
tropical fruit (pairs of 2) seem to be showing high support and
confidence. This makes us observe that people who buy whole milk, buy
vegetables (other/root) too. Similarly, when people buy tropical fruit,
there is high occurrence of other vegetables and whole milk but not vice
verse. This makes sense as milk and vegetables are more staple
purchases.

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##         0.2    0.1    1 none FALSE            TRUE       5   0.015      1
    ##  maxlen target  ext
    ##       4  rules TRUE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 229 
    ## 
    ## set item appearances ...[0 item(s)] done [0.00s].
    ## set transactions ...[169 item(s), 15296 transaction(s)] done [0.00s].
    ## sorting and recoding items ... [56 item(s)] done [0.00s].
    ## creating transaction tree ... done [0.00s].
    ## checking subsets of size 1 2 3 done [0.00s].
    ## writing ... [8 rule(s)] done [0.00s].
    ## creating S4 object  ... done [0.00s].

    ##     lhs                   rhs                support    confidence coverage  
    ## [1] {tropical fruit}   => {other vegetables} 0.01549425 0.2296512  0.06746862
    ## [2] {tropical fruit}   => {whole milk}       0.01830544 0.2713178  0.06746862
    ## [3] {root vegetables}  => {other vegetables} 0.02536611 0.3619403  0.07008368
    ## [4] {other vegetables} => {root vegetables}  0.02536611 0.2038886  0.12441161
    ## [5] {root vegetables}  => {whole milk}       0.02262029 0.3227612  0.07008368
    ## [6] {yogurt}           => {whole milk}       0.02425471 0.2704082  0.08969665
    ## [7] {other vegetables} => {whole milk}       0.04086036 0.3284288  0.12441161
    ## [8] {whole milk}       => {other vegetables} 0.04086036 0.2487067  0.16429132
    ##     lift     count
    ## [1] 1.845898 237  
    ## [2] 1.651444 280  
    ## [3] 2.909216 388  
    ## [4] 2.909216 388  
    ## [5] 1.964566 346  
    ## [6] 1.645907 371  
    ## [7] 1.999064 625  
    ## [8] 1.999064 625

We further do a detailed analysis of our previous set (support = 0.015
and confidence = 0.2). We keep the lift threshold at 2.5 and confidence
at 0.3. From this, we see:

1.  Lift &gt; 2.5: Vegetables (root and others) co-occur frequently

2.  Confidence &gt; 0.3: Whole milk co-occurs more vegetable purchases
    depicting that it is a staple purchase of many customers.

3.  Both: Root and other vegetables clear both the thresholds.

High association of vegetables shows that customers look for these items
highly and increasing the vegetable options available in stock might be
beneficial. Similarly placing high co-occurring items together in store
(vegetables, fruits, milk, yogurt etc) might help in higher sales.

    ##     lhs                   rhs                support    confidence coverage  
    ## [1] {root vegetables}  => {other vegetables} 0.02536611 0.3619403  0.07008368
    ## [2] {other vegetables} => {root vegetables}  0.02536611 0.2038886  0.12441161
    ##     lift     count
    ## [1] 2.909216 388  
    ## [2] 2.909216 388

    ##     lhs                   rhs                support    confidence coverage  
    ## [1] {root vegetables}  => {other vegetables} 0.02536611 0.3619403  0.07008368
    ## [2] {root vegetables}  => {whole milk}       0.02262029 0.3227612  0.07008368
    ## [3] {other vegetables} => {whole milk}       0.04086036 0.3284288  0.12441161
    ##     lift     count
    ## [1] 2.909216 388  
    ## [2] 1.964566 346  
    ## [3] 1.999064 625

    ##     lhs                  rhs                support    confidence coverage  
    ## [1] {root vegetables} => {other vegetables} 0.02536611 0.3619403  0.07008368
    ##     lift     count
    ## [1] 2.909216 388

We can then plot support versus confidence for the rules. We also get
the graph for the 8 rules.

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/6.8-1.png)![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/6.8-2.png)![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/6.8-3.png)

    ##     lhs                   rhs                support    confidence coverage  
    ## [1] {tropical fruit}   => {other vegetables} 0.01549425 0.2296512  0.06746862
    ## [2] {tropical fruit}   => {whole milk}       0.01830544 0.2713178  0.06746862
    ## [3] {root vegetables}  => {other vegetables} 0.02536611 0.3619403  0.07008368
    ## [4] {other vegetables} => {root vegetables}  0.02536611 0.2038886  0.12441161
    ## [5] {root vegetables}  => {whole milk}       0.02262029 0.3227612  0.07008368
    ## [6] {yogurt}           => {whole milk}       0.02425471 0.2704082  0.08969665
    ## [7] {other vegetables} => {whole milk}       0.04086036 0.3284288  0.12441161
    ## [8] {whole milk}       => {other vegetables} 0.04086036 0.2487067  0.16429132
    ##     lift     count
    ## [1] 1.845898 237  
    ## [2] 1.651444 280  
    ## [3] 2.909216 388  
    ## [4] 2.909216 388  
    ## [5] 1.964566 346  
    ## [6] 1.645907 371  
    ## [7] 1.999064 625  
    ## [8] 1.999064 625

    ##     lhs                   rhs                support    confidence coverage  
    ## [1] {root vegetables}  => {other vegetables} 0.02536611 0.3619403  0.07008368
    ## [2] {root vegetables}  => {whole milk}       0.02262029 0.3227612  0.07008368
    ## [3] {other vegetables} => {whole milk}       0.04086036 0.3284288  0.12441161
    ##     lift     count
    ## [1] 2.909216 388  
    ## [2] 1.964566 346  
    ## [3] 1.999064 625

    ## set of 8 rules
    ## 
    ## rule length distribution (lhs + rhs):sizes
    ## 2 
    ## 8 
    ## 
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##       2       2       2       2       2       2 
    ## 
    ## summary of quality measures:
    ##     support          confidence        coverage            lift      
    ##  Min.   :0.01549   Min.   :0.2039   Min.   :0.06747   Min.   :1.646  
    ##  1st Qu.:0.02154   1st Qu.:0.2439   1st Qu.:0.06943   1st Qu.:1.797  
    ##  Median :0.02481   Median :0.2709   Median :0.07989   Median :1.982  
    ##  Mean   :0.02664   Mean   :0.2796   Mean   :0.09724   Mean   :2.116  
    ##  3rd Qu.:0.02924   3rd Qu.:0.3242   3rd Qu.:0.12441   3rd Qu.:2.227  
    ##  Max.   :0.04086   Max.   :0.3619   Max.   :0.16429   Max.   :2.909  
    ##      count      
    ##  Min.   :237.0  
    ##  1st Qu.:329.5  
    ##  Median :379.5  
    ##  Mean   :407.5  
    ##  3rd Qu.:447.2  
    ##  Max.   :625.0  
    ## 
    ## mining info:
    ##        data ntransactions support confidence
    ##  grocstrans         15296   0.015        0.2

![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/6.8-4.png)![](Final_Walker_Seth_Nesfeder_Choudhary_files/figure-markdown_strict/6.8-5.png)

We see that while we expect Bread to be a common occurance in a shopping
cart, we do not see it in our top purchased items. Lets do a specific
analysis on co-occurrence of bread.

    ## Apriori
    ## 
    ## Parameter specification:
    ##  confidence minval smax arem  aval originalSupport maxtime support minlen
    ##        0.01    0.1    1 none FALSE            TRUE       5   0.001      1
    ##  maxlen target  ext
    ##      10  rules TRUE
    ## 
    ## Algorithmic control:
    ##  filter tree heap memopt load sort verbose
    ##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
    ## 
    ## Absolute minimum support count: 15 
    ## 
    ## set item appearances ...[1 item(s)] done [0.00s].
    ## set transactions ...[169 item(s), 15296 transaction(s)] done [0.00s].
    ## sorting and recoding items ... [151 item(s)] done [0.00s].
    ## creating transaction tree ... done [0.01s].
    ## checking subsets of size 1 2 3 4 done [0.00s].
    ## writing ... [31 rule(s)] done [0.00s].
    ## creating S4 object  ... done [0.00s].

    ##      lhs                        rhs           support     confidence coverage  
    ## [1]  {}                      => {brown bread} 0.041710251 0.04171025 1.00000000
    ## [2]  {oil}                   => {brown bread} 0.001176778 0.06521739 0.01804393
    ## [3]  {butter milk}           => {brown bread} 0.001046025 0.05818182 0.01797856
    ## [4]  {sugar}                 => {brown bread} 0.001307531 0.06006006 0.02177040
    ## [5]  {UHT-milk}              => {brown bread} 0.001569038 0.07294833 0.02150889
    ## [6]  {cream cheese }         => {brown bread} 0.001765167 0.06923077 0.02549686
    ## [7]  {white bread}           => {brown bread} 0.002157427 0.07971014 0.02706590
    ## [8]  {frozen vegetables}     => {brown bread} 0.001307531 0.04228330 0.03092312
    ## [9]  {canned beer}           => {brown bread} 0.001307531 0.02617801 0.04994770
    ## [10] {coffee}                => {brown bread} 0.001503661 0.04028021 0.03733002
    ## [11] {newspapers}            => {brown bread} 0.001307531 0.02547771 0.05132061
    ## [12] {margarine}             => {brown bread} 0.002811192 0.07465278 0.03765690
    ## [13] {butter}                => {brown bread} 0.001372908 0.03853211 0.03563023
    ## [14] {pork}                  => {brown bread} 0.001111402 0.02998236 0.03706851
    ## [15] {frankfurter}           => {brown bread} 0.001372908 0.03620690 0.03791841
    ## [16] {bottled beer}          => {brown bread} 0.001765167 0.03409091 0.05177824
    ## [17] {domestic eggs}         => {brown bread} 0.003138075 0.07692308 0.04079498
    ## [18] {fruit/vegetable juice} => {brown bread} 0.002549686 0.05485232 0.04648274
    ## [19] {shopping bags}         => {brown bread} 0.001111402 0.01754386 0.06334990
    ## [20] {whipped/sour cream}    => {brown bread} 0.001307531 0.02836879 0.04609048
    ## [21] {pastry}                => {brown bread} 0.005033996 0.08800000 0.05720450
    ## [22] {citrus fruit}          => {brown bread} 0.001046025 0.01965602 0.05321653
    ## [23] {sausage}               => {brown bread} 0.002026674 0.03354978 0.06040795
    ## [24] {bottled water}         => {brown bread} 0.002811192 0.03955842 0.07106433
    ## [25] {tropical fruit}        => {brown bread} 0.001830544 0.02713178 0.06746862
    ## [26] {root vegetables}       => {brown bread} 0.001503661 0.02145522 0.07008368
    ## [27] {yogurt}                => {brown bread} 0.003464958 0.03862974 0.08969665
    ## [28] {soda}                  => {brown bread} 0.003726464 0.03323615 0.11212082
    ## [29] {rolls/buns}            => {brown bread} 0.006276151 0.05306799 0.11826621
    ## [30] {other vegetables}      => {brown bread} 0.003661088 0.02942722 0.12441161
    ## [31] {whole milk}            => {brown bread} 0.006733787 0.04098687 0.16429132
    ##      lift      count
    ## [1]  1.0000000 638  
    ## [2]  1.5635818  18  
    ## [3]  1.3949045  16  
    ## [4]  1.4399352  20  
    ## [5]  1.7489305  24  
    ## [6]  1.6598023  27  
    ## [7]  1.9110445  33  
    ## [8]  1.0137388  20  
    ## [9]  0.6276157  20  
    ## [10] 0.9657149  23  
    ## [11] 0.6108260  20  
    ## [12] 1.7897945  43  
    ## [13] 0.9238043  21  
    ## [14] 0.7188248  17  
    ## [15] 0.8680575  21  
    ## [16] 0.8173269  27  
    ## [17] 1.8442247  48  
    ## [18] 1.3150801  39  
    ## [19] 0.4206127  17  
    ## [20] 0.6801396  20  
    ## [21] 2.1097931  77  
    ## [22] 0.4712515  16  
    ## [23] 0.8043534  31  
    ## [24] 0.9484100  43  
    ## [25] 0.6504824  28  
    ## [26] 0.5143873  23  
    ## [27] 0.9261449  53  
    ## [28] 0.7968341  57  
    ## [29] 1.2723010  96  
    ## [30] 0.7055153  56  
    ## [31] 0.9826570 103

We further tweak the subset parameters and find that bread has higher
co- occurrence with other items like eggs, cheese and pastry.

    ##     lhs         rhs           support     confidence coverage  lift     count
    ## [1] {pastry} => {brown bread} 0.005033996 0.088      0.0572045 2.109793 77

    ##     lhs                rhs           support     confidence coverage   lift    
    ## [1] {UHT-milk}      => {brown bread} 0.001569038 0.07294833 0.02150889 1.748930
    ## [2] {white bread}   => {brown bread} 0.002157427 0.07971014 0.02706590 1.911044
    ## [3] {margarine}     => {brown bread} 0.002811192 0.07465278 0.03765690 1.789794
    ## [4] {domestic eggs} => {brown bread} 0.003138075 0.07692308 0.04079498 1.844225
    ## [5] {pastry}        => {brown bread} 0.005033996 0.08800000 0.05720450 2.109793
    ##     count
    ## [1] 24   
    ## [2] 33   
    ## [3] 43   
    ## [4] 48   
    ## [5] 77

    ##     lhs                rhs           support     confidence coverage   lift    
    ## [1] {oil}           => {brown bread} 0.001176778 0.06521739 0.01804393 1.563582
    ## [2] {UHT-milk}      => {brown bread} 0.001569038 0.07294833 0.02150889 1.748930
    ## [3] {cream cheese } => {brown bread} 0.001765167 0.06923077 0.02549686 1.659802
    ## [4] {white bread}   => {brown bread} 0.002157427 0.07971014 0.02706590 1.911044
    ## [5] {margarine}     => {brown bread} 0.002811192 0.07465278 0.03765690 1.789794
    ## [6] {domestic eggs} => {brown bread} 0.003138075 0.07692308 0.04079498 1.844225
    ## [7] {pastry}        => {brown bread} 0.005033996 0.08800000 0.05720450 2.109793
    ##     count
    ## [1] 18   
    ## [2] 24   
    ## [3] 27   
    ## [4] 33   
    ## [5] 43   
    ## [6] 48   
    ## [7] 77

### Summary

Upon inspection of the shopping carts we find the associations between
the specific cart items. We find that milk, vegetables, and fruits seem
to be high purchase high association items. Upon close placement of
pairs that see high confidence and support, we can expect to see
positive results in sales as they give ease of access to shoppers while
filling their cart. For items that see high confidence in X,Y pairs, we
must try to place Y close to X if we are not doing so for a similar
reason. Complementary items of high association pairs can be placed at
checkout counter as well to drive impulse purchase behavior.

Also, adding complementary offers of pairs with medium range confidence
can increase shopping tendency. We also see that an item like bread
which is expected to be a high frequency purchase does not feature in
our top list. The reasons can vary from: (1) people buying bread from
their specific bakers and not a grocery store, (2) poor product
placement, or that (3) bread of this store isn’t of great quality.

Checking association rules for shopping baskets gives a deeper insight
into purchase patterns. If the shopkeeper can also try to get user
information while checkout, we can cluster the users based on their
purchase behavior and try to increase our sales and customer retention.
