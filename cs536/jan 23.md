---
id: jan 23
aliases: []
tags: []
---

# jan 23

Date created: 2025-01-23
Time created: 18:06

## about the course
- we will study compilers & programming languages
- we will understand how both work
- we will build a full compiler

```Always include '536' in subject line for emails```

## course mechanics

### exams (60%)
- midterm 1 (18%) Thursday, Feb 27 6:30 - 8pm
- midterm 2 (16%) Thursday, Mar 20 6:30 - 8pm
- final (26%)     Thursday, May 8 6:30 - 8pm

### programming assignments (40%)
- 6 programs: 5 + 7 + 7 + 7 + 7 + 7

### homework assignments
- 8 short assignments (optional, not graded)
- especially useful for content on exams, so I should do them

## what is a compiler?
- assume we have some source program, then compiler spits out a target program
- recognizer of language s (source)
- translator from s to t (target)
- doesn't need to compile to assembly
- is a program itself, written in some language h (host)
    - in 536 s = bach h = Java t = MIPS
- error msgs
- can be separated into a frontend and backend
    - intermediate representation, IR
        - allows for multiple frontends or backends
    - frontend
         - understand s, map s to IR
    - backend
        - translate intermediary to t

## overview of a typical compiler
- frontend
    1. s -> sequence of characters
    2. scanner (aka lexical analyzer), job is to 'recognize words' -> sequence of tokens
        - input - characters from source program
        - ouput - sequence of tokens (where tokens can potentially be associated with additional info)
        - actions
            - group characters into lexemes (tokens)
            - identify and ignore whitespace, comments, etc.
        - error handling
            - bad characters (e.g. # in Java)
            - unterminated strings (e.g. "Hello)
            - integer literals that are too large
    3. parser (aka syntax analyzer), recognize tokens -> abstract syntax tree (AST)
        - input - sequence of tokens from the scanner
        - output - AST
        - actions
            - group token into sentences
        - error handling
            - syntax errors (e.g. x = y = 5;)
            - (possibly) static semantic errors (e.g. undeclared variables)
    4. semantic analyzer, name analysis (e.g. recognizing keywords), type checking -> adds to AST (augmented AST)
        - input - AST
        - output - annotated AST
        - actions
            - does more semantic checks
            - name analysis
                - process declarations and uses of identifiers + matches them up
                    - enforces the scoping rules (e.g. are nested scopes allowed?)
                - errors
                    - multiply declared variables, uses of undeclared variables
            - type checking
                - check types and augment AST by adding types to things that aren't identifiers or literals
- symbol table (not in the order, but necessary)
    - helps with name analysis
    - e.g. ID kind type
    - uses in the whole compiler
        - semantic analyzer
            - both name analysis and type checking
        - code generation
            - offsets into stack
        - optimizer
            - keep track of definition and use information
    - in a block structured language
        - we need multiple symbol tables
        - often represented as list of hashtables, where each hashtable is a symbol table

- intermediate code generator
    - input - annotated AST (assumes no syntax/semantic errors)
    - output - intermediate representation (IR)
         - e.g. 3 address code
            - instructions have at most 3 operands
            - easy to generate from an AST
                - 1 instruction per AST internal node
- backend
    1. optimizer (can be wherever really, not really necessary)
        - input - IR
        - output - optimized IR
        - actions - improve code
            - make it faster/smaller
            - several passes: local or global optimization
                - e.g. local = looking at a few instructions at a time
                - e.g. gloabl = looking at entire function or program (e.g. loops that never happen)
            - more time spent in compilation, less time in execution
    2. code generator, accepts optimized IR -> code in language t
        - input - IR, optimized or not
        - output - code in target t

- class example
    - ```a = 2 * b + abs(-71);```
    - scanner produces tokens
        - ```ID(a) ASSIGN INTLIT(2) TIMES ID(b) PLUS ID(abs) LPAREN MINUS INTLIT(71) RPAREN SEMICOLON```
    - parser produces AST
        - ASSIGN
            - ID - a
            - PLUS
                - MULT
                    - INTLIT - 2
                    - ID - b
                - CALL
                    - ID - abs
                    - NEG
                        - INTLIT - 71
    - symbol table
        - ID
            - a
            - b
            - abs
        - kind
            - var
            - var
            - func
        - type
            - int
            - int
            - int -> int
    - 3 address code (sort of works backwards)
        - temp1 = 2 * b
        - temp2 = 0 - 71
        - move temp2 param1
        - call abs
        - move return1 temp3
        - temp4 = temp1 + temp3
        - a = temp4







