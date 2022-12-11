Welcome to the final 

Part 3:
token_code. token -> symbol
1. real_literal -> {0-9}\*.{0-9}\*
2. natural_literal -> {0-9}\*
3. bool_literal -> True | False
4. char_literal -> '{a-zA-Z,.\'\";:\\!@#%^&*()_-=+`~<>?/\t\n\f\b}'
5. string_literal -> "{a-zA-Z,.\'\";:\\!@#%^&*()_-=+`~<>?/\t\n\f\b}\*"
6. selection_keyword_if -> if
7. selection_keyword_else -> else
8. iteration_keyword_while -> while
9. iteration_keyword_for -> for
10. iteration_keyword_do -> do
11. variable_keyword_var -> var
12. type_keyword_nat -> nat
13. type_keyword_bool -> bool
14. type_keyword_char -> char
15. type_keyword_string -> string
16. type_keyword_real -> real
17. addition_operator -> +
18. subtraction_operator -> -
19. multiplication_operator -> *
20. division_operator -> /
21. modulo_operator -> %
22. assignment_operator -> =
23. equality_operator -> ==
24. inequality_operator -> !==  
25. less_than_operator -> <
26. greater_than_operator -> >
27. less_than_or_equal_operator -> <==
28. greater_than_or_equal_operator -> >==
29. logical_and_operator -> &&
30. logical_or_operator -> ||
31. logical_not_operator -> !
32. exponentiation_operator -> **
33. left_parenthesis -> (
34. right_parenthesis -> )
35. left_brace -> {
36. right_brace -> }
37. negation_operator -> -
38. function_keyword -> function
39. identifier -> id
Part 4: 
<stmt> --> <block> | <if_stmt> | <assignment> | <empty>
<block> --> `{`{<stmt>}`}`
<loop> --> <while_loop> | <do_while> | <for_loop>
<if_stmt>   -->  `if``(`<bool_stmt> `)`<stmt>[`else ` <stmt>]
<do_while> --> `do` <block> <while_loop>
<while_loop> -->  `while``(`<bool_stmt>`)`<stmt>
<for_loop> --> `for``(`<bool_stmt>`)`<block>
<assignment> --> `var` <id> `=` <expr>
<functions> -- > `function` <id> `(` <id> `)` <block>
<expr> --> <term> {(`+`|`-`)<term>}
<term> --> <val>{(`*`|`/`|`%`|`\*\*`)<val>}
<val> --> <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)`
<bool_stmt> --> `True` | `False` | <expr> (`==`|`!==`|`<`|`>`|`<==`|`>==`) <expr> | `(` <bool_stmt> `)` | `!` <bool_stmt> | <bool_stmt> (`&&`|`||`) <bool_stmt> --> `True` | `False` | <expr> (`==`|`!==`|`<`|`>`|`<==`|`>==`|`&&`|`||`) <expr>
<empty> --> ``

Part 5:
<if_stmt>   -->  `if``(`<bool_stmt> `)`<stmt>[`else ` <stmt>]
M_if(`if``(`<bool_stmt> `)`<stmt>, s) -->
    if M_b(<bool_stmt>,s) == error
        return error;
    if M_b(<bool_stmt>,s)
        if M_stmt(<stmt>,s) == error
            return error
        return M_stmt(<stmt>,s)

M_if(`if``(`<bool_stmt> `)`<stmt1> `else ` <stmt2>, s) -->
    if M_b(<bool_stmt>,s) == error
        return error;
    if M_b(<bool_stmt>,s)
        if M_stmt(<stmt>,s) == error
            return error
        return M_stmt(<stmt1>,s)
    else 
        if M_stmt(<stmt2>,s) == error
            return error
        return M_stmt(<stmt2>,s)

Part 6: 
<while_loop> | <do_while> | <for_loop>
M_loop(<while_loop> | <do_while> | <for_loop>,s) --> 
    if M_while(`while``(`<bool_stmt>`)`<stmt>,s)
        if M_b(<bool_stmt>,s)
            if M_stmt(<stmt>,s) == error
                return error
            return M_stmt(<stmt1>,s)
    if M_do_while(`do` <block> <while_loop>, s)
        if M_block(`{`{<stmt>}`}`,s) == error
            return error
        else
            return M_loop(<while_loop>)
    if M_for_loop(`for``(`<bool_stmt>`)`<block>)
        if M_b(<bool_stmt>,s)
            if M_block(`{`{<stmt>}`}`,s) == error
                return error
            return M_block(<block>,s)

Part 7:
<expr> --> <term> {(`+`|`-`)<term>}
<term> --> <val>{(`*`|`/`|`%`|`\*\*`)<val>}
<val> --> <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)
M_expr(<term>)
    if M_term(<val>,s) == error
        return error
    else
        if M_term(<val>,s)
            if M_facotr(<val>,s) == error
                return error
            else
                if M_facotr(<val>,s) 
                    return <val>
M_expr(<term>)
    if M_term(<val>,s) == error
        return error
    else
        if M_term(<val[1]>(`*`|`/`|`%`|`\*\*`)<val[2]>,s)
            if M_val(<val1>,s) == error
                return error
            elif M_val(<val2>,s) == error
                return error
            else
                return <val1>, <val2>

M_expr(<term>)
    if M_term(<val>,s) == error
        return error
    else
        if M_term(<val[1]>(`*`|`/`|`%`|`\*\*`)<val[2]>,s)
            if M_val(<val1>,s) == error
                return error
            
            elif M_val(<val2>,s) == error
                return error
            else
                return <val1>, <val2>

M_expr(<term> (`+`|`-`) <term>)
    if M_term(<val>,s) == error
            return error
        else
            if M_term(<val>,s)
                if M_facotr(<val>,s) == error
                    return error
                else
                    if M_facotr(<val>,s) 
                        return <val>
M_expr(<term> (`+`|`-`) <term>)
    if M_term(<val>,s) == error
        return error
    else
        if M_term(<val[1]>(`*`|`/`|`%`|`\*\*`)<val[2]>,s)
            if M_val(<val1>,s) == error
                return error
            
            elif M_val(<val2>,s) == error
                return error
            else
                return <val1>, <val2>

Part 8:
<expr> --> <term> {(`+`|`-`)<term>}
<term> --> <val>{(`*`|`/`|`%`|`\*\*`)<val>}
<val> --> <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)
M_expr(<term>)
    if M_term(<val>,s) == error
        return error
    else
        if M_term(<val>,s)
            if M_facotr(<val>,s) == error
                return error
            else
                if M_val(<val>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                    return <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)

M_expr(<term>)
    if M_term(<val>,s) == error
        return error
    else
        if M_term(<val[1]>(`*`|`/`|`%`|`\*\*`)<val[2]>,s)
            if M_val(<val1>,s) == error
                return error
            elif M_val(<val2>,s) == error
                return error
            else
                if M_val(<val1>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                elif  M_val(<val2>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                return <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)


M_expr(<term> (`+`|`-`) <term>)
    if M_term(<val>,s) == error
            return error
        else
            if M_term(<val>,s)
                if M_facotr(<val>,s) == error
                    return error
                el
                    if M_val(<val>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                    return <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)

M_expr(<term> (`+`|`-`) <term>)
    if M_term(<val>,s) == error
        return error
    else
        if M_term(<val[1]>(`*`|`/`|`%`|`\*\*`)<val[2]>,s)
            if M_val(<val1>,s) == error
                return error
            
            elif M_val(<val2>,s) == error
                return error
            else
                if M_val(<val1>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                elif  M_val(<val1>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                return <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)

Part 9:
<assignment> --> `var` <id> `=` <expr>
<expr> --> <term> {(`+`|`-`)<term>}
<term> --> <val>{(`*`|`/`|`%`|`\*\*`)<val>}
<val> --> <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)`
Syntax: <assignment> --> var <id> = <expr>
Semantics:  <expr>.expected_type  <- <id>.actual_type

