# sslab
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

int main()
{
	char opcode[10],oper[10],label[10],code[10],mnem[10]	;
   int locctr,start,length;
   
   FILE *f1,*f2,*f3,*f4;
   
   
   f1=fopen("input.txt","r");
   f2=fopen("optab.txt","r");
   f3=fopen("symtab.txt","w");
   f4=fopen("output.txt","w");
   
   fscanf(f1,"%s\t%s\t%s",label,opcode,oper);
   
   if(strcmp(opcode,"start")==0)
   {	
	   start=atoi(oper);
	   locctr=start; 
	   fprintf(f4,"\t%s\t%s\t%s\n",label,opcode,oper);
	   fscanf(f1,"%s\t%s\t%s",label,opcode,oper);
   }
   else
     {
		locctr=0;
	 }
	 
	 while (strcmp(opcode,"end")!=0)
	 {
		 fprintf(f4,"%d\t",locctr);		 
		 
		 if(strcmp(label,"*")!=0)
			   fprintf(f3,"%s\t%d\n",label,locctr);			 
			     

	     fscanf(f2,"%s\t%s",code,mnem);
	        
	   	 while (strcmp(code,"end")!=0)
	   	 {
			 if(strcmp(opcode,code)==0)
			 {
			     locctr+=3;
			     break;
			     
			 }
  	        fscanf(f2,"%s\t%s",code,mnem);
         }
		if(strcmp(opcode,"WORD")==0) {
		   locctr+=3;}
		   
        else if(strcmp(opcode,"RESW")==0) {
             locctr+=(3*(atoi(oper)));}

		else if(strcmp(opcode,"RESB")==0) {
               locctr+=atoi(oper);}

		else if(strcmp(opcode,"BYTE")==0) {
		     ++locctr;}
	
	    fprintf(f4,"%s\t%s\t%s\t\n",label,opcode,oper);	
	    fscanf(f1,"%s\t%s\t%s",label,opcode,oper);
	  }
	  
	  	  fprintf(f4,"%d\t%s\t%s\t%s",locctr,label,opcode,oper);	  
           length=locctr-start;
           printf("\n\n LENGTH OF CODE IS %d",length);
          
           fclose(f1);
           fclose(f2);
           fclose(f3);
           fclose(f4);
       return 0;   
	  
}
/* *	START   1000
*	LDA     ALPHA
*	ADD     ONE
*     	SUB     TWO
*     	STA     BETA
ALPHA   BYTE    C'KLNCE
ONE     RESB    2
TWO     WORD    5
BETA    RESW    1
*      END     *   */



/*START	*
LDA     00
STA     23
ADD     01
SUB     05
END	*  */














#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main()
{
    FILE *fint, *ftab, *flen, *fsym;
    int op1[10], txtlen, txtlen1, i, j = 0, len;
    char add[5], symadd[5], op[5], start[10], temp[30], line[20], label[20], mne[10], operand[10], symtab[10], opmne[10];
    fint = fopen("input.txt", "r");
    flen = fopen("length.txt", "r");
    ftab = fopen("optab.txt", "r");
    fsym = fopen("symbol.txt", "r");
    fscanf(fint, "%s%s%s%s", add, label, mne, operand);
    if (strcmp(mne, "START") == 0)
    {
        strcpy(start, operand);
        fscanf(flen, "%d", &len);
    }
    printf("H^%s^%s^%d\nT^00%s^", label, start, len, start);
    fscanf(fint, "%s%s%s%s", add, label, mne, operand);
    while (strcmp(mne, "END") != 0)
    {
        fscanf(ftab, "%s%s", opmne, op);
        while (!feof(ftab))
        {
            if (strcmp(mne, opmne) == 0)
            {
                fclose(ftab);
                fscanf(fsym, "%s%s", symadd, symtab);
                while (!feof(fsym))
                {
                    if (strcmp(operand, symtab) == 0)
                    {
                        printf("%s%s^", op, symadd);
                        break;
                    }
                    else
                        fscanf(fsym, "%s%s", symadd, symtab);
                }
                break;
            }
            else
                fscanf(ftab, "%s%s", opmne, op);
        }
        if ((strcmp(mne, "BYTE") == 0) || (strcmp(mne, "WORD") == 0))
        {
            if (strcmp(mne, "WORD") == 0)
                printf("0000%s^", operand);
            else
            {
                len = strlen(operand);
                for (i = 2; i < len; i++)
                {
                    printf("%d", operand[i]);
                }
                printf("^");
            }
        }
        fscanf(fint, "%s%s%s%s", add, label, mne, operand);
        ftab = fopen("optab.txt", "r");
        fseek(ftab, SEEK_SET, 0);
    }
    printf("\nE^00%s", start);
    fclose(fint);
    fclose(ftab);
    fclose(fsym);
    fclose(flen);
    // fclose(fout);
}


/*
input.txt:
        -	COPY	START	1000
        1000	-	LDA	ALPHA
        1003	-	ADD	ONE
        1006	-	SUB	TWO
        1009	-	STA	BETA
        1012	ALPHA	BYTE	C'KLNCE
        1017	ONE	RESB	2
        1019	TWO	WORD	5
        1022	BETA	RESW	1
        1025	-	END	-

optab.txt:
        LDA	00
        STA	23
        ADD	01
        SUB	05


length.txt:
        25

symbol.txt:
        1012	ALPHA
        1017	ONE
        1019	TWO
        1022	BETA
*/
