---
title: "Building Better Plots"
date: 2017-02-18
tags: [ visualization, plots, charts ]
---

Consider the lowly chart. Chances are you
have seen quite a few, perhaps in advertisements, news, reports, or papers.
You have probably even made a few yourself for school or work. There was a
time when making such figures was an arduous chore. But now every
spreadsheet program worth its salt can quickly plot its data, and several
programs and libraries are dedicated to plotting charts.

{{< right >}}
{{< figure
  src="chart-montage.png"
  width="400px"
  caption="A small montage of plots. Some are good. Some are bad. Let's find out which are which."
>}}
{{< /right >}}

But although all of this software gives you the tools to display data in
meaningful ways, they also give you plenty of leverage to proverbially
shoot yourself in the foot. Have you given much thought about what makes a
plot effective and what makes the presentation obscure, unclear, or
downright dishonest? Few people have, but if you care to read further we
will go over some first principles on creating better plots.

Throughout this blog I will give several examples. Some will be good,
others bad. Those examples showing a particularly good feature of plotting
I will mark with ![](good-example.png) whereas bad examples I will mark with
![](bad-example.png).

{{< clear-float >}}


## Chart Types

Let us begin by going over some of the basic and common chart types. You
likely have experience with all of these chart types, but we will discuss
what each are best for and --- just as important --- when you shouldn't use
them.

### X-Y Charts

Our first type of chart is the basic X-Y chart. The concept of the X-Y
chart is simple. The space of the chart is gridded along horizontal and
vertical axes, not unlike a map is gridded by latitude and longitude. When
data are plotted on this chart, it can demonstrate a relationship between
the variables of the two axes.

{{< right >}}
{{< figure
  src="XY_Trend.svg" width="300px"
  caption="Using an X-Y chart to capture a simple trend over time."
>}}
{{< /right >}}

For example, one common use of an X-Y chart is to show how something might
change over time. This is done by representing time on the horizontal axis
and then drawing a curve from left to right that represents how a measured
variable changes over time. In the example here, we are plotting how the
average miles per gallon (MPG) changed in vehicles manufactured between
1970 and 1982. The data show a clear upward trend of MPG during this time
period with the average under 20 MPG in 1970 and around 30 MPG by 1982.
This response won't surprise anyone living in the U.S. during the [oil
crises of the 1970's]. It is only natural that consumers would demand more
fuel efficient vehicles. (Incidentally, these data come from [an example
for the CMU StatLib library]. We'll be using this data set a lot because it
is fun and generates interesting plots.)

{{< right >}}
{{< figure
  src="XY_Scatterplot.svg" width="300px"
  caption="Using an X-Y chart for a scatterplot."
>}}
<img src="good-example.png" style="position:absolute; top:120px; left:260px"/>
{{< /right >}}

X-Y charts also make a good canvas to show lots of data at once.
Scatterplots in particular are good at showing lots of data at once. In a
scatterplot, you typically have two arbitrary variables on each axis, and
then use those as coordinates to plot a dot for each data value. Because
the dots are small, you can fit a lot of them on the chart at once.
Consider the scatterplot that is shown here, which is showing the
correlation between car weights and horsepower. This chart is showing
values for almost 400 separate values, but it is organized in such a way to
clearly show the relationship among them.

This is a good plot that shows us lots of interesting information about the
data. Clearly there is a positive correlation between weight and
horsepower. This is not surprising as heaver vehicles need more power to
move them. We can also see a tighter correlation for lighter vehicles than
heavier ones. Finally, we see an outlier with a significantly high
horsepower for its body weight. (This outlier happens to be a [1970 Buick
Estate Wagon], and its weight is probably mis-entered in the data.)

{{< right >}}
{{< figure
  src="XY_Scatterplot_Trend_Bad.svg" width="300px"
  caption="This chart pretends the data represents a functional trend when it is not."
>}}
<img src="bad-example.png" style="position:absolute; top:100px; left:260px"/>
{{< /right >}}

