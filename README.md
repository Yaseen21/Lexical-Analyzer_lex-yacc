%{
    #include <stdio.h>
    #define yywrap() 1
    int yylineno = 1,total=0,totalOperator=0,totalDatatype=0,totalbracket,lexError=0,totalKeyword=0,totalError=0;
    void printInFile(char []);
    void printError();
%}

/* ------------Fundamental Pieces Of My Language ------------*/

/* Keywords */
Keywords break|skip|back|printf|if|else|typeIn|typeOut|fetchlib|for|whileloop|const|global|func
/* character */
char \"[a-zA-Z0-9]\"|\'[a-zA-Z0-9]\'
/* Identifier */
Identifiers [a-zA-Z][a-zA-Z0-9]*
/* datatype */
dataType intType|charType|decType|boolType|strType
/* Unary Operator */
UnaryOperator "++"|"--"
/* Arithme */
ArithmeticOperator "+"|"-"|"*"|"/"|"%"
/* Relational Operator */
RelationalOperator "<"|">"|"<="|">="|"=="|"!="
/* terminate  */
Terminate ";"
/* Assignment Operator */
AssignmentOperator "="|"+="|"-="|"*="|"/="|"*="
/* Logical Operator */
LogicalOperator "&&"|"||"|"!"
/* Brackets */
/* Brackets [\{\}\[\]\(\)\<\>] */
Brackets "{"|"}"|"["|"]"|"<"|">"|"("|")"
/* String */
String \".*\"
/* Integer */
Integer [0-9]+
/* Float */
Float [0-9]*\.[0-9]+
/* Boolen value */
Boolean ["True"|"False"|"0"|"1"]
/* Space/Tab */
space [ \t]
/* New Line */
NewLine \n

/* ------------Rules ------------*/

%%

{NewLine} {
  yylineno++;}
{Keywords}                                        {
                                                    printf("%d \t%s    \t\tkeyword\n",yylineno,yytext);
                                                    printInFile("keyword");
                                                    total++;
                                                    totalKeyword++;
                                                  }

{char}                                            {
                                                    printf("%d \t%s    \t\t\tchar\n",yylineno,yytext);
                                                    printInFile("char");
                                                    total++;
                                                  } 

{UnaryOperator}                                   {
                                                    printf("%d \t%s    \t\t\tUnary Operator\n",yylineno,yytext);
                                                    printInFile("Unary Operator");
                                                    totalOperator++;
                                                    total++;
                                                  }

{ArithmeticOperator}                              {
                                                    printf("%d \t%s    \t\t\tArithmetic Operator\n",yylineno,yytext);
                                                    printInFile("Arithmetic Operator");
                                                    total++;
                                                    totalOperator++;
                                                  }

{RelationalOperator}                              {
                                                    printf("%d \t%s    \t\t\tRelational Operator\n",yylineno,yytext);
                                                    printInFile("Relational Operator");
                                                    total++;
                                                    totalOperator++;
                                                  }

{AssignmentOperator}                              {
                                                    printf("%d \t%s    \t\t\tAssignment Operator\n",yylineno,yytext);
                                                    printInFile("\tAssignment Operator");
                                                    total++;
                                                    totalOperator++;
                                                  }

{LogicalOperator}                                 {
                                                    printf("%d \t%s    \t\t\tLogical Operator\n",yylineno,yytext);
                                                    printInFile("Logical Operator");
                                                    total++;
                                                    totalOperator++;
                                                  }

{Terminate}                                       {
                                                    printf("%d \t%s    \t\t\tTerminate\n",yylineno,yytext);
                                                    printInFile("\tTerminate");
                                                    total++;
                                                    totalOperator++;
                                                  }
{dataType}                                        {
                                                    printf("%d \t%s    \t\t\tDatatype\n",yylineno,yytext);
                                                    printInFile("Datatype");
                                                    total++;
                                                    totalDatatype++;
                                                  }

{Brackets}                                        {
                                                    printf("%d \t%s    \t\t\tBrackets\n",yylineno,yytext);
                                                    printInFile("\tBrackets");
                                                    total++;
                                                    totalbracket++;
                                                  }

{Identifiers}                                     {
                                                    printf("%d \t%s    \t\t\tIdentifiers\n",yylineno,yytext);
                                                    printInFile("\tIdentifiers");
                                                    total++;
                                                  }
{String}                                          {
                                                    printf("%d \t%s    \t\tString constants\n",yylineno,yytext);
                                                    printInFile("String constants");
                                                    total++;
                                                  }
{Integer}                                         {
                                                    printf("%d \t%s    \t\t\tReal number\n",yylineno,yytext);
                                                    printInFile("Real number");
                                                    total++;
                                                  }
{Float}                                           {
                                                    printf("%d \t%s    \t\t\tFloat number\n",yylineno,yytext);
                                                    printInFile("Float number");
                                                    total++;
                                                  }
{Boolean}                                         {
                                                    printf("%d \t%s    \t\t\tBoolean Value\n",yylineno,yytext);
                                                    printInFile("Boolean Value");
                                                    total++;
                                                  }
{space}                                           {;}



"charType"{space}+{Identifiers}{space}+{AssignmentOperator}{space}+({Integer}|{Float}|{String})  {
  printf("\nLexical Erorr : {%s} line %d\n\n",yytext,yylineno); 
  printError();
  totalError++;

}

"fetchlib"{space}+({Integer}|{Float}) {
  printf("\nLexical Erorr : {%s} line %d\n\n",yytext,yylineno); 
  printError();
  totalError++;
}



%%

int main(int argc, char * argv[]){


    FILE *fptr;
    fptr = fopen("Token.txt", "w");
    fprintf(fptr,"LINE\tLEXME\t\t\t\t TOKEN\n\n");
    printf("LINE\tLEXME\t\t\t TOKEN\n\n");
    fclose(fptr);
    yyin=fopen(argv[1],"r");

    //calling lex function

    yylex();

    printf("_______________________________________________\n");
    printf("Summary\n");
    printf("_______________________________________________\n");
    printf("Total Token      : %d\n",total);
    printf("Total Keyword    : %d\n",totalKeyword);
    printf("Total Operators  : %d\n",totalOperator);
    printf("Total Datatypes  : %d\n",totalDatatype);
    printf("Total Brackets   : %d\n",totalbracket);
    printf("Total Error      : %d\n",totalError);

    fptr = fopen("Token.txt", "a+");
    fprintf(fptr,"_______________________________________________\n");
    fprintf(fptr,"Summary\n");
    fprintf(fptr,"_______________________________________________\n");
    fprintf(fptr,"Total Token      : %d\n",total);
    fprintf(fptr,"Total Keyword    : %d\n",totalKeyword);
    fprintf(fptr,"Total Operators  : %d\n",totalOperator);
    fprintf(fptr,"Total Datatypes  : %d\n",totalDatatype);
    fprintf(fptr,"Total Brackets   : %d\n",totalbracket);
    fprintf(fptr,"Total Error      : %d\n",totalError);


    fclose(fptr);
    fclose(yyin);

    return 0;
}
void printInFile(char Line[])
{
    FILE *fptr;
    fptr = fopen("Token.txt", "a+");
    fprintf(fptr, "  %d \t%s \t\t\t\t%s\n",yylineno,yytext,Line);
    fclose(fptr);
}


void printError()
{
    FILE *fptr;
    fptr = fopen("Token.txt", "a+");
    fprintf(fptr,"\nLexical Erorr : {%s} line %d\n\n",yytext,yylineno); 
    fclose(fptr);
}

