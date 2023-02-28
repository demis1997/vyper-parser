# vyper-parser

import ply.lex as lex
import ply.yacc as yacc

# Define the Vyper lexer
tokens = (
    'IDENTIFIER',
    'NUMBER',
    'STRING',
    'LPAREN',
    'RPAREN',
    'LBRACKET',
    'RBRACKET',
    'PERIOD',
    'COMMA',
    'COLON',
    'SEMICOLON',
    'PLUS',
    'MINUS',
    'TIMES',
    'DIVIDE',
    'EQUALS',
    'ASSIGN',
    'GT',
    'LT',
    'GTE',
    'LTE',
    'NOTEQUAL',
    'AND',
    'OR',
    'NOT',
)

# Token definitions
t_LPAREN = r'\('
t_RPAREN = r'\)'
t_LBRACKET = r'\['
t_RBRACKET = r'\]'
t_PERIOD = r'\.'
t_COMMA = r','
t_COLON = r':'
t_SEMICOLON = r';'
t_PLUS = r'\+'
t_MINUS = r'-'
t_TIMES = r'\*'
t_DIVIDE = r'/'
t_EQUALS = r'=='
t_ASSIGN = r'='
t_GT = r'>'
t_LT = r'<'
t_GTE = r'>='
t_LTE = r'<='
t_NOTEQUAL = r'!='
t_AND = r'&&'
t_OR = r'\|\|'
t_NOT = r'!'

# Ignored characters
t_ignore = ' \t'

# Define the IDENTIFIER token
def t_IDENTIFIER(t):
    r'[a-zA-Z_][a-zA-Z0-9_]*'
    return t

# Define the NUMBER token
def t_NUMBER(t):
    r'\d+'
    t.value = int(t.value)
    return t

# Define the STRING token
def t_STRING(t):
    r'"[^"]*"'
    t.value = t.value[1:-1]
    return t

# Error handling
def t_error(t):
    print("Illegal character '%s'" % t.value[0])
    t.lexer.skip(1)

# Define the Vyper parser
def p_statement_assign(p):
    'statement : IDENTIFIER ASSIGN expression SEMICOLON'
    print("Assign statement: %s = %s" % (p[1], p[3]))

def p_expression_binop(p):
    '''expression : expression PLUS expression
                  | expression MINUS expression
                  | expression TIMES expression
                  | expression DIVIDE expression'''
    print("Binary operation: %s %s %s" % (p[1], p[2], p[3]))

def p_expression_group(p):
    'expression : LPAREN expression RPAREN'
    p[0] = p[2]

def p_expression_number(p):
    'expression : NUMBER'
    p[0] = p[1]

def p_expression_identifier(p):
    'expression : IDENTIFIER'
    p[0] = p[1]

def p_error(p):
    print("Syntax error at '%s'" % p.value)

# Build the lexer and parser
lexer = lex.lex()
parser = yacc.yacc()

# Test the parser
input_str = '''
x = 5;
y = (x + 3) * 2;
'''
parser.parse(input_str)