So far we have given 2 examples of X-Y charts. One used a line and the
other used dots. When should you use which? You should use a line only when
your data can be thought of as representing a mathematical function where
for each position on the horizontal axis there is exactly one right number
for the vertical axis. The first example follows this criterion. You can
think of time as a continuously increasing variable, and for each point in
time there is a single average MPG for vehicles made at that time. That is
not the case for the second example. There is no single horsepower that
corresponds to a particular weight of a car. Trying to connect the values
via a line creates a mess of no value as shown here.

{{< right >}}
{{< figure
  src="XY_Trend_Bad.svg" width="300px"
  caption="Using a line to demonstrate a \"trend\" over arbitrarily positioned discrete categories is nonsense."
>}}
<img src="bad-example.png" style="position:absolute; top:100px; left:260px"/>
{{< /right >}}

I bring this up because many plotting programs, such as Microsoft Excel's
charts, tend to lead you toward line charts. It's not so much that the
program is misbehaving. It is just that the program does not understand the
data as well as you should. Granted, that last example was pretty extreme,
but you have probably seen examples of plots like the one shown here where
a line is plotted over categorical data. The order of the categories is
completely arbitrary (in this case they are alphabetized, but that has no
bearing on the data), and suggesting a trend by connecting the measures
with a line is just false.

So how should categorical measurements like these be represented? A very
good answer is: bar charts.

{{< clear-float >}}

### Bar Charts

{{< right >}}
{{< figure
  src="Bar.svg" width="300px"
  caption="Bar charts are good at representing categorical data."
>}}
{{< /right >}}

Bar charts, as the name would imply, use bars whose relative length denotes
value. Bar charts can be a very effective tool to compare measurements as
people are pretty good at estimating relative linear distance. This example
shows the previous data as a bar chart. Here the context makes clear what
the data are and are not.
{{< clear-float >}}

{{< right >}}
{{< figure
  src="Bar_Sorted.svg" width="300px"
  caption="Sorting bars from highest value to lowest often makes the data easier to read."
>}}
<img src="good-example.png" style="position:absolute; top:10px; left:260px"/>
{{< /right >}}

One of the nice things about bar charts is that they provide a lot of
leverage to move things around. For example, unless there is some grouping
of the bars, the position of the bars can be reordered to make them easier
to read. In particular, sorting them from highest to lowest value often
makes bars easier to compare with each other. It also has the added benefit
of providing an ordered list of categories from high to low, which itself
can be useful. 
{{< clear-float >}}

{{< right >}}
{{< figure
  src="Bar_Rotated.svg" width="300px"
  caption="Rotating the bars can sometimes make more efficient use of space."
>}}
<img src="good-example.png" style="position:absolute; top:100px; left:260px"/>
{{< /right >}}

Another possible format change for bar charts is to rotate the chart so
that the bars are horizontal, as shown here. Sometimes this layout of the
bar chart can make more efficient use of the space and also provides a nice
list of the categories. One word of warning: most plotting programs when
orienting bars this way want to order them from the bottom up, so you may
need to play with it to get a nice top to bottom ordering.

{{< clear-float >}}

### Pie Charts

{{< right >}}
{{< figure
  link="SillyPieChart.png" thumb="_Thumbnail" width="400px"
  caption="Sarcastic pie charts. The lowest form of all comedies."
>}}
{{< /right >}}

Pie charts are quite popular because they are an easily recognizable
metaphor for parts of a whole. It is the same metaphor we use to teach
fractions to elementary school children, so they are easy to understand.
Plus, they look delicious.

However, I recommend never using pie charts. As a form of visual display,
they are quite terrible. The problem is that people are not that good at
comparing relative angles, especially when they are rotated with respect to
one another.
{{< clear-float >}}

{{< right >}}
{{< figure
  src="PieChart.svg" width="200px" link="PieBar_Compare.svg"
  caption="Guess which wedges are largest and smallest."
>}}
<img src="bad-example.png" style="position:absolute; top:0px; left:175px"/>
{{< /right >}}

As a quick example of this phenomenon, take a quick look at the pie chart
here. How easily can you determine which of the wedges in this pie is the
largest? Which is the smallest? How sure are you? Take a few moments...
three, two, one. OK. Now [click here](PieBar_Compare.svg) to see the same
values in a bar chart.

