---
title: "Is Black a Color?"
date: 2013-11-09
tags: [ color, perception, visual system ]
summary: "An overview of basic color theory."
---

What is Color?  This seems like a silly question. We are all familiar with
colors and have been taught about them since kindergarten. But chances are
that unless you have studied the physics of light you will be hard pressed
to provide a concise description of what a color is. You probably remember
there being some set of primary colors (there were three, right?), and that
they mix to form other colors. But that is not really a definition, and it
leads to some open questions.

Like, is black a color? Ask this to a group of people at your next social
gathering and you are likely to get disagreements on the answer. (I'm sure
discourses on color will be considered fascinating in your social circle.)
Later I answer this question definitively (at least as far as I define
color), but first let us get a better understanding of the physiology of
sight and color.

## The Visual Response to Light

Anyone with eyelids is aware that our sight is based on the sensing of our
eyes. Vision comes from electromagnetic radiation entering through a hole
in the front of our eye called a pupil, being refracted by a lens behind
that and focused on the back of the eye called a retina.

Electromagnetic radiation is characterized by a property called
wavelength. Electromagnetic radiation of different wavelengths has all
sorts of useful properties. We use waves of about 1&nbsp;m to transmit
radio and TV signals. Microwave ovens use waves of about 1&nbsp;cm to cook
our food. X-rays, at about 1&nbsp;nm, can penetrate our bodies and can be
used to take pictures of our insides. High energy gamma-rays of about
0.01&nbsp;nm are particularly good at killing us.

It turns out that our eye can only sense these waves of radiation with
wavelengths between about 400&nbsp;nm and 700&nbsp;nm. This type of
electromagnetic radiation we call &ldquo;visible light&rdquo; (or simply
&ldquo;light&rdquo; if you are not insanely pedantic). As we look at light
from different wavelengths, we will observe they they are of different
colors. (The pure wavelength colors are by no coincidence the same colors
as a rainbow.) So, clearly, color has something to do with the wavelength
of light.

Our sense of color comes from how different types of light receptors in the
eye activate differently based on the wavelength of light.  In your retina
there are two major types of light receptors called rods and cones, named
after their shape under a microscope. I won't discuss the rod type receptor
here because it is really only used to sense low levels of light and does
not contribute to color perception at all. It is the cones that
differentiate light wavelengths into color.

{{< right >}}
  {{< figure src="cone-response.svg" width="287px"
      caption="Normalized typical response of cone receptors to different wavelengths of light. "
      attr="© Vanessa Ezekowitz, Creative Commons Attribution"
      attrlink_="https://commons.wikimedia.org/wiki/File:Cones_SMJ2_E.svg"
  >}}
{{< /right >}}

There are three types of cone receptors. The short-wavelength cones are most
sensitive to blue light; the middle-wavelength cones are most sensitive to green
light; and the long-wavelength cones are most sensitive to greenish
yellow. The response of each of these to different wavelengths of light is
shown at right. Don't worry about making too much sense of this plot. It
won't be on the quiz.

## Trichromatic Theory and Color Spaces

The fact that we have exactly three types of cones is very important. It
means that using only three colors of light, we can mix them together in
different ways to replicate any discernable color. This property is known
as the trichromatic theory. It is this trichromatic property that makes
color TVs, monitors, cameras, and pictures possible. It is not feasible to
capture and display every possible wavelength of light, but we only need
three. The colors we typically use to capture light are red, green, and
blue.

{{< right >}}
  {{< figure src="color-cube.png" width="300px"
  caption="The RGB color space." >}}
{{< /right >}}

Consider the combinations of red, green, and blue light of different
brightness. Each combination results in a different color. We can think of
all these colors existing in a 3D space such as the one shown here.

<!--

There is a caveat, though. Although we might be able to mathematically
define all perceptible colors in this RGB space, in practice there are a
few that are not possible to replicate. For example, if you try to
replicate a pure yellow light with red and green light, the green light
will also activate the short-wavelength cone a bit more than a pure yellow
light. Colors that are hard to replicate are pure rainbow colors,
iridescent, and phosphorescent objects.

-->

Some readers might be confused that I keep referring to the "primary"
colors of red, green, and blue whereas you might be familiar with primary
colors like red yellow, and blue. The difference is that so far I have
talked about color in an _additive_ color space where we are adding
different colors of light. However, the primary colors you probably learned
in elementary school dealt with paints and other pigment-based coloring.
Paint colors work differently than colored lights. Rather than emit colors,
paints actually absorb light of specific colors. Thus, a red paint is
actually absorbing all light that is not red. So when you mix paints, you
are actually combining the light colors that will be absorbed. That makes
this type of color space a _subtractive_ color space. (Incidentally, your
elementary-school teacher lied to you. Primary paint colors are not red,
yellow, and blue. They are actually magenta, yellow, and cyan. However,
since magenta looks like a light red and cyan looks like a light blue, it
is easier for a 5-year-old to simply remember red, yellow, and blue.)

## Perception of Color

Now that we know how the different wavelengths of light cause us to see
different colors, we know all there is to know about generating color,
right? Well, no, not quite. So far I have talked about colors exclusively
by their physical properties. However, we will find that the relationship
between the physical properties of colors is not a good indicator of the
relationship in our perception.

