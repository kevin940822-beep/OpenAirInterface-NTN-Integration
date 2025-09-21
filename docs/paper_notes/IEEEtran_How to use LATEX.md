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

### #The draft modes
- provide a larger (double) line spacing to allow for editing comments as well as one inch margins on all four sides of the paper.
- it will puts every package used in the document into draft mode.
- this has the effect of disabling the rendering of figures.

### #draftcls option 
- It will be confined within the IEEEtran class so that figures will be included as normal.

### #draftclsnofoot
- Like draftcls
- Not display the word “DRAFT” along with the date at the foot of each page.

## C. conference, journal, technote, peerreview, peerreviewca
- IEEEtran offers five major modes to encompass conference.
#### conference
#### journal
#### correspondence (brief/technote) 
#### peer review papers

### #Journal and technote modes
- produce papers very similar to those that appear in many IEEE TRANSACTIONS journals.
- When using technote, most users should also select the 9pt option.

### #The peerreview mode
- Produces a single-column cover page (with the title, author names and abstract) to facilitate anonymous peerreview.
- Papers using the peerreview options require an {\IEEEpeerreviewmaketitle} command

 (in addition to and after the traditional {\maketitle}) 
- to be executed at the place the cover page is to end—usually just after the abstract.

## #Conference Mode Details
### Page layout and margins
- The text height is reduced to about 9.25 inches.
- the top and bottom margins are asymmetrical.

the bottom margin is larger (IEEE requires additional space at the bottom).

- Text height is slightly adjusted to the normal font size to ensure a perfect number of lines within the column.
- Horizontal margins are symmetrical, and headers and footers do not display titles or page numbers.
- The difference between the one and two sided options will not be a noticeable.

### The \author text
- Is placed within a tabular environment to allow for multicolumn formatting of author names and affiliations.
- The spacing after the authors’ names is reduced. So is the spacing around the section names.
- The special paper notice (if used) will appear between the author names and the title (not after as with journals).
- The figure captions are centered.
- The following commands are intentionally disabled:

\thanks , \IEEEPARstart , \IEEEbiography , \IEEEbiographynophoto , \IEEEpubid , \IEEEpubidadjcol , \IEEEmembership , \IEEEaftertitletext.
- If needed, they can be reenabled by issuing the command: {\IEEEoverridecommandlockouts.}
- Various reminder (related to camera ready work) and warning notices are enabled.

## D. comsoc, compsoc, transmag
### #Comsoc Mode
- only affects the math font so that it will more closely match the Times Roman main text.
- The recommended loading procedure and order for newtx math is:

#### \usepackage[T1]{fontenc} % optional
#### \usepackage{amsmath}
#### \usepackage[cmintegrals]{newtxmath}
#### \usepackage{bm} % optional

### #Compsoc Mode
- The default text font is changed from Times Roman to Palatino/Palladio.
- Arabic section numbering.
- Enabling of the \IEEEtitleabstractindextext command to provide for single column abstract and index terms.

### #Transmag Mode
- The text within \author should be entered as the long form under conference mode.
- Enabling of the \IEEEtitleabstractindextext command -> provide for single column abstract and index terms.
- \IEEEauthorrefmark will produce arabic author affiliation symbols.
- Subsection and subsubsection headings and/or their spacings are slightly different.
- A smaller, bold font than normal is used for the title.

## E. letterpaper, a4paper, cspaper
- Usually choose letterpaper (8.5 × 11 inches, US standard, IEEE default)
- Authors should ensure that all post-processing (PS, PDF, etc.) uses the same paper specification as the .tex document.

## F. oneside, twoside
- These options control whether the layout follows that of single sided or two sided printing.

## G. onecolumn, twocolumn
- IEEE always uses two column text.
- These options allow the user to select between one and two column text formatting.

##  H. romanappendices
- Invoke this option to get Roman numbering.

##  I. captionsoff
- This option will inhibit the display of captions within figures and tables.

##  J. nofonttune
-  The nofonttune option will disable the adjustment of these font parameters.