Did you get it right? Even if you guessed correctly, how sure were you? In
any case, I'm sure you will agree that it is much easier to measure the
data with the bar chart. It is for this reason why I recommend never using
a pie chart. There is no reason not to use a bar chart instead. If you are
interested in more information on the subject I recommend [this white paper
by Stephen Few].

{{< clear-float >}}

### Data by Area

It is natural to use relative size to indicate the value of a data item. It
is how bar charts work: Larger bars indicate larger values. It is also how
pie charts work. Another method often employed is associating the area of a
shape to a data value.

{{< right >}}
{{< figure
  src="AreaMystery.png" width="239px" link="AreaReveal.png"
  caption="How large is the smallest circle? "
  attr="(Originally published in IEEE Spectrum.)"
  attrlink_="http://spectrum.ieee.org/at-work/innovation/chinas-supercomputing-prowess"
>}}
<img src="bad-example.png" style="position:absolute; top:125px; left:175px"/>
{{< /right >}}

How well does this method stack up? Consider the example shown here. Not
unlike a pie chart, this particular diagram is showing parts of a whole.
The darkest outer circle represents 100% The next largest circle represents
50%.

The question is, how large is the smallest circle, labeled with question
marks? Is it 25%? 10%? 15%? Once you have guessed, [click
here](AreaReveal.png).

How did you do? I'm guessing that the circle was not as big as you thought.
That is the problem with using area to represent values: It is difficult to
determine the linear relationship between relative areas. In contrast, it
is easy to determine the relative distance of bar charts, which are a much
more effective display.

{{< clear-float >}}

### 3D Charts

Many plotting programs have the ability to plot data using 3D graphics.
Many designers are drawn to 3D graphics because the extra dimension
provides a richer space to represent data. Designers also like 3D graphics
because they look more interesting, and often 3D effects are extruded from
2D information to make the view more engaging.

But 3D graphics are problematic. We can point out lots of technical issues
with using 3D graphics for plotting, but they all come back to problems
with interpreting 3D data through 2D images. When we say something is drawn
in 3D, it's not really 3D. It's a 3D space projected down to a 2D image.
Even if we were to plot the data using some future hologram technology that
displayed it in a real 3D space, ultimately the viewer has to reconstruct
the 3D nature from 2D projections on the retina at the back of the eye.
When a person looks at a 3D picture or object, that person has to perform
the "inverse problem" of converting that 2D image back into a 3D mental
model. This problem, like many inverse problems, is ambiguous and error
prone. Humans are quite adept at reconstructing a 3D mental model, but
locations can be imprecise, and many of the visual cues we use are missing
from 3D plots.

{{< right >}}
{{< figure
  link="3DChart.png" thumb="_Thumbnail" width="300px"
  caption="3D charts effects might look nice, but they tend to obscure data."
>}}
<img src="bad-example.png" style="position:absolute; top:0px; left:275px"/>
{{< /right >}}

Consequently, I recommend staying away from 3D charts as much as possible.
Take for example this chart that uses 3D to show the trend of the average
MPG of vehicles for each year divided by the origin of manufacture. It
makes use of the extra dimension to space out the data, but the 3D
projection makes it difficult to compare values. The upward trend is much
less noticeable; a comparison of like entries for different origins is
near impossible, and some of the bars are completely obscured by other
ones.
{{< clear-float>}}

{{< right >}}
{{< figure
  src="MultiSeries.svg" width="300px"
  caption="It is much more effective to show multiple series in a 2D plot than to try and space them out in a 3D plot."
>}}
<img src="good-example.png" style="position:absolute; top:120px; left:260px"/>
{{< /right >}}

Compare that to the same data shown as multiple series on the same flat X-Y
chart shown here. Although this chart may look more plain, the data are
much easier to understand. Trends are self evident, comparisons across
series are easier, and no data are obscured.

Don't be drawn into the luster of 3D graphics. 3D graphics were tricky to
create and a rarity 3 decades ago. But it's 2017 now. Even the lowest
powered computer processors can quickly generate 3D graphics, and software
packages that make it easy to generate 3D graphics abound. 3D no longer
means you have the prowess to make amazing plots. It probably means you
don't know enough about data display to do it right.

