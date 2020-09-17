LaTeX Best Practices
=======================

This document intends to serve as a guide listing a set of best practices to write scientific papers using LaTeX. The rules are gathered either by experience, online search and most importantly the wisdom of my PhD supervisor [@Raphael Troncy](https://twitter.com/rtroncy).

## General Organizations

- Create a structure to include your papers in an organized fashion. I keep separate folders for `papers`, `posters`, `presentations`, `reports` and `thesis`. **Note**: Demo and research paper are included in the same folder, you can decide to have posters and demo in the same folder. It is up to you.
- To easily navigate through papers, it is good to have a separate folder for each conference (if you a large number of papers, and then from there you have another hierarchy with the year) but for me it is a flat structure inside each folder with a naming convention that follows `<Conference>'<Year> - <Paper Name>`.
- Keep a `util` folder in the root. This will help misc files that you need globally. For me, i have a `LaTeX` folder inside that includes all the document classes that i use across my papers. I then reference the class using relative path. This way, you will need to maintain different copies of the same files in each paper's folder. Also this folder will include a `Bibliography` folder containing a centralized `.bib` file. I will come to this in details after.
- In every paper folder, create a separate folder for your `figures` to include shapes, graphics and charts and `tables` to include `.xls, .csv, .lyx` files or any information related to creating tables.

#### An Example of my folders structure
```
├── Papers
│   ├── ESWC'13 - SNARC A Semantic Social News Aggregator
│   │   ├── figures
│   │   └── presentation
│   ├── IJSWIS - Towards An Objective Assessment Framework for Linked Data Quality
│   │   ├── figures
│   │   └── tables
│   ├── PROFILES'15 - An Extensible Framework to Validate and Build Dataset Profiles
│   │   └── figures
├── Posters
│   ├── ESWC'14 - What are the Important Properties of an Entity - Comparing Users and Knowledge Graph Point of View
│   │   └── poster
│   └── WWW'15 - Observing The State of Linked Open Data Cloud Metadata
│       └── figures
├── Presentations
├── Progress Reports CIFRE & Workplans
│   ├── CIFRE Acitivity Reports
│   ├── Conferences Attestations
│   ├── EDITE Progress Reports
│   ├── EURECOM PhD Scientific Council
│   │   ├── EURECOM PhD Day 2013
│   │   └── EURECOM PhD Day 2014
│   └── PhD Interim Report
│       ├── figures
│       └── presentation
└── Util
    ├── Bibliography
    └── LaTEX
```

### Special Organization

For special reports (or thesis), when you have multiple `.tex` files and things can get messy then you can:

- Create separate folder for each section (introduction, abstract, etc.). Each directory contain all the LaTeX code, and could have further subdirectories if required. All `.tex` files are included in the paper by adding `\input{directory/subdirectory/something.tex}` to your main `.tex` file in the appropriate place.
- The `Util` folder in this case will include additional files:
    + All package inclusions `\usepackage{}`, command declarations, etc. required for the paper are included in `include.tex`
    + Formatting and special stylings are included in `format.tex` file.

```
├── Thesis
│   ├── Part1
│   │   └── Chapter1
│   ├── abstract
│   ├── acknowledgement
│   ├── acronyms
│   ├── appendix
│   ├── background
│   ├── conclusions
│   ├── introduction
│   └── util
│       └── figures
        └── include.tex
        └── format.tex
```

## Environment Setup

- I cannot stress enough on the importance of using a versioning system like [`git`](http://git-scm.com/) or [`svn`](https://subversion.apache.org/). It is vital for collaborative authoring.
- My development machine is a Mac running OSX Yosemite. I use [MacTEX](https://tug.org/mactex/). MacTex is an all-in-one package installing the following:
    + GUI Applications: [TeXShop](http://www.uoregon.edu/~koch/texshop/texshop.html) , [BibDesk](http://bibdesk.sourceforge.net/), [LaTeXiT](http://ktd.club.fr/programmation/latexit_en.php), [TeX Live Utility](https://code.google.com/p/mactlmgr/), the spell checker [Excalibur](http://excalibur.sourceforge.net/), and some documentation
    + Ghostscript 9.10
    + TeX Live 2014, the actual TeX distribution

### Generating LaTeX Tables

-  A nice program that provides a GUI for various tasks is Lyx. I have not used it for Mac but it was useful when editing/creating tables on Windows. It can import data in `.csv` format and generate `.tex` code from it.
-  There is several online `.tex` generators. I use [the online LaTeX table generator](http://www.tablesgenerator.com/), and found it very useful for small/medium tables.
-  There is a handy Excel script to convert tables in Excel files to LaTeX. You can find it in `Util/Excel To LaTeX Table Converter.xla`

### Editing LaTeX with Sublime Text

I use [Sublime Text](https://www.sublimetext.com/) for nearly everything, so using it to write LaTeX was the logical choice for me. With the correct plugins it is a handy editor. You first need to make sure you have a valid TeX distribution (see above) and installed [Skim PDF Viewer](http://skim-app.sourceforge.net/). Skim is used to generate the result of the LaTeX build.

Assuming you already have [package control installed](https://packagecontrol.io/installation) install the [`LaTeX Tools`](https://github.com/SublimeText/LaTeXTools) package. Now, after editing a LaTeX document, simply hit `cmd+b` to build the file and launch it in Skim.

**Note** Make sure you have the build settings in Sublime Text set to automatic `[tools -> Build Systems]`.

Now you will be able to use latex in Sublime. A powerful feature is **auto-completion** for references and citations, just start typing `\cite{` or `\ref{` and you will get the list of targets in a dropdown list (where you can also filter) to select from.

## LaTeX Writing Tips

- Use as few packages as possible. That's because packages tend to conflict and go obsolete [check this](http://tex.stackexchange.com/questions/3910/how-to-keep-up-with-packages-and-know-which-ones-are-obsolete)
- Call the packages in particular order. Some packages require (or are recommended) to be called before/after other packages. Some must be called among the last, some among the first.
- Use comment blocks to identify each section
- Use labels always to refer to sections, figures or tables. Start your label for each with a unique string e.g. `fig:`, `sec:`, `tab:`. This is useful when the auto-complete suggests several entries, you can easily filter and find what you want exactly.
- Always use `\url` command to represent URLs.
- Use `~` to have a space between the last work and the reference or citation e.g. `in~cite{}`
-  In your LaTeX document, avoid as much as possible to control the layout. Hence, you should never use `\\` Either you want to start a new paragraph or you don't but you should let LaTeX format the paper.

### Writing-Style Tips

- When writing, try to use as much as possible the present tense (as opposed to the past tense). Most of the things you're writing are still true, so why using the past tense? This, generally, makes sentences which are much more complicated and hard to read.

## Working with Citations and `.bib` files

I used to have a separate `.bib` file for each paper. However, i ended up using a bunch of the same references in more than one paper. I thought of why not having a centralized `.bib` file where i can maintain and then reference this file from all my other papers. The idea is:

- There is a central `.bib` file containing just about every reference you (or your coauthors) have ever even thought of.
- You (and your various coauthors) have a variety of documents that refer to stuff in this central `.bib` file.
- You want to look in the `.aux` file for a list of references, then go to this `.bib` file, extracts the relevant entries, and then puts them in a local `.bbl` file ready for insertion in to the document for each paper.

This is done automatically by BibTex. So for me, in every paper now i only add `\bibliography{../../Util/Bibliography/bibliography}` and i am good to go.

### Maintaining a clean `.bib` file

I used to copy/paste BibTex citations from the Internet and though that i am good to go. However, the information copied is very noisy and contains lots of unneeded information. The best practice is to check what are the required information for each `bib` entry and include only that. For example, for the `@inproceedings` you need to have `[title, author, year, booktitle]`. In addition make sure to:

- Escape the accents and special characters in people names
- Try to always write "firstName lastName" and not "lastName, firstName" or an initial for the firstName or whatever ... all this is decided by the bib style (e.g. a .bst file). Use the "and" operator between authors and not the comma.
- Include the `title` between double curly braces e.g. `{{The best way to write .bib files}}`  This way the capitalization will be respected as you write. Otherwise, all words will be lower case, including acronyms, etc. Don't write a final dot in the title, the style will add one already.
- Have a consistent way of naming the `bib` entry. Choose whatever you like, for example i chose: `<Main_author_LastName>:<Conference/Venue>:<Year>` where the year is in two digits representation, e.g. `Assaf:ESWC:13`
- For `inproceedings` just write the conference name, and do not include the address, or month, or even the word **Proceedings** since the style might display it as well and you don't want to read "Proceedings of Proceedings of ..."
- If you write a URL, anywhere, then say it's a url using the `\url` command.
- Don't write journal abbreviation, or conference abbreviation, again the style can do this.
- If you use the article element, then specify the volume and number.
- Don't use the `incollection` element, this is often incorrect. In most of your references, those Springer volumes are just proceedings of conferences

### Some Useful Hacks

- Loading the [strict](http://texd.cvs.sourceforge.net/texd/latex/sty/strict.sty?revision=1.2&view=markup) package prevents using LaTeX's declarations as environments.
- Loading the [fixltx2e](http://ctan.org/pkg/fixltx2e) package fixes some LaTeX2e errors.
- [fixmath](http://ctan.org/pkg/fixmath) changes LaTeX's default math style to comply with some international standards, specifically regarding Greek letters, see package description.
- [refcheck](https://www.ctan.org/tex-archive/macros/latex/contrib/refcheck) for checking "references in a document, looking for numbered but unlabeled equations, for labels which are not used in the text, for unused bibliography references.






