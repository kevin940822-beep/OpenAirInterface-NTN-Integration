# Key Points

# I. INTRODUCTION
- Is a user guide of IEEEtran LATEX.
- IEEEtran is designed for typesetting IEEE journals, conferences, and technical reports.
- More complex usage techniques can be found in {bare_adv.tex}.

# II. CLASS OPTIONS
## A. 9pt, 10pt, 11pt, 12pt
- There are four possible values for the normal text size.

9pt : technote papers

10pt : vast majority of papers

11pt : some conferences

- IEEE Computer Society publications use “PostScript” (i.e., “big point”, bp) point sizes.

(i.e., 72bp = 1in) 
- Not the traditional typesetters’ point

(i.e., 72.27pt = 1in). 

##  B. draft, draftcls, draftclsnofoot, final
- IEEEtran provides for three draft modes as well as the normal final mode.

### The draft modes
- provide a larger (double) line spacing to allow for editing comments as well as one inch margins on all four sides of the paper.
- it will puts every package used in the document into draft mode.
- this has the effect of disabling the rendering of figures.

### draftcls option 
- It will be confined within the IEEEtran class so that figures will be included as normal.
- Like draftcls
- Not display the word “DRAFT” along with the date at the foot of each page.

## C. conference, journal, technote, peerreview, peerreviewca
- IEEEtran offers five major modes to encompass conference.
#### conference
#### journal
#### correspondence (brief/technote) 
#### peer review papers

### Journal and technote modes
- produce papers very similar to those that appear in many IEEE TRANSACTIONS journals.
- When using technote, most users should also select the 9pt option.

### The peerreview mode
- Produces a single-column cover page (with the title, author names and abstract) to facilitate anonymous peer review.
- Papers using the peer review options require an {\IEEEpeerreviewmaketitle} command

 (in addition to and after the traditional {\maketitle}) 