{{< clear-float >}}


## Axis Scaling

Now that we have covered the most common of charts along with their
strengths and weaknesses, let us consider some of the properties of these
charts and how they can help or hinder our interpretation of the data. Our
first topic is on the scaling of the axes. By this I mean the choice of the
minimum and maximum values along each axis. The choice of this domain can
have a huge impact on the interpretation of data.

{{< right >}}
{{< figure
  src="ActiveBlocksBroken.svg" width="300px"
  caption="This plot seems to show a sudden drop near cycle 200, but something is wrong."
>}}
<img src="bad-example.png" style="position:absolute; top:0px; left:275px"/>
{{< /right >}}

Consider the example here that was generated as part of the work for a
technical report I was involved in. The plot shows the number of blocks
generated by an adaptive mesher in a shock physics simulation. As the
material in the simulation tends to fragment, we expected that the number
of blocks required to represent them would go up, but the plot shows the
number of blocks going down.

It is a curious effect, and we spent some time hypothesizing why this
occurred. Then I noticed the numbers on the vertical axis. Although it
looks like there is a sudden drop, the entire range is less than 0.03% of
the total number. This plot was created with an R plotting package that,
like most plotting software, chose an axis range based on the minimum and
maximum values. This leads to a plot that consists of essentially all
noise.

{{< right >}}
{{< figure
  src="ActiveBlocksFixed.svg" width="300px"
  caption="This rescaled plot is boring, but correct. There is no significant change in data."
>}}
<img src="good-example.png" style="position:absolute; top:90px; left:250px"/>
{{< /right >}}

The corrected plot is shown here. With the Y axis appropriately scaled with
a 0 baseline, it is now clearly shown that the variance in the data is
insignificant.

This is a cautionary tale to be on the lookout for bad default values from
plotting software. It is also a call to plotting software to be more
careful about the default scaling. Most data ranges should start at 0. This
is not always the case (for example, 0 Kelvin is seldom a good baseline for
temperature), but if it is not think real hard about the justification.

This particular example was an honest mistake, but this form of data
misrepresentation is quite common in wide media. [Several egregious
examples were presented at the most recent VisLies meeting], and I recommend
taking a look.

{{< clear-float >}}


## Using Color

Color can be an important component when plotting data. When used
correctly, it can add another dimension to the data displayed. If used
incorrectly, it can hide or confuse the display. Color is a fascinating and
complex topic, and I could drone on at length on the subject (such as in
[this earlier blog post](../is-black-a-color/)). But instead I will just
touch on some of the issues here and point to some solutions.

### Choosing Good Color Sets

{{< right >}}
{{< figure
  src="Colors_Bad.svg" width="300px"
  caption="When colors are used poorly, plots get messy."
>}}
<img src="bad-example.png" style="position:absolute; top:100px; left:275px"/>
{{< /right >}}

Here is shown a plot with exceptionally poor color choice. There are two
major problems with the colors in this plot. The first problem is that the
colors chosen are not conducive to seeing the data elements. These colors
were chosen, as many naively chosen colors are, by picking supersaturated
colors along the hue color wheel. These colors are not ideal. For starters,
many are difficult to see. In particular, the yellow is almost invisible on
the white background. And an uneven dispersal of colors makes some harder
to distinguish some colors from others.

The criteria for choosing good colors is actually very complex. My advice
is to avoid picking your own colors and instead pick a color set designed
by a professional. The first place I go when I need a group of colors is
https://colorbrewer2.org. This [Color Brewer] site contains a well
organized collection of color sets of different sizes and for different use
cases. Although you will see that the colors were originally designed for
cartography, the color sets are so useful they are applied to just about
every mode of visual data display. If you make a link to anything mentioned
in this blog, make it to [Color Brewer]. Fortunately, many plotting tools
such as Excel, Tableau, Gnuplot, Matlab, and Toyplot have put some thought
into the default color sets in recent versions. If, however, you find
yourself using a plotting tool with poor colors, there is always [Color
Brewer].

