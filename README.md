<div align="center">

## Windows Screen Saver Password Decoder


</div>

### Description

Tells you the Windows screen saver password
 
### More Info
 
hex values

password


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[John Doe](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/john-doe.md)
**Level**          |Beginner
**User Rating**    |4.6 (23 globes from 5 users)
**Compatibility**  |C
**Category**       |[Registry](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/registry__3-11.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/john-doe-windows-screen-saver-password-decoder__3-134/archive/master.zip)

### API Declarations

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
```


### Source Code

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
unsigned char matrix[256+2];
unsigned char matgut[256+2];
unsigned char mystery[4]={ 0xb2, 0xdc, 0x90, 0x8f };
unsigned char h1;
unsigned char pa[79], passwd[80];
unsigned char tofind[30];
int h2=4;
unsigned int lentofind;
int len;
void fixmatrix()
{
	 unsigned char orig, mys, help1, last;
  int i,j, help2;
	 for(i=0; i<256; i++)
		 matrix[i]=i;
	 matrix[256]=0; matrix[256+1]=0;
	 h1=0; last=0;
	 for(j=0;j<256;j++) {
		 orig=matrix[j];
		 mys=mystery[h1];
		 help2=(mys+last+matrix[j]) & 0xff;
		 help1=matrix[help2];
		 matrix[j]=help1;
		 matrix[help2]=orig;
		 last=help2;
		 h1++; h1=h1%4;
	 }
	 memcpy(matgut, matrix, sizeof(matrix));
}
void check(char *test)
{
	 unsigned char help1, oldh2;
	 int i;
	 strcpy(passwd, test);
	 strcpy(pa, passwd);
	 len=strlen(pa);
	 memcpy(matrix, matgut, sizeof(matrix));
	 h1=0; h2=0;
	 for(i=0;i<len;i++)
	 {
		 h1++; h1=h1&0xff;
		 oldh2=matrix[h1];
		 h2=(h2+matrix[h1]) & 0xff;
		 help1=matrix[h1];
		 matrix[h1]=matrix[h2];
		 matrix[h2]=help1;
		 help1=(matrix[h1]+oldh2) & 0xff;
		 help1=matrix[help1];
		 pa[i]^=help1;
	 }
}
int is_ok(char a)
{
	 if ((a<='9') && (a>='0'))
		 return 1;
	 else if ((a<='F') && (a>='A'))
		 return 1;
	 else
		 return 0;
}
int nibble(char c)
{
	 if((c>='A') && (c<='F'))
		 return (10+c-'A');
	 else if((c>='0') && (c<='9'))
		 return (c-'0');
}
void parse(char *inpt)
{
	 char *tok;
	 char num[2];
	 lentofind=0;
	 tok=strtok(inpt, "\t ,\n");
	 while(tok!=NULL) {
		 num[0]=tok[0]; num[1]=tok[1];
		 if ((!is_ok(num[0])) || (!is_ok(num[1])))
		 {
				puts("Please input strings like: b2,a1,03");
				exit(0);
		 }
		 tofind[lentofind++]=16*nibble(num[0])+nibble(num[1]);
		 tok=strtok(NULL, "\t ,\n");
	 }
	 tofind[lentofind]=0;
}
int hex(char *str)
{
	return (str[0]-'0')*16+(str[1]-'0');
}
void main()
{
	 unsigned int i;
	 int j,n=0,odd=0;
	 unsigned char tst[80];
	 char inpt[120];
	 char ascii[120];
	 char temp[3];
	 char ans;
	 fixmatrix();
	 printf("All ascii codes are from der RegEdit and hex codes are from ein text editor\n\n");
	 do
	 {
		 printf("Are der codes hex or ascii [h/a]?");
		 ans = getchar();
		 getchar();
	 } while(tolower(ans) != 'h' && tolower(ans) != 'a');
	 tolower(ans) == 'a';
	 if(tolower(ans) == 'a')
	 {
		 printf("Give me the codes, separated by commas (in ascii):\n >");
		 gets(ascii);
    i=0;
    do
    {
			 temp[0]=ascii[i];
     temp[1]=ascii[i+1];
     temp[2]=NULL;
     inpt[n]=hex(temp);
			 n++;
     odd++;
     if(odd % 2 == 0 && i+3<=strlen(ascii))
     {
				 inpt[n]=',';
       n++;
     }
     i+=3;
		 }while(i<=strlen(ascii));
    inpt[n]=NULL;
		 printf("Der hex codes fur der password are: %s\n", inpt);
	 }
	 else
	 {
		 printf("What are der codes, separated by commas, in hex?:\n >");
		 gets(inpt);
	 }
	 for(i=0;i<strlen(inpt);i++)
		 inpt[i]=toupper(inpt[i]);
	 parse(inpt);
	 for(i=0; i<lentofind; i++)
		 tst[i]='A';
	 tst[lentofind]=0;
	 for(i=0; i<lentofind; i++)
	 {
		 for(j=' '; j<='Z'; j++)
		 {
				tst[i]=j;
				check(tst);
				if(pa[i]==tofind[i])
					 break;
		 }
	 }
	 printf("Password is: %s\n", tst);
	 exit(0);
	 }
```

