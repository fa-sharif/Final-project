#include<stdio.h>
#include<conio.h>
#include<string.h>
#include<stdlib.h>
#include<time.h>
#include<ctype.h>
#include <windows.h>

int random(int tedad_kalamat_sabet,char araye[50][20])
{
    int x;
    time_t t=time(NULL);
    srand(t);
    x=(rand()*tedad_kalamat_sabet)/(RAND_MAX);
    while(isupper(araye[x][0]))
    {
        x=(rand()*tedad_kalamat_sabet)/(RAND_MAX);
    }
    return x;

}
int tedad_marahel()
{
    char string1[20]="level-", string2[20], string3[20]=".txt",string4[20];
    int level_number=1,counter=0;
    FILE *fp;
    sprintf(string2,"%d",level_number);

    strcat(string2,string3);
    strcat(string1,string2);
    fp=fopen(string1,"r");
    strcat(string2,string4);
    strcpy(string1,"level-");
    if(fp==NULL)
    {
        return counter;
    }
    while (fp!=NULL)
    {
        counter++;
        level_number++;
        sprintf(string2,"%d",level_number);
        strcat(string2,string3);
        strcat(string1,string2);
        fp=fopen(string1,"r");
        strcat(string2,string4);
        strcpy(string1,"level-");
        if(fp==NULL)
        {
            return counter;
        }
        fclose(fp);
    }
}

