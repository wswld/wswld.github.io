---
layout: post
title: 'Humble Collection of Python Sphinx Gotchas: Part II'
date: 2015-04-03 14:26:19.000000000 +03:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories: []
tags:
- bash
- DIY
- documentation
- how-to
- python
- Sphinx
- static sites
meta:
  _edit_last: '10080708'
  geo_public: '0'
  _publicize_job_id: '11491912059'
author:
  login: wswld
  email: seva17@gmail.com
  display_name: wswld
  first_name: ''
  last_name: ''
---
### Gotcha 1: Release and Version

Sphinx makes a distinction between the release code and the version of the 
application. The idea is that it should look this way:

``` python
version = "4.0.4"
release = "4.0.4.rc.1"
```

Most project use a much simpler versioning convention, so they would probably 
do something like this:

``` python
version = "4.0.4"
release = "4.0.4"
```

I'd been doing this myself for some time, until I realized the `conf.py` file 
is a simple Python file (no shit!) and it is perfectly fine to do something 
like this:

``` python
version = "4.0.4"
release = version
```

Yeah, kinda obvious I know. I however missed this (though I've been putting 
much more complex code to `conf.py`) and [some people](
http://stackoverflow.com/questions/23470010/sphinx-does-not-change-version-number/23910848#23910848) 
did either. So, I'll just leave it here.

### Gotcha 2: Download as PDF

Sometimes a PDF download should be provided along with the hosted HTML version. 
It looks good and people can get a well-formatted file to use locally as they 
are trying to work with your API or product. In short: it could be done easily, 
using Python. I'm really pissed off at people, who know Python or Bash and they 
still keep asking, whether there is an *automatic* way of doing that. Well, it 
doesn't get more automatic than that:

``` sh
# generating latex and pdf
make latexpdf
# generating html
find 'index.rst' -print -exec sed -i.bak "s/.. &//g" {} \;
find 'index.rst' -print -exec sed -i.bak "s/TARGET/$UPTARGET/g" {} \;
VERSION="$(grep -F -m 1 'version = ' conf.py)";
VERSION="${VERSION#*\'}";
VERSION="${VERSION%\'*}"
sphinx-build -b html . ../$TARGET/$VERSION/
cp latex/$UPTARGET.pdf ../$TARGET/$VERSION/
```

Note, that there should be the following line, inserted into the `index.rst` in 
the example above:

```
.. &  `Download as PDF <TARGET.pdf>`_
```

Where `..` is the comment syntax, `&` is here to distinguish the line from 
usual comments and `TARGET` is replaced with `$UPTARGET` which is the upper 
case version of the project name and the default name of the .tex and .pdf 
files. It creates a relative link to the .pdf file, which is then copied to the 
exact same folder, where HTML output is located. I'm not going explain much 
about the variables, as their sources may differ. In my work I use a python 
script, with exact same principle (I figured bash example would be more 
universal) and it gets values of `$TARGET`, `$UPTARGET` and `$VERSION` from a 
JSON file with a list of targets (more on that in the next example). In the 
example above, I'm stripping values off the conf.py file. In fact you can use 
whatever input you wish, even pass the values as arguments. What I was trying 
to illustrate is the concept itself.

### Gotcha 3: Using Scripting to Organize the Sphinx Project as a Multiple Project Knowledge Base

Some of the companies, I've been working at had this huge array of active 
projects, that they wanted to present as a single site, or the whole variety of 
sites with the same theme, or the single site with PDF version for every first 
level subsection. Basically they wanted me to create a Sphinx-based knowledge 
base. Using a simple Python or Bash script there are ways to organize your 
project any way you want (we'll use Python this time as it's closer to what 
I've been using). We're going to create a site that automatically builds PDF 
version for each first level subsection (project) and puts it alongside the 
subsection's `index.html`. Basically this is a bit more complex variant of the 
previous example. 

Let's imagine we have a single Sphinx project with a couple first level 
sections corresponding to company's projects, for example: Foo and Bar (give me 
that medal for originality, yeah). Basically, your folder structure will look 
like this:

```
Acme
|  index.rst
|_ Foo
|  |_1.0.0
|    |_index.rst
|  |_1.1.0
|    |_index.rst
|
|_ Bar
   |_1.0.1
     |_index.rst
```

Yeah, we also have versions. I use the [following script](
https://gist.github.com/wswld/aa35821b2d76fba73cc1) for the projects with such 
layout. Don't worry, it only looks kinda big. The script is rather simple. 
Also, I've commented the hell out of it so that you could figure it all out. 

Note, that you also need to create a `targets.json` file in the root of your 
project, containing the following lines (assuming we're using the structure we 
agreed on in the beginning):

``` js
{
  "foo" : "Foo Foo",
  "bar" : "Barrington"
}
```

The file will tell the script of full project names and how they correspond to 
target names (folder names) in the structure. Also you will need to have a 
`temp.py` file containing only the info we need for PDF building with most of 
the target names and version numbers represented as variables for injection 
(yeah, I know this is hacky, but did't want to bother with imports, 
dependencies etc). First of all it should have `$VRSN` tags:

``` python
# The short X.Y version.
version = '&VRSN'
# The full version, including alpha/beta/rc tags.
release = '&VRSN'
```

It should also have tags in the LaTeX part of the settings:

``` python
latex_documents = [
  ('index', '&TRGT.tex', u'&UPTRGT',
   u'ACME', 'manual'),
]
```

Other than that `temp.py` may resemble your usual `conf.py`. The reason is that 
we use `conf.py` for HTML and it has preset version and project name values for 
the project as the whole. So we better distinguish between the file for 
injections and the main configuration file, so that they don't mess with each 
other. Note that if you'll need to add some additional parameters or a preamble 
to the LaTeX output, you should do that in `temp.py` as `conf.py` is not used 
for building PDFs at all.

If we prepare the project this way, the script should build PDF's for every 
subproject and put them to the subproject's HTML root. Ideally the HTML version 
could also be built separately for every subproject (for the right project 
name/version to appear for every subproject). This script is more of a proof of 
concept rather than out-of-the-box solution. However if you now understand 
Sphinx's capability to extension and automation, you may create projects of any 
complexity yourself.