String + String does concatenation
a.  Syntax: <expr> --> <term[1]> + <term[2]>
    Semantics: <expr>.expected_type <- <term[1]>.actual_type + <term[2]>.actual_type
                <term[1]>.expected_type <- <val[1]>.actual_type
                <term[2]>.expected_type <- <val[2]>.actual_type
                if (<val[1]>.actual_type == string_literal) && (<val[2]>.actual_type == string_literal)
                    then strings are concatenated
                else 
                    <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal>

String * Natural repeats the Natural
b. Syntax: <expr> --> <term>  = <val[1]> * <val[2]>
    Semantics: <expr>.expected_type <- <term>.actual_type  <- <val[1]>.actual_type * <val[2]>.actual_type
                if (<val[1]>.actual_type == string) && (<val[2]>.actual_type == natural_literal)
                    then 
                    <var[3]> = string_literal <var[1]> is repeated <var[2]> times
                    return <var3>
                else 
                    <id> | <real_literal> | <bool_literal> | <char_literal>

Assign bool to natural is allowed
c. Syntax: <assignment> --> var <id> = <expr>
    Semantics: <expr>.expected_type <- <id>.actual_type
                if (<expr>.actual_type == bool_literal) && (<id>.actual_type == natural_literal)
                    then 
                    <id> = 1 if <expr> is true
                    <id> = 0 if <expr> is false
                else 
                    <id> | <real_literal> | <bool_literal> | <char_literal>