{{< right >}}
{{< figure
  src="Colors_Bad.svg" width="300px"
  caption="When colors are used poorly, plots get messy."
>}}
<img src="bad-example.png" style="position:absolute; top:100px; left:275px"/>
{{< /right >}}

But enough about [Color Brewer]. Let's get back to this plot, which I'm
repeating here so you don't have to keep scrolling back up. Another problem
here is that I'm using too many colors. The colors (which I've also failed
to label because there are too damn many) represent the different makes of
vehicle, of which there are 31 in this database. Even if I were to label
each color, 31 is far too many to reliably identify.

I know what you are thinking. Wait a minute. I can tell the difference
between way more than 31 colors. Heck, an image of 24-bit colors (typical
of most currently used image formats) can differentiate over 16 million
colors, and new high dynamic range sensors and displays are working to
differentiate even more. Surely we can see more than 31 different colors.

Well, yes and no. It is true that people (with normal vision) can
differentiate millions of different colors over the entire visual color
range. But this number is based on a "just noticeable difference" when two
color swatches are placed adjacent to each other. It turns out that if you
place the color swatches far from each other, you need a much more
distinctive change to reliably compare colors.

{{< right >}}
{{< figure
  src="ColorCompare_Thumbnail.png" width="300px"
  link="ColorCompare.gif"
  caption="Are the green squares the same color or the blue squares? Click on the image when you think you know."
>}}
{{< /right >}}

To demonstrate this effect, take a look at the image here. There are two
green squares and two blue squares. One set of squares is the exact same
color, the other set are a slightly different shade. The question is, are
the green the same, or are the blue the same. Once you think you know the
answer click on the image or [here](ColorCompare.gif). Did you guess right?
Even if you did, I bet you were surprised at how different the colors
looked when they were close.

In all, it is a bad idea to expect a viewer to reliably differentiate more
that 12 colors (including at least 1 for a background color). Furthermore,
under most circumstances I would avoid using more than 7 as studies show
most people have difficulty keeping track of more than 7 distinct items at
one time.

{{< right >}}
{{< figure
  src="Colors.svg" width="300px"
  caption="Appropriate use of color in a scatterplot."
>}}
<img src="good-example.png" style="position:absolute; top:100px; left:260px"/>
{{< /right >}}

Applying the two recommendations for color we just discussed, we can come
up with a plot like the one shown here. The colors come from [Color
Brewer], and we can see that they are both more
aesthetically pleasing and easy to identify. We have also dramatically
reduced the number of colors we are attempting to display. Instead of
quixotically attempting to make sense out of every car make, we have
divided the makes into three categories based on the origin of
manufacturing. Not only is this division easier to label and see, we get
some interesting information about these classes, such as the USA had
cornered the market on the large, powerful cars.

### Consistent Colors

One final note about using colors. Often documents, particularly technical
papers, will have multiple figures with related data. When colors change
from one figure to the next, as shown in the following pair of images, it
makes logical arguments hard to follow.

<figure sytle="width: 100%; position: relative;">
  <table class="image-table">
    <tr>
      <td><img src="Colors.svg" /></td>
      <td><img src="MultiSeries_Inconsistent_colors.svg" /></td?
    </tr>
  </table>
  <img src="bad-example.png" style="position:absolute; top:50%; left:90%"/>
  <figcaption>
    The meaning of the colors changes from the left image to the right
    image. Don't do that in your papers.
  </figcaption>
</figure>

If you are using colors to identify categories, strive to keep the meaning
of colors consistent among all the figures.

<figure sytle="width: 100%; position: relative;">
  <table class="image-table">
    <tr>
      <td><img src="Colors.svg" /></td>
      <td><img src="MultiSeries.svg" /></td?
    </tr>
  </table>
  <img src="good-example.png" style="position:absolute; top:50%; left:90%"/>
  <figcaption>
    The meaning of the colors changes from the left image to the right
    image. Don't do that in your papers.
  </figcaption>
</figure>


## Grid Lines

