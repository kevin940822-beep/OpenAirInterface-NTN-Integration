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

## A. Initial Drop Cap Letter
- The first letter of a journal paper. oversized letter is called “drop cap”.
- Can be accurately produced using the IEEEtran command:

\IEEEPARstart{}{}
- The first argument is the first letter of the first word.
- The second argument is the remaining letters of the first word.

#  VII. CITATIONS
- Are made with the \cite command as usual.
- IEEEtran will produce citation numbers that are individually bracketed in IEEE style.
- If the \cite with note has more than one reference, the note will be applied to the last of the listed references.

# VIII. EQUATIONS
- Equations are created using the traditional equation environment:

 \begin{equation}

 \label{eqn_example}
 
 x = \sum\limits_{i=0}^{z} 2^{i}Q
 
 \end{equation}


- which yields <img width="127" height="72" alt="image" src="https://github.com/user-attachments/assets/e675545b-4241-473c-bffb-9233497fe9e3" />

#  IX. MULTI-LINE EQUATIONS
- The most convenient and popular way to produce multiline equations is LATEX2 ’s eqnarray environment.
- eqnarray has several serious shortcomings:
  
Use 2 \arraycolsep for a column separation space does not provide natural math spacing in the default configuration.

Column definitions cannot be altered.

It is limited to three alignment columns.

Column alignment cannot be overridden within individual cells.

## Amsmath
- Contains many helpful tools.
- upon loading, amsmath will configure LATEX to disallow page breaks within multiline equations.
- To restore IEEEtran’s ability to automatically break within multiline equations, load amsmath like:

 \usepackage{amsmath}
 
 \interdisplaylinepenalty=2500

 # X. FLOATING STRUCTURES
 - Most IEEE journals strongly favor the positioning of floats to the top of the page.
 - IEEE journals never place floats in the first column of the first page.

## A. Figures
- Handled in the standard LATEX manner. For example:

 \begin{figure}[!t]
 
 \centering

 \includegraphics[width=2.5in]{myfigure}
 
 \caption{Simulation results for the network.}
 
 - The IEEE normally wants all of the lines left aligned.

\label{fig_sim}

\end{figure}

- Figures should be centered via the LATEX command:

\centering

- The caption follows the graphic.
- Any labels must be declared after (or within) the caption command.

### #Subfigures
- Can be obtain via the use of subfig packages.
- Subfig.sty package options are usually required to obtain IEEE compliant subfigure captions.
- This package depends on caption.sty. To prevent this, be sure to invoke subfig.sty’s option:

caption=false

- The recommended way to load subfig.sty is:

\ifCLASSOPTIONcompsoc
 
\usepackage[caption=false,font=normalsize,labelfon
t=sf,textfont=sf]{subfig}
 
\else
 
\usepackage[caption=false,font=footnotesize]{subfi
g}
 
\fi

## B. Algorithms
- The only floating structures IEEE uses are figures and tables.
- Do not use the floating algorithm environment of algorithm.sty or algorithm2e.sty.
- IEEEtran will not be in control of the (non-IEEE) caption style by the algorithm.sty or algorithm2e.sty float environments.

##  C. Tables
- IEEE places table captions before the tables.
- Table captions much like titles.
- Table captions usually use capitalized except for " a, an, and, as, at, but, by, for, in, nor, of, on, or, the, to and up "
- IEEE generally uses the standard text font, not the small caps font.

## D. Double Column Floats
- LATEX’s figure* and table* environments produce figures and tables that span both columns.
- Double column floats cannot be placed at the bottom of pages.
- That means “\begin{figure*}[!b]” will not normally work as intended.
- IEEE authors are warned not to use packages that allow material to be placed across the middle of the two text columns.

#  XI. LISTS
- IEEEtran provides enhanced IED list environments that make it much easier to produce IEEE style lists.
- The underlying \list remains the same as in traditional LATEX so as not to break code that depends upon it.
- IEEEtran uses a new length variable:

\IEEElabelindent
- So users can specify IED list structures directly in IEEE fashion.
- IED lists are controlled exclusively via two interfaces:

#### “global” control via the command:

\IEEEiedlistdecl 

#### “local” control via an optional argument that can be provided to \itemize, \enumerate, and \description.

## A. Itemize(列舉)
- Normally automatically calculate the width of whatever symbol the current list level is using, so we can call

\begin{itemize}...\end{itemize}

## B. Enumerate(枚舉)
- The \labelwidth will default to the length of "9" in the normal size and style.
- The width of the longest label will have to be manually specified if any of the following conditions are true:

A top level list has more than 9 items.
A relevant \labelenumX or \theenumX has been redefined.
\item[X] has been used to manually specify labels. 
The labels are using a font that is not the normal size and style.
The enumerated list is nested (i.e., not at the top level) and is therefore not using Arabic digits as labels.

## C. Description(描述)
- The longest label width will always have to be specified for description lists.

#  XII. THEOREMS AND PROOFS
- Axioms, corollaries and lemmas, are handled in the traditional LATEX fashion.
- Need to declare the structure name via the

\newtheorem{struct_type}{struct_title}[in_counter]

- struct_type is the user chosen identifier for the structure.
- struct_title is the heading that is used for the structure.
- in_counter is an optional name of a counter, the number will be displayed with the structure number, and whose update will reset the structure counter.

## A. Proofs
- Easily handled by the predefined IEEEproof environment:

\begin{IEEEproof}

.
 
.
 
\end{IEEEproof}

#  XIII. END SECTIONS
## A. Appendices
- The \appendix command is used to start a single appendix.
- After issuing \appendix, the \section command will be disabled and any attempt to use \section.
- The section counter is fixed at zero
- \appendices is used when there is more than one appendix section.
- Then use \section to declare each appendix:

 \section{Proof of the First Zonklar Equation}

## B. Acknowledgments(致謝)
- Acknowledgments and other unnumbered sections are created using the \section* command:

\section*{Acknowledgment}
 
\addcontentsline{toc}{section}{Acknowledgment}

## C. Bibliographies(參考書目)
- Using the IEEEtran BIBTEX package which is easily invoked via:

\bibliographystyle{IEEEtran}
 
\bibliography{IEEEabrv,mybibfile}

- When submitting the document source (.tex) file, strongly recommended that the BIBTEX .bbl file be manually copied into the document to prevent the possibility of changes occurring therein.

##  D. Biographies(簡歷)

\begin{IEEEbiography}[{\includegraphics[width=1in,height=1.25in,clip,keepaspectratio]{./shell}}]{MichaelShell}.. \end{IEEEbiography}

- The photo area is 1in wide and 1.25in long.
- Images should be of 220dpi (dots per inch) resolution and in gray scale with 8 bits/sample.

#  XIV. LAST PAGE COLUMN EQUALIZATION
- Balancing the last two columns is especially important for camera ready work.
- Use the manual approach by putting in \newpage at the appropriate point or \enlargethispage{-X.Yin} somewhere at the top of the first column of the last page.
- Where “X.Yin” is the amount to effectively shorten the text height of the given page. 
