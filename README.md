This provides a template for writing papers and reports in the
Trustworthy Systems group.  Use the new_paper.py program on
papers.keg.cse.unsw.edu.au to create a cloneable repo for a new paper or report.
See further down how to set up a new paper repo.

Please update the templates when appropriate.

## Document Styles

The repo contains several templates:
1. paper.tex:  for conference and journal papers
2. simple.tex: for simple, UNSW-branded documents (based on article.cls)
3.  report.tex: for UNSW-branded reports (based on report.cls)
4. thesis.tex: for UNSW undergraduate and HDR theses

In most cases you'll want (1). The difference between (2) and (3) is
that the latter is for lengthy documents (separate title page, table
of contents, chapters) while the former is for shorter documents where
such spacial overhead is not justified.

Note: The file dissertation-sheet.tex is not a stand-alone document,
it is included by thesis.tex if the HDR option is set.

## Creating Paper Repos

### Setting up the paper Git repo

Assuming the venue is "sp14" and the paper name is "covert", the Git
repository for the newly created directory is set up as follows:

$ ssh papers.keg.cse.unsw.edu.au

$ /usr/local/bin/new_paper.py

This will prompt for venue and paper name, as well as email addresses for
additional authors (you, the creator of the repository, will be included in
the email notification list automatically).

Please use lower-case only names for conferences/journals!
Please use a two-level hierarchy (venue/paper) for each paper.
Please end the venue name with a two-digit year!

It will create the repo in the Github au-ts-writings organisation and set up
a commit mailing list, and will let you know the clone command invocation to
use. The repository will be named papers_<conference>_<name>.

The au-ts-writings organisation has a webhook which will send push notifications
to the Trustworthy Systems infrastructure each time commits are made to
a repository. This webhook will be received on our end, and used to create
a notification email which is sent to the repository's commit mailing list.

The commit mailing list is maintained using mlalias, which is available
on papers.keg.cse.unsw.edu.au. If you add authors to the paper later, please
add their email addresses there. The mlalias name is `ts.papers_`_conference_`_`_name_.

You can specify much of the information using command line arguments if
you want; run

$ ./new_paper.py -h

to see what your options are.

### External Contributors

You can provide the emails for external contributors to the
`new_paper.py` script.  However, this does not give them access to the
repository.   To give someone access visit
https://github.com/au-ts-writings/papers_sp14_covert
click 'settings' in the middle near the top, then _Collaborators and
teams_ at the top left hand corner, then `Add people` in the middle at
the right.  In the dropdown you can enter the github userid or email
of the person you want to invite.

### Tidying Up

You will probably want to change the repository description on GitHub.
Click the 'cog' icon at the top right of the repository page, above the
description that says, `Paper automatically created by
au-ts-writings/bin/new_paper.py` 

### Configuring the repo

On your local machine, do:

$ cd working_dir
$ git clone git@github.com:au-ts-writings/papers_sp14_covert.git

where the latter is the clone incantation the script told you.

Run:

$ make

... and follow the instructions given!

In particular, edit the Makefile and choose the appropriate target,
and (if paper) chose the appropriate document style by enabling the correct
\if... The paper template contains a lot of conditional code to
satisfy requirements with different conference or paper styles. This
makes the source quite messy.

After setting up your repo you should **eliminate all the dead
(\iffalse) code** to make the document more readable.

**Also, remove unnecessary .tex, .cls, .sty, .bst files!**

Arguably, a better way to do this would be to have a style option in
the new_paper script and generate the simplified template for the
appropriate style. Feel free to implement!

### BibTeX

The setup assumes that you have a clone of the TS BibTeX
databases in your BIBINPUTS path (currently cloned from
ssh://cse.unsw.edu.au/~ts/git_root/bibtex).

**New references should be added to the group's .bib files.**

At make time, the build system will parse your targets for citations
and then collate all the BibTeX entries in references.bib.  This is
useful for several reasons. If you need to hand edit BibTeX entries to
save room in your paper, you don't need to edit the group bibfiles, or
make duplicates on entries in extras.bib. It also means that BibTeX
entries are stored with the paper in the repository.

New references should be added to the group's .bib files. Make sure
you checkout the group's BibTeX database (currently maintained under
Git in ssh://cse.unsw.edu.au//home/ts/git_root/bibtex) and have it in
your BIBINPUTS path.

If you are an external collaborating on a paper, you won't be able to
commit to this. The recommended procedure is to add any new references
to both, extra.bib and references.bib. The latter will ensure that you
get your cites into the paper, while the former ensures that if your
TS co-author rebuilds references.bib, the new references will not
get lost. One of the locals can then transfer from extra.bib to the
central database and rebuild references.bib.

Other than this special purpose, please only use extras.bib for
something that is not relevant to other papers, typically ephemeral
references to web sites, news articles etc.

The Makefile is smart. If you add or remove a reference to a target,
the build system will ask you whether you want to rebuild
references.bib. This is to ensure that if you have made changes by
hand to any references, these aren't overwritten.

The Makefile is also messy, but it does much more than latexmk...

## Submitting to arXiv

There's an "arxiv" target for making a clean version to submit to
arXiv. It uses the arxiv_latex_cleaner program from Google. Mac users
can install it with homebrew. Use cleaner_config.yaml to tell it extra
stuff to strip.
