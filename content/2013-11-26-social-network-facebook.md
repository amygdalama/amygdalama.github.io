Title: Visualizing Social Circles Using Facebook Data
Date: 2013-11-26 23:07
Author: Amy Hanlon
Category: Projects
Tags: igraph, r, rfacebook, networks
Slug: visualizing-social-circles-using-facebook-data

Intrigued by David Smith's [Facebook friends network analysis][] using
the Rfacebook package, I decided to try it out on my own group of
friends, to see if my social circles are clique-y ala [North Shore High
School].

![alt text][fb-network]

It turns out the social circles in my network were visualized almost
perfectly! The smallest group to the far right of the graph contains
family members, the large group at the top contains current co-workers,
and the large group at the bottom contains friends in the Austin cyclist
scene. Other clusters are friends from high school, friends from
college, and bar friends. The most isolated group is my family. The
groups with the most connections to each other are my current coworkers,
cyclists, and bar friends, which makes sense, because they are roughly
in the same age group and live (or have lived) in Austin, a fairly
connected city. Now I'm wondering how to get the data to see if Austin
really is more connected than other cities or if it's just in our
collective imagination.

Thanks to David for the [tutorial][Facebook friends network analysis] on
graphing social networks and to Julianhi for his [tutorial] on setting
up Rfacebook.

  [Facebook friends network analysis]: http://blog.revolutionanalytics.com/2013/11/how-to-analyze-you-facebook-friends-network-with-r.html
  [North Shore High School]: https://raw2.github.com/amygdalama/amygdalama.github.io/master/images/nshs.jpg
  [fb-network]: https://raw2.github.com/amygdalama/amygdalama.github.io/master/images/fb-network.png
  [tutorial]: http://thinktostart.wordpress.com/2013/11/19/analyzing-facebook-with-r/
