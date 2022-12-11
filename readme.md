Welcome to the final 

Part 3:
token_code. token -> symbol
1. real_literal -> {0-9}\*.{0-9}\*
2. natural_literal -> {0-9}\*
3. bool_literal -> True | False
4. char_literal -> '{a-zA-Z,.\'\";:\\!@#%^&*()_-=+`~<>?/\t\n\f\b}'
5. string_literal -> "{a-zA-Z,.\'\";:\\!@#%^&*()_-=+`~<>?/\t\n\f\b}\*"
6. selection_keyword_if -> if
6. selection_keyword_else -> else
7. iteration_keyword_while -> while
7. iteration_keyword_for -> for
8. iteration_keyword_do -> do
9. variable_keyword_var -> var
10. type_keyword_nat -> nat
11. type_keyword_bool -> bool
12. type_keyword_char -> char
13. type_keyword_string -> string
14. type_keyword_real -> real
16. addition_operator -> +
17. subtraction_operator -> -
18. multiplication_operator -> *
19. division_operator -> /
20. modulo_operator -> %
21. assignment_operator -> =
22. equality_operator -> ==
23. inequality_operator -> !==  
24. less_than_operator -> <
25. greater_than_operator -> >
26. less_than_or_equal_operator -> <==
27. greater_than_or_equal_operator -> >==
28. logical_and_operator -> &&
29. logical_or_operator -> ||
30. logical_not_operator -> !
31. exponentiation_operator -> **
32. left_parenthesis -> (
33. right_parenthesis -> )
34. left_brace -> {
35. right_brace -> }
36. negation_operator -> -
37. function_keyword -> function
38. identifier -> id
Part 4: 
<stmt> --> <block> | <if_stmt> | <assignment> | 
<block> --> `{`{<stmt>}`}`
<loop> --> <while_loop> | <do_while> | <for_loop>
<if_stmt>   -->  `if``(`<bool_stmt> `)`<stmt>[`else ` <stmt>]
<do_while> --> `do` <block> <while_loop>
<while_loop> -->  `while``(`<bool_stmt>`)`<stmt>
<for_loop> --> `for``(`<bool_stmt>`)`<block>
<assignment> --> `var` <id> `=` <expr>
<functions> -- > `function` <id> `(` <id> `)` <block>
<expr> --> <term> {(`+`|`-`)<term>}
<term> --> <factor>{(`*`|`/`|`%`)<factor>}
<factor> --> <val> {`**`<val>}
<val> --> <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)`
<bool_stmt> --> `True` | `False` | <expr> (`==`|`!==`|`<`|`>`|`<==`|`>==`) <expr> | `(` <bool_stmt> `)` | `!` <bool_stmt> | <bool_stmt> (`&&`|`||`) <bool_stmt>

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
<term> --> <factor>{(`*`|`/`|`%`)<factor>}
<factor> --> <val> {`**`<val>}
<val> --> <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)
M_expr(<term>)
    if M_term(<factor>,s) == error
        return error
    else
        if M_term(<factor>,s)
            if M_facotr(<val>,s) == error
                return error
            else
                if M_facotr(<val>,s) 
                    return <val>
M_expr(<term>)
    if M_term(<factor>,s) == error
        return error
    else
        if M_term(<factor1>(`*`|`/`|`%`)<factor2>,s)
            if M_factor(<val1>,s) == error
                return error
            elif M_factor(<val2>,s) == error
                return error
            else
                return <val1>, <val2>

M_expr(<term>)
    if M_term(<factor>,s) == error
        return error
    else
        if M_term(<factor1>(`*`|`/`|`%`)<factor2>,s)
            if M_factor(<val1>,s) == error
                return error
            
            elif M_factor(<val2>,s) == error
                return error
            else
                return <val1>, <val2>

M_expr(<term> (`+`|`-`) <term>)
    if M_term(<factor>,s) == error
            return error
        else
            if M_term(<factor>,s)
                if M_facotr(<val>,s) == error
                    return error
                else
                    if M_facotr(<val>,s) 
                        return <val>
M_expr(<term> (`+`|`-`) <term>)
    if M_term(<factor>,s) == error
        return error
    else
        if M_term(<factor1>(`*`|`/`|`%`)<factor2>,s)
            if M_factor(<val1>,s) == error
                return error
            
            elif M_factor(<val2>,s) == error
                return error
            else
                return <val1>, <val2>

Part 8:
<expr> --> <term> {(`+`|`-`)<term>}
<term> --> <factor>{(`*`|`/`|`%`)<factor>}
<factor> --> <val> {`**`<val>}
<val> --> <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)
M_expr(<term>)
    if M_term(<factor>,s) == error
        return error
    else
        if M_term(<factor>,s)
            if M_facotr(<val>,s) == error
                return error
            else
                if M_factor(<val>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                    return <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)

M_expr(<term>)
    if M_term(<factor>,s) == error
        return error
    else
        if M_term(<factor1>(`*`|`/`|`%`)<factor2>,s)
            if M_factor(<val1>,s) == error
                return error
            elif M_factor(<val2>,s) == error
                return error
            else
                if M_factor(<val1>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                elif  M_factor(<val2>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                return <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)


M_expr(<term> (`+`|`-`) <term>)
    if M_term(<factor>,s) == error
            return error
        else
            if M_term(<factor>,s)
                if M_facotr(<val>,s) == error
                    return error
                el
                    if M_factor(<val>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                    return <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)

M_expr(<term> (`+`|`-`) <term>)
    if M_term(<factor>,s) == error
        return error
    else
        if M_term(<factor1>(`*`|`/`|`%`)<factor2>,s)
            if M_factor(<val1>,s) == error
                return error
            
            elif M_factor(<val2>,s) == error
                return error
            else
                if M_factor(<val1>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                elif  M_factor(<val1>,s) 
                    if M_val(<id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)) == error
                        return error
                return <id> | <real_literal> | <natural_literal> | <bool_literal> | <char_literal> | <string_literal> | `(` <expr> `)

Part 9:

Part 10:
a.
b.
c.s

Part 11:
a.
b.
c.
d.