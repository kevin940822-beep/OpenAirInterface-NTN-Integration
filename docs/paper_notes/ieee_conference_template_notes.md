# Key Points

## I. Introduction
- **Maintaining the Integrity of the Specifications**  
  Authors must ensure their papers comply with IEEE formatting standards and do not modify the integrity of the specifications.

---

## II. Prepare Your Paper Before Styling
- Write and save your paper content as a plain text file before formatting.  
- Complete all content and organizational editing before applying the template.  
- **Do not manually number section headings** — LATEX will do that automatically.  

### A. Abbreviations and Acronyms
- Do not use abbreviations in the title or headings unless absolutely unavoidable.  

### B. Units
- Use either **SI (MKS)** or **CGS** as the primary system of units.  
- Exception: English units may be used as identifiers in trade, e.g., *“3.5-inch disk drive”*.  
- Avoid mixing SI and CGS units.  
- If mixed units are required, clearly state the unit for each quantity in equations.  
- Do not write *“webers/m2”*; use **Wb/m²** or *“webers per square meter”*.  
- Spell out units when they appear in the text: *“a few henries”* not *“a few H”*.  
- Always use a leading zero before decimal points: **0.25**, not **.25**.  

### C. Equations
- Use a long dash (–) for minus signs, not a hyphen.  
- Punctuate equations with commas or periods if they are part of a sentence.  
- Number equations as **(1)**, not *“Eq. (1)”*.  

### D. LATEX
- Use *soft cross-references* such as `\eqref{Eq}` instead of hard references like *(1)*.  
- Do not use `{eqnarray}`; use **{align}** or **{IEEEeqnarray}** instead.  
- The `{subequations}` environment increments the main equation counter.  
- Do not assign the same label to both a subsection and a table — this may cause incorrect cross-references.  
- Avoid using `\nonumber` inside the `{array}` environment as it may suppress required equation numbers.  

### E. Some Common Mistakes
- The word *“data”* is plural, not singular.  
- Constants such as **µ₀** use zero as a subscript, not the lowercase letter “o”.  
- Use quotation marks properly, instead of bold or italic, for emphasis.  
- When highlighting with quotation marks, punctuation should appear **outside** the quotes.  
- Parenthetical remarks at the end of a sentence should be punctuated **outside** the closing parenthesis.
- The prefix “non” is not a word; it should be joined to the word it modifies, usually without a hyphen.
- The abbreviation “i.e.” means “that is”, and the abbreviation “e.g.” means “for example”.

###  F. Authors and Affiliations
- Author names should be listed starting from left.
- Names should not be listed in columns nor group by affiliation.

### G. Identify the Headings
 - If there are two or more sub-topics, the nextlevel head (uppercase Roman numerals).
 - If there are not at least two sub-topics, then no subheads should be introduced.

### H. Figures and Tables
Positioning Figures and Tables
- Place figures and tables at the top and bottom of columns.
- Avoid placing them in the middle of columns.
- Large figures and tables may span across both columns.
- Use the abbreviation “Fig. 1”, even at the beginning of a sentence.
#### ** Figure captions should be below the figures. **
#### ** table heads should appear above the tables. **

  
Figure Labels
- Use 8 point Times New Roman for Figure labels.
- Use words rather than symbols or abbreviations when writing Figure axis labels to avoid confusing the reader.
#### Notice : Do not label axes only with units.
- write “Magnetization (A/m)” or “Magnetization {A[m(1)]}”, not just “A/m” 
#### Notice : Do not label axes with a ratio of quantities and units.
- write “Temperature (K)”, not “Temperature/K”.