{{< right >}}
{{< figure
  src="MultiSeries_Grid_Dark.svg" width="300px"
  caption="The grid lines completely take over the plot making the actual data hard to see."
>}}
<img src="bad-example.png" style="position:absolute; top:120px; left:260px"/>
{{< /right >}}

Lots of designers like to add grid lines to their plots to make it easier
to follow plotted data to the axes and determine their actual values.
However, if you choose to add grid lines to the plot, make sure that they
do not distract from the data being shown. In the plot shown here the grid
lines are numerous and dark. They completely overshadow all of the data
making it very difficult to see the actual data in the plot.
{{< clear-float >}}

{{< right >}}
{{< figure
  src="MultiSeries_Grid_Light.svg" width="300px"
  caption="Lighter grid lines make it much easier to focus on the data."
>}}
{{< /right >}}

If you do decide to use grid lines, make them with muted colors that blend
in with the background as shown here. The muted grid lines make it easier
to focus on the actual data in the plot, yet they are still easy enough to
follow to axes to get the actual values.
{{< clear-float >}}

{{< right >}}
{{< figure
  src="MultiSeries_Grid_Light_Fewer.svg" width="300px"
  caption="There is no need to saturate a plot with lots of grid lines. Fewer is better."
>}}
<img src="good-example.png" style="position:absolute; top:120px; left:260px"/>
{{< /right >}}

Personally, I find grid lines overrated. Gone are the days where we need a
grid to draw values in a chart. The computer does that for us. We do not
need all that many to get a good idea of what the values are. Vertical
lines in a series like this are usually unnecessary, and we do not need so
many horizontal lines.
{{< clear-float >}}

{{< right >}}
{{< figure
  src="MultiSeries.svg" width="300px"
  caption="Don't be afraid to use no grid lines at all."
>}}
<img src="good-example.png" style="position:absolute; top:120px; left:260px"/>
{{< /right >}}

For that matter, why do we need grid lines at all? Keep the canvas nice and
clean to put all the focus on data. If you want to be able to retrieve the
exact values for data, label them or put them in a table.
{{< clear-float >}}

## Legends

A legend is that little box put in the corner or side of a chart that
provides a key for the meaning of colors and shapes in the chart. Every
plotting program I have ever used creates legends for their labeling
purposes. This is because, from a programmer's perspective, it is easier to
create a box of labels than to find a place in the data space.

That said, I have a personal dislike to legends. They are distracting
because you constantly have to look back and forth between the data and the
legend, which interferes with your concentration. You may have noticed that
throughout all of my examples you have not seen one legend. Rather, I have
chosen to place labels directly in the plot area. I find that to be much
more effective labeling.

That said, I know you are going to create plots with legends simply because
that is what all the software tools do. However, if you do choose to use
the legend created by your plotting software, I ask that at the very least
you order the entries in the legend in the same order as the data.

{{< right >}}
{{< figure
  src="Legend_Backward.svg" width="300px"
  caption="The legend ordering is inconsistent with the data."
>}}
<img src="bad-example.png" style="position:absolute; top:120px; left:180px"/>
{{< /right >}}

If we look at the plot here, we see that the "Europe" and "Japan" series
generally have the highest value whereas the "USA" series is consistently
lower. But the legend shows USA on the top. This creates an inconsistent
mental model and makes it more difficult to interpret the data. Plotting
programs consider the order in the legend as arbitrary and generally use
the same order the data happen to be listed in. In this case, the order of
the data happens to conflict with the values in the data.
{{< clear-float >}}

{{< right >}}
{{< figure
  src="Legend_OK.svg" width="300px"
  caption="At least now the legend gives the same message as the data."
>}}
{{< /right >}}

Legends are much more clear if you give some thought to the order in which
the keys are listed. In this corrected example, the entries in the legend
are reordered to better match the values in the data.

One word of caution about reordering the legend. A lot of plotting programs
use the same order of entries in the legend to assign the colors of data
series. The upshot is that reordering the legend may have the unintended
side effect of changing the colors, which in turn might lead to
inconsistent colors between plots if you make more than one. Thus, you
might need to simultaneously edit the legend ordering and the colors.
{{< clear-float >}}