# III. THE CLASSINPUT, CLASSOPTION AND CLASSINFO CONTROLS
##  A. CLASSINPUTs
- Are inputs that provide a way to customize the operation of IEEEtran by overriding some of the default settings.
### Can be set via the traditional LATEX interface
- \CLASSINPUTbaselinestretch -> sets the line spacing of the document.
- \CLASSINPUTinnersidemargin -> sets the margin at the inner (binding) edge.
- \CLASSINPUTinnersidemargin -> sets the margin at the inner (binding) edge.
- \CLASSINPUTtoptextmargin   -> sets the top margin.
- \CLASSINPUTbottomtextmargin -> sets the bottom margin.

## B. CLASSOPTIONs
- Are primarily TEX \if conditionals that are automatically set based on which IEEEtran options are being used. \ifCLASSOPTIONconference
 \typeout{in conference mode}

 \else
 
 \typeout{not in conference mode}
 
 \fi
- Can be used to provide for conditional code execution.
- Treat the CLASSOPTIONs as being “read-only”.
- Changing these flags will likely result in improper formatting.

## C. CLASSINFOs
- Provide document information (line spacing, whether it is PDF, paper size, etc.).
- \CLASSINFOnormalsizebaselineskip       ->  \baselineskip of the normalsize font.
-  \CLASSINFOnormalsizeunitybaselineskip -> \baselineskip  of the normalsize font under unity \baselinestetch.
-  \CLASSINFOpaperwidth and \CLASSINFOpaperheight  -> The paper dimensions in their native specifications including units (e.g., 8.5in, 22mm, etc.).

# IV. THE TITLE PAGE
- The parts of the document unique to the title area are created using the standard LATEX command.

## A. Paper Title
- Is declared like: \title{A Heuristic Coconut-based Algorithm} in the standard LATEX manner.
- Titles are generally capitalized.
- Line breaks (\\) may be used to equalize the length of the title lines.

##  B. Author Names
- The name and associated information is declared with the \author command.
  
#### have to use a separate \thanks for each paragraph
- two % → Prevent the code line break on lines ending in a } from becoming an unwanted space.

##  C. Running Headings
- Declared with the \markboth{}{} command.
- The first argument contains the journal name information.
- The second contains the author name and paper title.

#### eg. \markboth{Journal of Quantum Telecommunications,˜Vol.˜1, No.˜1,˜January˜2025}{Shell \MakeLowercase{\text it{et al.}}: A Novel Tin Can Link}

- The \MakeLowercase{} command must be used to obtain lower case text. Because the text in the running headings is automatically capitalized.
- Page heading only for the odd number pages after the title page for two sided (duplex) journal papers.

## D. Publication ID Marks
- Can be placed on the title page of journal and technote papers via the \IEEEpubid{} command:

 #### \IEEEpubid{0000--0000/00\$00.00˜\copyright˜2015 IEEE}
 
- Because LATEX resets the text height at the beginning of each column. 
- so \IEEEpubidadjcol must be issued somewhere in the second column of the title page to prevent it from running into the publication ID.

## E. Special Paper Notices
- Can declare with:
####  \IEEEspecialpapernotice{(Invited Paper)}
- Sometimes we need to gain access to the space across both columns just above the main text. IEEEtran provides the command:

#### \IEEEaftertitletext{}

- Which can be used to insert text or to alter the spacing between the title area and the main text.

# V. ABSTRACT AND INDEX TERMS
- The first part of a paper after \maketitle.
- Placed within the abstract environment:

\begin{abstract}

We propose ...

\end{abstract}
-  Math, special symbols and/or citations should generally not be used in abstracts.
-  Journal and technote papers also have a list of key words (index terms) which can be declared with:

 \begin{IEEEkeywords}
 
 Broad band networks, quality of service, WDM.
 
 \end{IEEEkeywords}

- Here is a list of valid keywords from the IEEE.
#### http://www.computer.org/mc/keywords/keywords.htm.

#  VI. SECTIONS
- Declared in the usual LATEX fashion via

\section       -> use upper case Roman numerals.

\subsection    -> use upper case letters.

\subsubsection -> use Arabic numerals.

\paragraph     -> use lower case letters.
