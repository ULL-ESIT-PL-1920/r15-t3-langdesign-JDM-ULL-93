# Diseño de Lenguajes (Reto)

Modifica la gramática corrigiendo los errores que veas, de manera que genere frases como estas:

1. `a[4][b+c][5] = c[2][3]`
2. `a.b.c = a[x]`
3. `a.b[x*y].d = c.d[3][1].e(temp)`

* Procura que exprese bien la precedencia de operadores (vigila la asignación)

* La variable `<value>` no se describe (y debería describirse)

Ejemplos:

[4,5,7][0]

b[a=2]

[x => x+1][0](4)

## Grammar

```ruby
<program> ::= <block>

<block> ::= '{' <statement>* '}'  // Modified Casiano

<statement> ::=
               <declaration> |
              "if" <parenthesis> <block> ("else" "if" <block>)* ('else' <block>)? |
              "while" <parenthesis> <block> |
              'function' <word> '(' <word> (',' <word>)* ')' <block> |
              <asign> ";"
              
<declaration> ::= 'var' WORD ('=' <expr>)?

<asign> ::= (<leftVal> '=')* <expr>

<leftVal> ::= WORD (. WORD | '[' <expr> ']' )*

<expr> ::= <term> (('==', '!=', '>', '>=', '<', '<=', '=') <term>)*

<term> ::= <sum> (('+', '-') <sum>)*

<sum> ::= <fact> (('*', '/') <fact>)*

//<fact> ::= <value> | WORD <apply> | <parenthesis> | <array> // Added by: Casiano
<fact> ::= <value> | <parenthesis> //Para que sea LL (las reglas que se invocan deben empezar siempre de forma distinta)

<apply> ::= '(' <expr> (',' <expr>)* ')'<apply> | '.'WORD<apply> | empty

<array> ::= '[' ']' | '[' <expr> (',' <expr> )*] // Added by Casiano

<parenthesis> ::= '(' <expr> ')'

<value> : (VALUE | WORD | <array>) (. WORD | '[' <expr> ']' | <apply>)*  
```



## Tokens

```Ỳacc
WORD = [a-zA-Z_]\w*
VALUE = STRING | NUMBER
STRING = "( [^"\\]   #any character except " and escape
           | \\.     #any character before an escape
           )*"
NUMBER = ([-+]?\d+     #entero
          (\.\d+)?    #flotante
          ([eE][-+]?\d+)?  #con exponente
          )
```
