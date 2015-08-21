2015-06-03: Experimenting with a "functional prolog" syntax

  semantics.fpl is an attempt at back-porting semantics.pl to a
  language where functional patterns can be used as in mercury or
  curry. It might be better to just use mercury or curry directly, but
  that would mean a complete rewrite so I'll try to start easy.

2016-05-26:

As an attempt to keep the semantics in Prolog syntax but still enable
a more paper-readable form I have started writing a converter from
ProLog (perhaps extended with some layout) to LaTeX.  As a side-effect
I learn about the intricate syntax (and possibly semantics) of Prolog.

The main file is labelled BNFC grammar in Prolog.cf.

TODO:
* Why are there so many similar operators: "=", "is", "==", "=..", "\\=", "\\==" ?
* Clean up the precedences and fix the pretty-printing of ";" to not break the line.

\\== - syntactic inequality (at a particular search state)
\\=  - not unifiable


The underlying aim is to produce
* a human-readable semantics and
* a machine-executable semantics
from the same source.

Currently we have a prolog version (../semantics.pl) which is at least
one "conversion step" away from he desired "logic specification"
(independent of pretty-printing / formatting). It should be possible
to "undo" a few of those steps.

Examples:
* semantics.pl:57: ap(K,ok) is supposed to be "normal function application" of a function K to the atom ok.

C[F@X] :- R   |->    C[Y] :- R, eval(ap(F,X),Y)

* semantics.pl:77: append(Vs,[V],Vs1) would be more readable if "Vs1" could be replaced by "Vs ++ [V]".

C[X++Y] :- R   |->   C[Z] :- R, append(X,Y,Z)


* semantics.pl:135: "N1 is N-1" should be replaced by mathcing on N-1.

C[N-M] :- R   |->   C[N1] :- R, N1 is N-M
C[N+M] :- R   |->   C[N1] :- R, N1 is N+M


All of those examples are using function evaluation in combination
with the logic programming search.

If we have a special syntax for function application (like f@x), we
could implement a source to source translation from "prolog+@" to just
"prolog" for running. But we would pretty-print "prolog+@" directly to
LaTeX for the paper.