Assign bool to natural is allowed
d. Syntax: <assignment> --> var <id> = <expr>
    Semantics: <expr>.expected_type <- <id>.actual_type
                if (<expr>.actual_type == natural_literal) && (<id>.actual_type == bool_literal )
                    then 
                    <id> = true if <expr> is 1
                    <id> = false if <expr> is 0
                else 
                    <id> | <real_literal> | <string_literal> | <char_literal>
Assign char to natural is allowed
e. Syntax: <assignment> --> var <id> = <expr>
    Semantics: <expr>.expected_type <- <id>.actual_type
                if (<expr>.actual_type == char_literal) && (<id>.actual_type == natural_literal)
                    then 
                    <id> = ASCII value of <expr>
                else 
                    <id> | <real_literal> | <string_literal> | <bool_literal>
Assign natural to char is allowed
f. Syntax: <assignment> --> var <id> = <expr>
    Semantics: <expr>.expected_type <- <id>.actual_type
                if (<expr>.actual_type == natural_literal) && (<id>.actual_type == char_literal)
                    then 
                    <id> = ASCII character of <expr>
                else 
                    <id> | <real_literal> | <string_literal> | <bool_literal>
Assign natural to real is allowed
g. Syntax: <assignment> --> var <id> = <expr>
    Semantics: <expr>.expected_type <- <id>.actual_type
                if (<expr>.actual_type == natural_literal) && (<id>.actual_type == real_literal)
                    then 
                    <id> = <expr>
                else 
                    <id>  | <string_literal> | <bool_literal>
No other types are allowed to be assigned to others outside of their own
h. Syntax: <assignment> --> var <id> = <expr>
    Semantics: <expr>.expected_type <- <id>.actual_type
                if (<expr>.actual_type == <id>.actual_type)
                    then 
                    <id> = <expr>
                else 
                    error
Dividing by zero is an error
i. Syntax: <expr> --> <term[1]> / <term[2]>
    Semantics: <expr>.expected_type <- <term[1]>.actual_type / <term[2]>.actual_type
                <term[1]>.expected_type <- <val[1]>.actual_type
                <term[2]>.expected_type <- <val[2]>.actual_type
                if (<val[1]>.actual_type == real_literal) && (<val[2]>.actual_type == real_literal)
                    then 
                    if <val[2]> == 0
                        then error
                    else
                        <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)
                else 
                    <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)
Modulo operating by zero is an error
j. Syntax: <expr> --> <term[1]> % <term[2]>
    Semantics: <expr>.expected_type <- <term[1]>.actual_type % <term[2]>.actual_type
                <term[1]>.expected_type <- <val[1]>.actual_type
                <term[2]>.expected_type <- <val[2]>.actual_type
                if (<val[1]>.actual_type == real_literal) && (<val[2]>.actual_type == real_literal)
                    then 
                    if <val[2]> == 0
                        then error
                    else
                        <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)
                else 
                    <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)
Part 10: choose 3 syntactically valid assignment statements with at least 7 tokens to show these rules failing or passing semantic rules.
a. var x = 1 + 2 * 3 - 2
    semantics: <expr> -> <term> -> <val> -> <natural_literal> passes
    
b. var y =  "hello" + "world" + "!" 
    semantics: <expr> -> <term> -> <val> -> <string_literal> passes
c. var z = "hi" * 3 + "!" * 2
    semantics: <expr> -> <term> -> <val> -> <string_literal> passes
Part 11: Axiomatic Semantics ( find the weakest precondition) 
a.
a = 2 * (b - 1) - 1 
{a > 0}
2*(b-1) - 1 > 0
2*b - 2 > 0
b > 1
----------------------------------------
b.
if (x < y)
    x = x + 1
else
    x = 3 * x
{x < 0}

-1 < x  < y
x < 0 < y
----------------------------------------
c.
y = a * 2 * (b - 1) - 1
if (x < y)
    x = y + 1
else
    x = 3 * x
{x < 0}

-1 < y + 1  < a * 2 * (b - 1) - 1
-1 <  a * 2 * (b - 1) - 1 + 1  < a * 2 * (b - 1) - 1
-1 < a * 2 * (b - 1)  < a * 2 * (b - 1) - 1

x < 0 < a * 2 * (b - 1) - 1
----------------------------------------
d.
a = 3 * (2 * b + a);
b = 2 * a - 1
{b > 5}
2 * a - 1 > 5
2 * (3 * (2 * b + a)) - 1 > 5
6 * (2 * b + a) - 1 > 5

12 * b + 6 * a - 1 > 5