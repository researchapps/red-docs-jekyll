---
layout: post
title: How to use our tool
tags: "hpc"
categories: news
snippet: "Learn how to use our tool, it's really great"
---

This is early documentation on how to use our tool. Notice that we've given it a tag, "hpc" that corresponds with a template in `pages/tags/tag_hpc.html` that will automatically generate a page for all posts tagged with "hpc."

{% include toc.html %}

## Quick Start

You might want to give the user a quick start, and follow with numbered steps. We are going to borrow a post on <a href="http://vsoch.github.io/2016/pokemon-ascii/" target="_blank">Pokemon Ascii</a> to show a general example with different formatting. Here we go!

If you are a user of this place called the Internet, you will notice in many places that an icon or picture "avatar" is assigned to your user. Most of this is thanks to a service called <a href="https://en.gravatar.com/site/implement/images/" target="_blank">Gravatar</a> that makes it easy to generate a profile that is shared across sites. For example, in developing <a href="https://github.com/singularityware/singularity-hub" target="_blank">Singularity Hub</a> I found that there are many <a href="http://django-avatar.readthedocs.io/en/latest/" target="_blank">Django plugins</a> that make adding a user avatar to a page as easy as adding an image with a source (src) like `https://secure.gravatar.com/avatar/hello`.

The final avatar might look something like this:

<img src="https://secure.gravatar.com/avatar/hello?r=g&s=100&d=retro" width="100px">

This is the "retro" design, and in fact we can choose from one of many:

<div>
    <img src="https://vsoch.github.io/assets/images/posts/pokemon-ascii/gravatar_designs.png" style="width:800px"/>
</div><br>


## Command Line Avatars?
I recently started making a command line application that would require user authentication. To make it more interesting, I thought it would be fun to give the user an identity, or minimally, something nice to look at at starting up the application. My mind immediately drifted to avatars, because an access token required for the application could equivalently be used as a kind of unique identifier, and a hash generated to produce an avatar. But how can we show any kind of graphic in a terminal window? 


<div>
    <img src="https://vsoch.github.io/assets/images/posts/pokemon-ascii/terminal.png" style="width:800px"/>
</div><br>


## Ascii to the rescue!
Remember chain emails from the mid 1990s? There was usually some message compelling you to send the email to ten of your closest friends or face immediate consequences (cue diabolical flames and screams of terror). And on top of being littered with exploding balloons and kittens, <a href="https://en.wikipedia.org/wiki/ASCII_art" target="_blank">ascii art</a> was a common thing.


