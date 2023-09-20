# The Core Prelude

The Gerbil core prelude (`:gerbil/core`) implements the core Gerbil language.
This is the language you get in the interpreter and the default language for
file modules, unless you specify an alternate prelude with the `prelude:` directive.

## Prelude Dictionary 
- [Core Expander Syntax](core-expander-syntax.md)
  * [Top Forms](core-expander-syntax.md#top-forms)
    + [begin](core-expander-syntax.md#begin)
    + [begin-syntax](core-expander-syntax.md#begin-syntax)
    + [begin-annotation](core-expander-syntax.md#begin-annotation)
    + [import](core-expander-syntax.md#import)
    + [module](core-expander-syntax.md#module)
    + [export](core-expander-syntax.md#export)
    + [declare](core-expander-syntax.md#declare)
    + [include](core-expander-syntax.md#include)
    + [cond-expand](core-expander-syntax.md#cond-expand)
    + [provide](core-expander-syntax.md#provide)
    + [define-values](core-expander-syntax.md#define-values)
    + [define-syntax](core-expander-syntax.md#define-syntax)
    + [define-alias](core-expander-syntax.md#define-alias)
    + [extern](core-expander-syntax.md#extern)
  * [Expressions](core-expander-syntax.md#expressions)
    + [lambda%](core-expander-syntax.md#lambda%)
    + [case-lambda](core-expander-syntax.md#case-lambda)
    + [let-values letrec-values letrec*-values](core-expander-syntax.md#let-values-letrec-values-letrec-values)
    + [let-syntax letrec-syntax](core-expander-syntax.md#let-syntax-letrec-syntax)
    + [if](core-expander-syntax.md#if)
    + [quote](core-expander-syntax.md#quote)
    + [quote-syntax](core-expander-syntax.md#quote-syntax)
  * [Expander Hooks](core-expander-syntax.md#expander-hooks)
  * [Reserved Syntactic Tokens](core-expander-syntax.md#reserved-syntactic-tokens)
- [Prelude Macros](macros.md)
  * [Definition Forms](macros.md#definition-forms)
    + [define](macros.md#define)
    + [def](macros.md#def)
    + [def*](macros.md#def)
    + [defvalues](macros.md#defvalues)
    + [defsyntax](macros.md#defsyntax)
    + [defrules](macros.md#defrules)
    + [defalias](macros.md#defalias)
  * [Binding Forms](macros.md#binding-forms)
    + [let*-values](macros.md#let-values)
    + [let](macros.md#let)
    + [let*](macros.md#let)
    + [letrec letrec*](macros.md#letrec-letrec)
    + [lambda](macros.md#lambda)
    + [set!](macros.md#set)
  * [Common Syntactic Sugar](macros.md#common-syntactic-sugar)
    + [and or](macros.md#and-or)
    + [case cond](macros.md#case-cond)
    + [when unless](macros.md#when-unless)
    + [do do-while](macros.md#do-do-while)
    + [begin0](macros.md#begin0)
    + [rec](macros.md#rec)
    + [alet alet* and-let*](macros.md#alet-alet-and-let)
    + [@list](macros.md#list)
    + [quasiquote](macros.md#quasiquote)
    + [delay](macros.md#delay)
    + [cut](macros.md#cut)
    + [parameterize](macros.md#parameterize)
    + [let/cc let/esc](macros.md#letcc-letesc)
    + [unwind-protect](macros.md#unwind-protect)
    + [syntax-error](macros.md#syntax-error)
  * [MOP Macros](macros.md#mop-macros)
    + [defstruct-type defclass-type](macros.md#defstruct-type-defclass-type)
    + [defstruct define-struct](macros.md#defstruct-define-struct)
    + [defclass define-class](macros.md#defclass-define-class)
    + [defmethod](macros.md#defmethod)
    + [@method](macros.md#method)
    + [@](macros.md#slot-reference)
    + [@-set!](macros.md#-set2)
  * [Pattern Matching](macros.md#pattern-matching)
    + [match](macros.md#match)
    + [match*](macros.md#match)
    + [with with*](macros.md#with-with)
    + [?](macros.md#predicate-constructor)
    + [defsyntax-for-match](macros.md#defsyntax-for-match)
  * [Macros for Syntax](macros.md#macros-for-syntax)
    + [syntax-case syntax syntax/loc](macros.md#syntax-case-syntax-syntaxloc)
    + [syntax-rules](macros.md#syntax-rules)
    + [with-syntax with-syntax*](macros.md#with-syntax-with-syntax)
    + [identifier-rules](macros.md#identifier-rules)
  * [Module Sugar](macros.md#module-sugar)
    + [require](macros.md#require)
    + [defsyntax-for-import defsyntax-for-export defsyntax-for-import-export](macros.md#defsyntax-for-import-defsyntax-for-export-defsyntax-for-import-export)
    + [for-syntax for-template](macros.md#for-syntax-for-template)
    + [only-in](macros.md#only-in)
    + [except-in except-out](macros.md#except-in-except-out)
    + [rename-in rename-out](macros.md#rename-in-rename-out)
    + [prefix-in prefix-out](macros.md#prefix-in-prefix-out)
    + [struct-out](macros.md#struct-out)
    + [group-in](macros.md#group-in)
  * [Special Evaluation Forms](macros.md#special-evaluation-forms)
    + [eval-when-compile](macros.md#eval-when-compile)