{{< right >}}
{{< figure
  src="MultiSeries.svg" width="300px"
  caption="An awesome chart will label the data directly rather than use a legend."
>}}
<img src="good-example.png" style="position:absolute; top:120px; left:260px"/>
{{< /right >}}

Better yet, make your chart awesome and don't use legends at all! I find an
effective technique for line series plots is to place a color-coded label
at the right end of the line. It places all labels in the same relative
space, makes it clear what label keys to what series, and ensures that the
ordering of the labels is consistent with the data. Unfortunately, you are
unlikely to see labeling like this any time soon from Excel's charts. To do
this, you will have to go through the extra steps of turning off the
legend, saving the chart as an image, and then adding your own labels after
the fact in a separate image editor.
{{< clear-float >}}


## Lies, Damn Lies, and Statistics

Our last topic is an advanced topic on revealing the data behind the simple
statistics we often use when charting data. Often to make data visually
accessible we aggregate data into averages (as I have done in many of my
examples).

{{< right >}}
{{< figure
  src="Bar_Sorted.svg" width="300px"
  caption="I said earlier this bar chart is good, but what data is it really hiding?"
>}}
{{< /right >}}

Take for example this bar chart that I said earlier was a good example of a
chart. It gives a clean story of the top car makes sold in the US and their
fuel consumption rate. It gives us a clear ordering of makes, from best to
worst, and an indication of the distribution of gas consumption.

But hiding within each of these bars are measurements from several models
of cars. Is it true that that a Honda is necessarily more fuel efficient
than any other make? That does not seem right, and a closer inspection of
the data reveals that it is not right.
{{< clear-float >}}

{{< right >}}
{{< figure
  src="Detail.svg" width="300px"
  caption="1D scatterplots reveal an interesting distribution of data."
>}}
<img src="good-example.png" style="position:absolute; top:0px; left:260px"/>
{{< /right >}}

In this chart we replace each bar with a 1D scatterplot of all models for
the respective make, of which there are many. This more detailed view into
the data reveals a much more complex relationship among makes and fuel
consumption. The original bar chart suggested that Honda cars have the best
fuel efficiency, but we see that Volkswagen produced several models that
had better MPG than all but one model of Honda. Mazda, which is fourth down
on the list of averages, actually makes the model that has the best fuel
efficiency.
{{< clear-float >}}

{{< right >}}
{{< figure
  src="MultiSeries.svg" width="300px"
  caption="I also said this line chart is good, but what is it hiding?"
>}}
{{< /right >}}

As a final example, let's take another look at this multi-line series chart
I have used numerous times so far as a good example of plotting. The chart
suggests that although all manufacturers improved their fuel efficiency
during the 1970's, Japanese and European cars were always better than those
from US makes. Based on this chart you would probably conclude that a US
consumer should always go for a foreign make if he or she is interested in
fuel efficiency. However, once again the data plotted is aggregated into
averages. How valid are our conclusions?
{{< clear-float >}}

{{< right >}}
{{< figure
  src="Detail_MultiSeries_Trend.svg" width="300px"
  caption="A detailed view of the data reveals practical information not available in the summarized averages."
>}}
<img src="good-example.png" style="position:absolute; top:120px; left:260px"/>
{{< /right >}}

Here is a plot that shows the data with all their detail. Here you can see
that the actual story is more complex and interesting. The detailed data
confirm that in 1970 a consumer interested in fuel efficiency should
definitely prefer a foreign car over a domestic one. But by the early
1980's the story had changed. Although US makes cornered the market on
large, powerful cars, they also responded to consumer demand by making
several models of fuel efficient cars. In fact, many domestic makes were
very competitive to their foreign counterparts, and a consumer would be
wise to look at both.

As I hope I have demonstrated, details matter. Aggregating data can bias
conclusions quite a bit. Be wary when complex data are coalesced into
simplified forms, and don't be afraid to dive deeper. Showing more data
adds credence and honesty to your display.

## Making These Plots

As you have read this, you might have been wondering what I have been using
to make these amazing, beautiful, wonderful, glorious example plots. (OK,
you might have used less adjectives, but I bet you were thinking them.)
Before I get there I will start by saying that I'm guessing you already
have a plotting program you are familiar with and that it probably is
capable of creating equivalent plots of most of my examples.

