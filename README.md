# Prueba-de-precedencia-y-asociatividad-ANTLR

Programa en ANTLR ' caluladora ejemplo ' tiene una asociatividad por izquierda y la precedencia normal en donde se puedan a hacer operaciones matemáticas usando visitor.

Para realizar pruebas de precedencia y asociatividad. se re diseña la gramática y cambiando la precedencia y asocitividad. R

en el archivo LabeledExpr.g4 :

  caso 1 (funcionamiento de una calculadora normal)
  
  grammar LabeledExpr; // rename to distinguish from Expr.g4

  quí la asociatividad es izquierda y la precedencia normal:
* y / > + y -. 

      prog:   stat+ ;
      
      stat:   expr NEWLINE                # printExpr
          |   ID '=' expr NEWLINE         # assign
          |   NEWLINE                     # blank
          ;
      
      expr:   expr op=('*'|'/') expr      # MulDiv
          |   expr op=('+'|'-') expr      # AddSub
          |   INT                         # int
          |   ID                          # id
          |   '(' expr ')'                # parens
          ;
      
      MUL :   '*' ; // assigns token name to '*' used above in grammar
      DIV :   '/' ;
      ADD :   '+' ;
      SUB :   '-' ;
      ID  :   [a-zA-Z]+ ;      // match identifiers
      INT :   [0-9]+ ;         // match integers
      NEWLINE:'\r'? '\n' ;     // return newlines to parser (is end-statement signal)
      WS  :   [ \t]+ -> skip ; // toss out whitespace
  


<img width="715" height="213" alt="image" src="https://github.com/user-attachments/assets/4f85dd00-f533-46d9-a305-82e2fc24122d" /> 

caso 2 (cambio en el orden de precedencia)
    
    expr
        : expr ('+'|'-') expr           # AddSub   // MÁS fuerte ahora
        | expr ('*'|'/') expr           # MulDiv   // MENOS fuerte
        | '(' expr ')'                  # parens
        | INT                           # int
        | ID                            # id
        ;


<img width="585" height="217" alt="image" src="https://github.com/user-attachments/assets/a34a56d1-7256-432d-9ec2-035bf9db181c" /> 

caso 3 (asociatividad por derecha) 

    expr
        : <assoc=right> expr op=('+'|'-') expr   # AddSub
        | <assoc=right> expr op=('*'|'/') expr   # MulDiv
        | '(' expr ')'                           # parens
        | INT                                    # int
        | ID                                     # id
        ;



<img width="488" height="159" alt="image" src="https://github.com/user-attachments/assets/603eaf54-b381-4509-8480-81756b20b1c1" />

---


## Compilar y ejecutar
Generar los archivos de ANTLR4


        antlr4 -no-listener -visitor LabeledExpr.g4 # -visitor is required!!!


        
Compilar los archivos Java:
  

        javac Calc.java LabeledExpr*.java
  

Ejecutar el programa y recibir entradas por consola:
  

        java Calc
  
Para salir, presiona Ctrl + D o Ctrl + C.

Ejecutar el programa con un archivo de entrada


        java Calc t.expr

