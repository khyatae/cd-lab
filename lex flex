%{
#include<stdio.h>
typedef struct SymbolTableEntry {
    char name[50];
} SymbolTableEntry;

SymbolTableEntry symbolTable[50];
int symbolTableSize = 0;

void addToSymbolTable(char* identifier) {
        strcpy(symbolTable[symbolTableSize].name, identifier);
        symbolTableSize++;
}

void printSymbolTable() {
    printf("\nSymbol Table:\n");
    int i;
    for (i = 0; i < symbolTableSize; i++) {
        printf("%s\n", symbolTable[i].name);
    }
}

int flag=0;
FILE* fo;
%}
valid [a-zA-Z_][a-zA-Z0-9_]*
%%
"/*" { flag=1; fprintf(fo,"%s--start of the comment\n",yytext); printf("%s--start of the comment\n",yytext); }
"*/" { flag=0; fprintf(fo,"%s--end of the comment\n",yytext); printf("%s--end of the comment\n",yytext); }
#.* { if(flag!=1){fprintf(fo,"%s--preprocessor directive\n",yytext); printf("%s--preprocessor directive\n",yytext);} }
"int" | "double" | "float" | "void" | "if" | "else" | "while" | "do" | "break" | "continue" | "char" | "switch" | "case" | "goto" | "struct" | "for" | "typedef" |
"return" { if(!flag)printf("\nkeyword %s\n",yytext); }
= | \<= | \>= | \< | \> | 
== {if(!flag)printf("\nOperator %s",yytext);}
"{" { if(flag!=1){fprintf(fo,"%s--start of block\n",yytext); printf("%s--start of block\n",yytext);} }
"}" { if(flag!=1){fprintf(fo,"%s--end of block\n",yytext); printf("%s--end of block\n",yytext);} }
{valid}\([^)]*\) { if(flag!=1){fprintf(fo,"%s--funtion\n",yytext); printf("%s--funtion\n",yytext);} }
{valid}(\[[0-9]*\])? { if(flag!=1){fprintf(fo,"%s--identifier\n",yytext); printf("%s--identifier\n",yytext); addToSymbolTable(yytext);} }
\"[^\"]*\" { if(flag!=1){fprintf(fo,"%s--string\n",yytext); printf("%s--string\n",yytext);} }
[0-9]+ { if(flag!=1){fprintf(fo,"%s--number\n",yytext); printf("%s--number\n",yytext);} }

%%
int yywrap()
{
 return 1;
}
int main()
{
 FILE* f;
 f=fopen("D://compiler design lab//programs in cpp//fileinput.txt","r");
 yyin=f;
 yyout=fo;
 fo=fopen("D://compiler design lab//programs in cpp//fileoutput.txt","a");
 if (!fo) {
    perror("Error opening output file");
    return 1;
}
 yylex();
 printSymbolTable();
 getch();
 fclose(f);
 fclose(fo);
}