I postulate (with zero evidence) that [Microsoft Excel] is the most widely
used plotting program. When I first started using Excel around 20 years
ago, the plots it produced were, frankly, not all that good. But Microsoft
has made many improvements over the years. Now that you know the ins and
outs of good chart design, you can use the tools in Excel to make effective
charts. (Note, I have no affiliation with Microsoft or any of its
products.)

Although Excel does fine making standard 2D histograms, it would be
difficult to get it to create the detailed plots I gave in the previous
section. The best user-friendly GUI tool I have found for that is
[Tableau]. I would describe the interface to Tableau as something like a
visual pivot table. One of the features I like best about Tableau is that
it allows and even encourages detailed views into data. The full featured
product is quite expensive, but the [Tableau Public] version is free so
long as you can release your data publicly. I highly recommend playing with
the product.

Of course, neither of these products add labels correctly. (They both rely
on legends.) The best you can do is plot without the legend and then use an
image editing program to add them in after the fact.

That said, I did not use either of these two tools for these charts. I have
created these charts by loading data into a Python script and using the
[Toyplot] module. I hesitate to recommend this approach to everyone because
using Python scripting for plotting is much more time consuming than other
approaches (and inaccessible if you are not a programmer). But using this
approach offers several advantages. First, the script leaves an artifact
that makes the plotting reproducible. Second, I can use other Python
modules, like [pandas], to manipulate the data into a form amenable to
plotting. Third, scripting provides me with a lot of control over the
plotting so I, the picky person that I am, can make it look exactly as I
want. I use Toyplot because it creates nice, clean displays of data. And
although Toyplot does not automatically label data, I've bugged the author
enough to add features that make it easy to create labels without a legend.

If you are a coding wiz interested in how I used scripting to create these
plots, you can click on the following links to see the code for [trend
lines], [scatterplots], [bar charts], [colors], [multiple series and grid
lines], [details for makes], and [details for year].

[trend lines]: https://github.com/kmorel/kmorel.github.io/blob/master/images/better-plots/XY_Trend.ipynb
[scatterplots]: https://github.com/kmorel/kmorel.github.io/blob/master/images/better-plots/XY_Scatterplot.ipynb
[bar charts]: https://github.com/kmorel/kmorel.github.io/blob/master/images/better-plots/Bar_Basic.ipynb
[colors]: https://github.com/kmorel/kmorel.github.io/blob/master/images/better-plots/Colors.ipynb
[multiple series and grid lines]: https://github.com/kmorel/kmorel.github.io/blob/master/images/better-plots/MultiSeries.ipynb
[details for makes]: https://github.com/kmorel/kmorel.github.io/blob/master/images/better-plots/Detail_Make.ipynb
[details for year]: https://github.com/kmorel/kmorel.github.io/blob/master/images/better-plots/Detail_MultiSeries.ipynb


## Conclusion

There is no right or wrong way to plot data. The best I can do is provide
some advice. Plotting data is all about communicating information
effectively and honestly.

Go forth, and be awesome with your data!


[oil crises of the 1970's]: https://en.wikipedia.org/wiki/1973_oil_crisis
[an example for the CMU StatLib library]: https://archive.ics.uci.edu/ml/datasets/Auto+MPG
[1970 Buick Estate Wagon]: https://en.wikipedia.org/wiki/Buick_Estate#1970
[this white paper by Stephen Few]: http://www.perceptualedge.com/articles/visual_business_intelligence/save_the_pies_for_dessert.pdf
[Several egregious examples were presented at the most recent VisLies meeting]: http://www.vislies.org/2016/gallery/#axis-scales
[this earlier blog post]: /is-black-a-color/
[Color Brewer]: https://colorbrewer2.org
[Microsoft Excel]: http://office.microsoft.com/en-us/excel
[Tableau]: https://www.tableau.com/
[Tableau Public]: https://public.tableau.com/s/
[Toyplot]: https://toyplot.readthedocs.io/en/stable/
[pandas]: http://pandas.pydata.org/