<pre>
<code>

 __     __        _           _                     _ 
 \ \   / /       | |         | |                   | |
  \ \_/ /__  __ _| |__     __| | __ ___      ____ _| |
   \   / _ \/ _` | '_ \   / _` |/ _` \ \ /\ / / _` | |
    | |  __/ (_| | | | | | (_| | (_| |\ V  V / (_| |_|
    |_|\___|\__,_|_| |_|  \__,_|\__,_| \_/\_/ \__, (_)
                                               __/ |  
                                              |___/   
</code>
</pre>

or you can highlight like this:

{% highlight bash %}
 __     __        _           _                     _ 
 \ \   / /       | |         | |                   | |
  \ \_/ /__  __ _| |__     __| | __ ___      ____ _| |
   \   / _ \/ _` | '_ \   / _` |/ _` \ \ /\ / / _` | |
    | |  __/ (_| | | | | | (_| | (_| |\ V  V / (_| |_|
    |_|\___|\__,_|_| |_|  \__,_|\__,_| \_/\_/ \__, (_)
                                               __/ |  
                                              |___/   
{% endhighlight %}
<br>


# Pokemon Ascii Avatars!
I had a simple goal - to create a command line based avatar generator that I could use in my application. Could there be any cute, sometimes scheming characters that be helpful toward this goal? Pokemon!! Of course :) Thus, the idea for the pokemon ascii avatar generator was born. If you want to skip the fluff and description, <a href="https://github.com/vsoch/pokemon-ascii" target="_">here is pokemon-ascii</a>.

### Generate a pokemon database
Using the <a href="http://pokemondb.net/pokedex/national" target="_blank">Pokemon Database</a> I wrote a <a href="https://github.com/vsoch/pokemon-ascii/blob/master/scripts/make_db.py" target="_blank">script</a> that produces a <a href="https://raw.githubusercontent.com/vsoch/pokemon-ascii/master/pokemon/database/pokemons.json" target="_blank">data structure</a> that is stored with the module, and makes it painless to retrieve meta data and the ascii for each pokemon. The user can optionally run the script again to re-generate/update the database. It's quite fun to watch!


<div>
    <img src="https://github.com/vsoch/pokemon-ascii/raw/master/img/generation.gif" style="width:1000px"/>
</div><br>

The Pokemon Database has a unique ID for each pokemon, and so those IDs are the keys for the dictionary (the json linked above). I also store the raw images, in case they are needed and not available, or (in the future) if we want to generate the ascii's programatically (for example, to change the size or characters) we need these images. I chose this "pre-generate" strategy over creating the ascii from the images on the fly because it's slightly faster, but there are definitely good arguments for doing the latter.

<div>
    <img src="/assets/images/posts/pokemon-ascii/pokemon.png" style="width:1000px"/>
</div><br>


### Method to convert image to ascii

I first started with my own intuition, and decided to read in an image using the Image class from PIL, converting the RGB values to integers, and then mapping the integers onto the space of ascii characters, so each integer is assigned an ascii. I had an idea to look at the number of pixels that were represented in each character (to get a metric of how dark/gray/intense) each one was, that way the integer with value 0 (no color) could be mapped to an empty space. I would be interested if anyone has insight for how to derive this information. The closest thing I came to was determining the number of bits that are needed for different data types:

<pre>
<code>
# String
"s".__sizeof__()
38

# Integer
x=1
x.__sizeof__()
24

# Unicode
unicode("s").__sizeof__()
56

# Boolean
True.__sizeof__()
24

# Float
float(x).__sizeof__()
24
</code>
</pre>

Interesting, a float is equivalent to an integer. What about if there are decimal places?

<pre>
<code>
float(1.2222222222).__sizeof__()
24
</code>
</pre>

Nuts! I should probably not get distracted here. I ultimately decided it would be most reasonable to just make this decision visually. For example, the `@` character is a lot thicker than a `.`, so it would be farther to the right in the list. My first efforts rendering a pokemon looked something like this:

<div>
    <img src="https://vsoch.github.io/assets/images/posts/pokemon-ascii/attempt1.png" style="width:1000px"/>
</div><br>

I then was browsing around, and found a <a href="https://www.hackerearth.com/notes/beautiful-python-a-simple-ascii-art-generator-from-images/" target="_blank">beautifully done implementation</a>. The error in my approach was not normalizing the image first, and so I was getting a poor mapping between image values and characters. With the normalization, my second attempt looked much better:

<div>
    <img src="https://vsoch.github.io/assets/images/posts/pokemon-ascii/attempt2.png" style="width:1000px"/>
</div><br>

I ultimately modified this code sightly to account for the fact that characters tend to be thinner than they are tall. This meant that, even though the proportion / size of the image was "correct" when rescaling it, the images always looked too tall. To adjust for this, I modified the functions to adjust the new height by a factor of 2:

<pre>
<code>
def scale_image(image, new_width):
    """Resizes an image preserving the aspect ratio.
    """
    (original_width, original_height) = image.size
    aspect_ratio = original_height/float(original_width)
    new_height = int(aspect_ratio * new_width)

    # This scales it wider than tall, since characters are biased
    new_image = image.resize((new_width*2, new_height))
    return new_image
</code>
</pre>

Huge thanks, and complete credit, goes to the author of the original code, and a huge thanks for sharing it! This is a great example of why people should share their code - new and awesome things can be built, and the world generally benefits!
