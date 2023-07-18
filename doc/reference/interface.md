# Interfaces

THe `:std/interface` module provides an implementation of Go-style
interfaces.

Interfaces declare the methods that an object must implement and
provide a constructor to package an object together with the method
implementations in a flat instance. This allows interface method
invocations to bypass lookup and directly dispatch the method.
Furthermore, once an interface has been instantiated for some class,
the system keeps a prototype instance in cache; this allows subsequent
instantiations to reduce to a single hash table lookup to the
prototype which is then cloned.

::: tip To use the bindings from this module:
```scheme
(import :std/interface)
```
:::


## When to use interfaces

If you are interacting with an object using methods and you make many
method invocations, you should consider using interfaces.

Here is an example:
```scheme
(interface Operation
  (start!)
  (apply x)
  (finish!))

(defstruct LinearAccumulator (state a b))

(defmethod {apply LinearAccumulator}
  (lambda (self x)
    (with ((LinearAccumulator state a b) self)
      (set! (LinearAccumulator-state self)
         (+ state (* a x) b)))))

(defmethod {start! LinearAccumulator}
  (lambda (self)
    (set! (LinearAccumulator-state self) 0)))

(defmethod {finish! LinearAccumulator}
  LinearAccumulator-state)


;; implementation using vanilla method dispatch
(def (fold-accumulator/vanilla acc iters)
  {start! acc}
  (for (x (in-range iters))
    {apply acc x})
  {finish! acc})

;; implementation using the Operation interface
(def (fold-accumulator/interface acc iters)
  (let (op (Operation acc))
    (Operation-start! op)
    (for (x (in-range iters))
      (Operation-apply op x))
    (Operation-finish! op)))

;; implementation using the _unchecked_ Operation interface
(def (fold-accumulator/unchecked-interface acc iters)
  (let (op (Operation acc))
    (&Operation-start! op)
    (for (x (in-range iters))
      (&Operation-apply op x))
    (&Operation-finish! op)))
```

And here are some timings for 1M iterations, after compiling the above code:
```scheme
> (import :tmp/interface-example)
> (def acc (LinearAccumulator #f 2 3))
> (time (fold-accumulator/vanilla acc 1000000))
(time (tmp/interface-example#fold-accumulator/vanilla acc (##quote 1000000)))
    0.040925 secs real time
    0.040926 secs cpu time (0.040926 user, 0.000000 system)
    no collections
    no bytes allocated
    no minor faults
    no major faults
1000002000000
> (time (fold-accumulator/interface acc 1000000))
(time (tmp/interface-example#fold-accumulator/interface acc (##quote 1000000)))
    0.018799 secs real time
    0.018789 secs cpu time (0.018789 user, 0.000000 system)
    no collections
    1952 bytes allocated
    no minor faults
    no major faults
1000002000000
> (time (fold-accumulator/interface acc 1000000))
(time (tmp/interface-example#fold-accumulator/interface acc (##quote 1000000)))
    0.016031 secs real time
    0.016032 secs cpu time (0.016032 user, 0.000000 system)
    no collections
    1072 bytes allocated
    no minor faults
    no major faults
1000002000000
> (time (fold-accumulator/unchecked-interface acc 1000000))
(time (tmp/interface-example#fold-accumulator/unchecked-interface acc (##quote 1000000)))
    0.015697 secs real time
    0.015698 secs cpu time (0.015698 user, 0.000000 system)
    no collections
    1072 bytes allocated
    no minor faults
    no major faults
1000002000000
> (time (fold-accumulator/unchecked-interface acc 1000000))
(time (tmp/interface-example#fold-accumulator/unchecked-interface acc (##quote 1000000)))
    0.015520 secs real time
    0.015520 secs cpu time (0.015520 user, 0.000000 system)
    no collections
    1072 bytes allocated
    no minor faults
    no major faults
1000002000000
```

As we can see the interface version is significantly faster, and the
difference is becomes even more pronounced with deep/wide class
hierarchies.

## Macros
### interface
```scheme
(interface id method-spec ...)
(interface (id mixin-id ...) method-spec ...)

method-spec:
 (method-name . lambda-formals)
```

This is the interface definition macro; it creates an interface with
name `id`, optionally mixing in the methods from existing interfaces
`mixin-id ...`.  The body of the interface consists of zero or more
method specifications, which is simply the name of the method and the
lambda formals of the method, omitting self.

The macro generates a syntax binding for the interface meta
information (which is also a constructor macro), a constructor, two
predicates (for direct instance, `<interface??` and interface
satisfaction check `is-<interface>?`), and method definitions for all
the interface methods with the name `<interface>-<method>`. Here
`<interface>` means the symbolic name of the interface.

Here is an example:
```scheme
(interface Operation
  (start!)
  (apply x)
  (finish!))

; => expands to the following definitions:

(defsyntax Operation ...)
(def Operation::t ...)
(def Operation::descriptor ...)
(def (make-Operation obj) ...)
(def (Operation? obj) ...)
(def (is-Operation? obj) ...)
(def (Operation-start! self) ...)
(def (Operation-apply self x) ...)
(def (Operation-finish! self) ...)
```


### interface-out
```scheme
(interface-out interface-id ...)
(interface-out unchecked: #t interface-id ...)
```

Export macro for exporting the symbols related to an interface.
By default, unchecked method stubs are not exported; you will need to
use the second form to also export them.