int main()
{
    char name[20], c,harf,string1[20]="level-", string2[20], string3[20]=".txt",answer='n',araye[50][20],name2[20],word[20];
    int selection,lv,x=0,y=0,andis=0,jj,tedad_kalamat_sabet,i=0,b,moti=1,n=0,ghalat=0,levels_num,kalame;
    float zaman,emtiaz_marhale[50],emtiaz_kalame[50],emtiaz_kol[50];

    for(b=0; b<=50; b++)
    {
        emtiaz_kalame[b]=0;
        emtiaz_kol[b]=0;
        emtiaz_marhale[b]=0;
    }



    levels_num=tedad_marahel();
    HANDLE hstdout = GetStdHandle(STD_OUTPUT_HANDLE);
    clock_t start_t, end_t, total_t;

    printf("player name:");
    scanf("%s",name);
    system("cls");
    printf("Hi %s R U ready to play?!",name);

    while(answer='n')
    {
        printf("\n1.play a new game");
        printf("\n2.Resume an old game");
        scanf("%d",&selection);
        system("cls");


        if(selection==2)
        {
            system("cls");
            FILE *fpout;
            fpout=fopen("karbar_info.txt","a+");
            if(fpout==NULL)
            {
                printf("cannot  open file");
                return -1;
            }

            fscanf(fpout, "%s", name2);

            while(strcmp(name2,name)!=0)
            {

                fscanf(fpout, "%s", name2);
            }
            if (strcmp(name2,name)==0)
            {
                fscanf(fpout, "%d", &lv);
                fscanf(fpout, "%f", &emtiaz_marhale[lv]);
                fscanf(fpout, "%f", &emtiaz_kol[lv]);
                lv++;
                FILE*fp;
                sprintf(string2,"%d",lv);
                strcat(string2,string3);
                strcat(string1,string2);

                fp=fopen(string1,"r");
                if(fp==NULL)
                {
                    printf("cannot *open file");
                    return -1;
                }
                for(x=0,y=0; araye[x][y]!=EOF; )
                {

                    araye[x][y]=fgetc(fp);
                    y++;
                    if(araye[x][y-1]==' ')
                    {
                        y=0;
                        x++;
                    }
                }

                fclose(fp);
                tedad_kalamat_sabet=x;
                time_t t=time(NULL);
                srand(t);
                x=(rand()*tedad_kalamat_sabet)/(RAND_MAX);
                jj=0;

                while(araye[x][jj]!=' ')
                {
                    printf("%c",araye[x][jj]);
                    jj++;
                }

                start_t = clock();

                for(andis=0,kalame=0; kalame<tedad_kalamat_sabet;)
                {
                    if(araye[x][andis]==' ')
                    {
                        system("cls");
                        ghalat=0;
                        kalame++;
                        end_t = clock();
                        zaman=end_t - start_t;
                        n=x;
                        emtiaz_kalame[n]=(andis*3 - ghalat)/zaman;

                        x=random(tedad_kalamat_sabet,araye);
                        andis=0;
                        jj=0;
                        while(araye[x][jj]!=' ')
                        {

                            printf("%c",araye[x][jj]);
                            jj++;
                        }
                        start_t = clock();
                    }

                    scanf("%c",&harf);
                    system("cls");

                    if(harf=='Q')
                    {
                        system("cls");
                        printf("\n\n The end \n\n");
                        return 0;
                    }
                    if(harf=='P')
                    {
                        end_t=clock();
                        scanf("%c",&harf);
                    }
                    if(harf=='R')
                    {
                        start_t=clock()+(end_t-start_t);
                        scanf("%c",&harf);
                    }

                    if(harf!=araye[x][andis])
                    {
                        ghalat++;
                        jj=0;
                        for(b=0; b<=20; b++)
                        {
                            word[b]=' ';
                        }
                        b=0;

                        while(araye[x][jj]!=' ')// printf("%*s\n",  180/ 2 +5, string );
                        {
                            SetConsoleTextAttribute(hstdout,10);
                            word[b]=araye[x][jj];
                            jj++;
                            b++;

                        }
                        printf("%*s\n", 180/ 2 +5,word);
                    }
                    printf("\n");
                    if(harf==araye[x][andis])
                    {

                        araye[x][andis]=toupper(araye[x][andis]);
                        jj=0;
                        while(araye[x][jj]!=' ')
                        {

                            printf("%c*",araye[x][jj]);
                            jj++;
                        }
                        andis++;
                    }

                }
                for(b=0; b<tedad_kalamat_sabet; b++)
                {
                    emtiaz_marhale[lv]=emtiaz_kalame[b]+ emtiaz_marhale[lv];
                }
                emtiaz_marhale[lv]=emtiaz_marhale[lv]/tedad_kalamat_sabet;
                for(b=0; b<=lv; b++)
                {
                    emtiaz_kol[lv]=emtiaz_marhale[b]+ emtiaz_marhale[lv];
                }

                system("cls");
                printf("Total score = %f",emtiaz_kol[lv]);

                FILE *fpout;
                fpout=fopen("karbar_info.txt","a+");
                fprintf(fpout,"%s %d %2.6f %2.6f\n",name,lv,emtiaz_marhale[lv],emtiaz_kol[lv]);
                fclose(fpout);
                printf("\n\nExit??[y/n]\n\n");
                scanf("%c",&answer);
                if(answer=='y')
                {
                    answer='n';
                    return 0;
                }


            }

        }

        if(selection==1)
        {
            FILE *fp;
            printf("Ok,let's start");
            printf("pleas enter the level(now I have at most %d levels)",levels_num);
            scanf("%d",&lv);
            sprintf(string2,"%d",lv);
            strcat(string2,string3);
            strcat(string1,string2);
            fp=fopen(string1,"r");
            if(fp==NULL)
            {
                printf("cannot open file");
                return -1;
            }
            //emtiaz_marhale[lv]=0;
            //emtiaz_kol[lv]=0;
            for(x=0,y=0; araye[x][y]!=EOF; )
            {

                araye[x][y]=fgetc(fp);
                y++;
                if(araye[x][y-1]==' ')
                {
                    y=0;
                    x++;
                }
            }
            fclose(fp);
            tedad_kalamat_sabet=x;
            time_t t=time(NULL);
            srand(t);
            x=(rand()*tedad_kalamat_sabet)/(RAND_MAX);
            jj=0;
            while(araye[x][jj]!=' ')
            {
                printf("%c",araye[x][jj]);
                jj++;
            }

            start_t = clock();

            for(andis=0,kalame=0; kalame<tedad_kalamat_sabet;)
            {
                if(araye[x][andis]==' ')
                {
                    system("cls");
                    kalame++;
                    ghalat=0;
                    end_t = clock();
                    zaman=end_t - start_t;
                    n=x;
                    emtiaz_kalame[n]=(andis*3 - ghalat)/zaman;


                    x=random(tedad_kalamat_sabet,araye);
                    andis=0;
                    jj=0;
                    while(araye[x][jj]!=' ')
                    {

                        printf("%c",araye[x][jj]);
                        jj++;
                    }
                    start_t = clock();
                }

                scanf("%c",&harf);
                system("cls");
                if(harf=='Q')
                {
                    system("cls");
                    printf("\n\n The end \n\n");
                    return 0;
                }
                if(harf=='P')
                {
                    end_t=clock();
                    scanf("%c",&harf);
                }
                if(harf=='R')
                {
                    start_t=clock()+(end_t-start_t);
                    scanf("%c",&harf);
                }

                if(harf!=araye[x][andis])
                {
                    ghalat++;
                    jj=0;
                    for(b=0; b<=20; b++)
                    {
                        word[b]=' ';
                    }
                    b=0;
                    while(araye[x][jj]!=' ')// printf("%*s\n",  180/ 2 +5, string );
                    {
                        SetConsoleTextAttribute(hstdout,10);
                        word[b]=araye[x][jj];
                        jj++;
                        b++;

                    }
                    printf("%*s\n", 180/ 2 +5,word);
                }
                printf("\n");
                if(harf==araye[x][andis])
                {

                    araye[x][andis]=toupper(araye[x][andis]);
                    jj=0;
                    while(araye[x][jj]!=' ')
                    {

                        printf("%c*",araye[x][jj]);
                        jj++;
                    }
                    andis++;
                }

            }
            for(b=0; b<tedad_kalamat_sabet; b++)
            {
                emtiaz_marhale[lv]=emtiaz_kalame[b]+ emtiaz_marhale[lv];
            }
            emtiaz_marhale[lv]=emtiaz_marhale[lv]/tedad_kalamat_sabet;
            for(b=0; b<=lv; b++)
            {
                emtiaz_kol[lv]=emtiaz_marhale[b]+ emtiaz_marhale[lv];
            }
            system("cls");
            printf("Total score = %f",emtiaz_kol[lv]);

            FILE *fpout;
            fpout=fopen("karbar_info.txt","a+");
            fprintf(fpout,"%s %d %2.6f %2.6f\n",name,lv,emtiaz_marhale[lv],emtiaz_kol[lv]);
            fclose(fpout);
            printf("\n\nExit??[y/n]\n\n");
            scanf("%c",&answer);
            if(answer=='y')
            {
                answer='n';
                return 0;
            }
        }

    }

}