To demonstrate this difference, I particularly like the [color survey]
performed by Randall Munroe. A former NASA roboticist, Munroe is most well
known for his [xkcd internet comic]. For some time in 2010 Munroe placed on
his popular web site a survey form presenting volunteers with random colors
and asked them to type whatever name they thought best fit each color.
After some conditioning, here is a map of how people perceive saturated
colors.

{{< center >}}
  {{< figure src="color-survey-map.png"
             link="http://imgs.xkcd.com/blag/satfaces_map_1024.png"
             width="450px"
             attr="Image credit: xkcd.com"
             attrlink_="https://xkcd.com"
  >}}
{{< /center >}}

What is interesting about this diagram is that the regions representing
different colors have very different sizes. Although yellow is a very
recognizable and distinct color, it is contained in a fairly small region
of light. The region for cyan is so tiny that it's hardly ever mentioned as
a color of the rainbow even though there it is, wedged between green and
blue.

In contrast, the size of green is huge. In the spectrum of all visible
light, roughly 1/8th of it looks to be green. (I come to this 1/8th figure
using the sound scientific method of visually making a rough estimate then
confidently stating the figure in a non-peer-reviewed blog with no proper
justification.) In fact, in many of the snarky comments posted in place of
proper colors in the study, subjects indirectly commented on that fact that
the random colors came up green surprisingly often. Here are some of the
more overt (and humorous) remarks.

* jesus christ what is with you and green?
* wait, i want to change my startup answers. i am now colour blind by
  choice. i can no longer see green. and it's soo nice...
* day 3: sanity lost, colors keep changing but they keep staying the
  same...keep seeing this green, this slightly different green, mocking
  me...studying me...this ms green...what do you want mr green

The point is that if you are going to use color in a practical sense,
whether it be drawing a picture for Facebook or demonstrating de novo
chromosome formations, it is important to understand the effect of mixing
colors is highly nonuniform. For example, if you start with a pure green
light and add a bit of red light, it will still look green. The color will
continue to look green until you have almost as much red light as you do
green. But if you start with a pure red light, you don't have to add much
green to make the color look orange.

<!-- Skipping a discussion on perceptual color spaces like Munsell and
CIELAB. -->

[color survey]: http://blog.xkcd.com/2010/05/03/color-survey-results/
[xkcd internet comic]: http://xkcd.com

## Perceptual Properties of Color

So although trichromatic theory and primary colors do a good job telling us
how to make colors, it does not do a good job modeling how we interpret
colors. The only reason why you know that "yellow and blue make green" is
that you memorized it after mixing those finger paints when you were 5.

If you thought that the physical definition of color was complicated, that
is nothing compared to the messy process of defining a perceptual basis for
color. Consequently, I'm going to skip over the details (much if which I
only have a tenuous understanding anyway). Instead, I am going to suggest
that a more natural way to describe color is by its properties of hue,
saturation, and brightness.

{{< right >}}
  {{< figure
    src="hue-saturation-brightness.jpg"
    width="484px"
    caption="Hue, saturation, and brightness. "
    attr="© Commonwealth of Australia | Licensed under Creative Commons Attribution-ShareAlike 3.0 Australia License."
  >}}
{{< /right >}}

The brightness property is self explanatory. It refers to the overall
intensity of the light. The hue property refers to the dominant
wavelength. Remember that each wavelength corresponds to one of the colors
of the rainbow. The saturation refers to how dominant the hue is. Full
saturation represents a pure rainbow color. Lower saturation means the
color becomes washed out. No saturation means the light is some shade of
gray (ranging from black to white). A black-and-white photograph has no
saturation.

An interesting feature of the hue, saturation, and brightness dimensions is
that the hue and saturation form polar coordinates. The hue is measured in
degrees and is a continuous circle. The saturation is the radius of this
circle. When the saturation is zero, the hue does not matter. Thus, the
brightness axis is a singularity in the visual system where changes in hue
make no difference.

## Is Black a Color?

At the beginning of this post I promised to answer the question, "Is black
a color?" (And if you skipped right down to here, then you are big cheater
and shame on you.) The answer is, yes, black is a color. We have formally
parameterized color as a 3D space with dimensions as mixtures of light, and
black is sure enough in this space. We also talked about color as a space
of hue, saturation, and brightness, and sure enough black is there when
brightness is 0. There exist plenty of other color spaces that I've skipped
over, such as CYMK, XYZ, Munsell, Luv, and Lab, and sure enough, every
single one of them defines black as a color. In fact, it would be very
difficult to formally define color without black. It would be like having a
hole in space.

There are several reasons why black is considered not a color. One common
explanation is that black is the absence of light. Although this is
principally true, the lack of activation in your eye's light receptors is
just as important a stimulus as lots of activation. In the end, saying
black is not a color because it is the absence of light is roughly
equivalent to saying cold is not a temperature because it is the absence of
heat. I can assure you, I have been outside on a winter night. I saw a
black sky and felt a temperature that was freakin' cold.

Another common reason for the misconception that black is not a color is
confusing color with hue. Hue is not the same thing as color; it is just
one of three dimensions of color. You cannot specify every color with only
hue. Consider, for example, brown. Brown has the same hue as orange (but is
darker and has less saturation). Are you willing to claim brown and orange
are the same color? If not, then it really does not make sense to claim
black is not a color.

So rejoice. Black is a color. But if that's all you see, I suggest you turn
on a light before you run into a wall.
