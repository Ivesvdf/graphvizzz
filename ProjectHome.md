A small LaTeX package which allows inline usage of graphviz code (at the moment only dot graphs & digraphs).

# Installation #
Either copy the graphvizzz.sty file into the folder that contains your other tex files or put it somewhere your LaTex distribution will find it (no info on that, sorry!).

# Usage #
Put `\usepackage{graphvizzz`} in your preamble and use the
```
\digraph
[scale=1.0]{nameOfGraph}
{
   q0->q1; 
}
```
command to insert a digraph. This will generate a nameOfGraph.dot and nameOfGraph.pdf file so make sure this name is unique! The [scale=1.0] can be any \includegraphics parameter.

A complete example:
```
\title{Graphvizzz test}
\author{Ives van der Flaas}
\date{\today}

\documentclass[11pt]{article}
\usepackage{graphicx}
\usepackage{graphvizzz}  % <---- This needs to be included

\begin{document}
\maketitle

\section{Oefening 1}

%% Start of dot diagram
\digraph
[scale=0.7]{g1}
{
   margin="0 0 0 0";
   rankdir="LR";
   q0->q1->q2->q3->q0;
}
%% End of dot diagram

\end{document}
```

# FAQ #
**How do I make a non-directional graph instead of a directional one?**
Use \graph instead of \digraph (syntax is the same).

**An error instead of my graph is being rendered, why is this?**
Try opening a command window and type `dot --help`. If this command is not recognised, dot needs to be added to your path.

**An error instead of my graph is being rendered and the log mentions something about access not allowed. Why is this?**
This is probably because your LaTeX installation is not allowed to execute programs (dot in our case). To fix this; see Enable Write18.

# Enable Write18 #
Because an executable (dot) needs to be started, write18 needs to be enabled.

For MikTeX users:
write18 allows calling external programs from within TeX. It's not a good idea to have it
enabled on a server, but for comfortable work with ConTeXt (for MetaFun or modules for R    and gnuplot) you probably need it. Each application now has it's own configuration file. To enable write18 run` initexmf --edit-config-file=miktex\config\pdftex.ini`
> and put
` EnableWrite18=t`


For non-miktex users:
http://wiki.contextgarden.net/Write18#Windows_-_MikTeX


# The Path #
Make sure dot is in your %PATH% or $PATH.



# The Source #
```
% graphvizzz.sty
% Ives van der Flaas 2010
% ives.vdf@gmail.com
% http://code.google.com/p/graphvizzz/
%
% Based on graphviz.sty by Mark Aufflick 2006-03-25
% Based on graphviz.tex by Derek Rayside 2003

% add the following lines to your preamble:

% \usepackage[pdftex]{graphicx}
% \usepackage{graphvizzz}

% parameters to \digraph:
% 1 - parameters for \includegraphics (optional; default value is "scale=1")
% 2 - name of the digraph
% 3 - body of the digraph

\ProvidesPackage{graphvizzz}


\newcommand{\digraph}[3][scale=1]{ 
  \newwrite\dotfile 
  \immediate\openout\dotfile=#2.dot 
  \immediate\write\dotfile{digraph #2 {\string#3}} 
  \immediate\closeout\dotfile
  \immediate\write18{dot -Tpdf #2.dot > #2.pdf}
  \IfFileExists{#2.pdf}
  % the pdf exists: include it 
  { \includegraphics[#1]{#2} } 
  % the pdf was not created - show a hint
  {   
      The PDF file displaying your dot graph could not be created. 
      This usually means one of
      two things:
        \begin{itemize}
        \item dot is not in your path
        \item write18 is not enabled
        \end{itemize}
        Please check http://code.google.com/p/graphvizzz/ for more
        information. 
      
  } 
}

\newcommand{\graph}[3][scale=1]{ 
  \newwrite\dotfile 
  \immediate\openout\dotfile=#2.dot 
  \immediate\write\dotfile{graph #2 {\string#3}} 
  \immediate\closeout\dotfile
  \immediate\write18{dot -Tpdf #2.dot > #2.pdf}
  \IfFileExists{#2.pdf}
  % the pdf exists: include it 
  { \includegraphics[#1]{#2} } 
  % the pdf was not created - show a hint
  {   
      The PDF file displaying your dot graph could not be created. 
      This usually means one of
      two things:
        \begin{itemize}
        \item dot is not in your path
        \item write18 is not enabled
        \end{itemize}
        Please check http://code.google.com/p/graphvizzz/ for more
        information. 
      
  } 
}
```