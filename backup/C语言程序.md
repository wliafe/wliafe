# 文件输入输出程序

创建文件并输入信息

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define MAX 41

int main(void)
{
	FILE *fp;
	char words[MAX];
	
	if((fp=fopen("wordy","a+"))==NULL)
	{
		fprintf(stdout,"Can't open \"wordy\" file.\n");
		exit(EXIT_FAILURE);
	}
	puts("Enter words to add to the file; press the #");
	puts("key at the beginning of a line to terminate.");
	while((fscanf(stdin,"%40s",words)==1)&&(words[0]!='#'))
		fprintf(fp,"%s\n",words);
	
	puts("File contents:");
	rewind(fp);
	while(fscanf(fp,"%s",words)==1)
		puts(words);
	puts("Done!");
	if(fclose(fp)!=0)
		fprintf(stderr,"Error closing file\n");
	
	return 0; 
}
```

# 走迷宫游戏

```c
#include<stdio.h>
#include<stdlib.h>
#define CHANG 7
#define KUAN 6
void w();
void a();
void s();
void d();
char c[KUAN][CHANG]={   "######\n",
						"# 0#  \n",
						"# ## #\n",
						"#  # #\n",
						"##   #\n",
						"######\n"
						};
int main(void)
{
	int i;
	char ch;
	printf("%s",c[0]);
	ch=getch();
	while(ch!='q')
	{
		switch(ch)
		{
			case 'w': w();
					break;
			case 'a': a();
					break;
			case 's': s();
					break;
			case 'd': d();
					break;
			default:break;
		}
		if(c[1][5]=='0')
		{
			system("cls");
			printf("Congratulation");
			break;
		}
		system("cls");
		printf("%s",c[0]);
		ch=getch();
	}
	system("pause");
}
void w()
{
	int i,j,n=0;
	for(i=0;i<KUAN;i++)
	{
		for(j=0;j<CHANG;j++)
			if(c[i][j]=='0')
			{
				n++;
				break;
			}
		if(n==1)
			break;
	}
	if(c[i-1][j]!='#')
		{
			c[i-1][j]='0';
			c[i][j]=' ';
		}
}
void a()
{
	int i,j,n=0;
	for(i=0;i<KUAN;i++)
	{
		for(j=0;j<CHANG;j++)
			if(c[i][j]=='0')
			{
				n++;
				break;
			}
		if(n==1)
			break;
	}
	if(c[i][j-1]!='#')
		{
			c[i][j-1]='0';
			c[i][j]=' ';
		}
}
void s()
{
	int i,j,n=0;
	for(i=0;i<KUAN;i++)
	{
		for(j=0;j<CHANG;j++)
			if(c[i][j]=='0')
			{
				n++;
				break;
			}
		if(n==1)
			break;
	}
	if(c[i+1][j]!='#')
		{
			c[i+1][j]='0';
			c[i][j]=' ';
		}
}
void d()
{
	int i,j,n=0;
	for(i=0;i<KUAN;i++)
	{
		for(j=0;j<CHANG;j++)
			if(c[i][j]=='0')
			{
				n++;
				break;
			}
		if(n==1)
			break;
	}
	if(c[i][j+1]!='#')
		{
			c[i][j+1]='0';
			c[i][j]=' ';
		}
}
```

# 学生管理系统1

学生信息录入，打印，添加，删除，更改，查看。

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define TSIZE 45
struct xinxi{
	char xuehao[10];
	char name[TSIZE];
	struct xinxi * next;
};
void input(void);
void print(void);
int add(void);
int cut(void);
int change(void);
void check(void);
struct xinxi * first;
struct xinxi * prev,* current;

int main(void)
{
	first=(struct xinxi*)malloc(sizeof(struct xinxi));
	first->next=NULL;
	int ch;
	puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
	while(scanf("%d",&ch)!=0)
	{
		switch(ch)
		{
			case 1:input();
					continue;
			case 2:print();
					continue;
			case 3:cut();	
					continue;
			case 4:change();
					continue;
			case 5:check();
					continue;
			case 6:add();
					continue;
		}
	}
	return 0;
}
//输入信息 ，添加信息。 
void input(void)
{
	if(first->next==NULL)
	{
		current=(struct xinxi*)malloc(sizeof(struct xinxi));
		first->next=current;
	}
	else
	{
		prev=first;	
		while(prev->next!=NULL)
			prev=prev->next;
		current=(struct xinxi*)malloc(sizeof(struct xinxi));
		prev->next=current;
	}
		puts("input name:");
		scanf("%s",current->name);
		puts("input xuehao:");
		scanf("%s",current->xuehao);
		current->next=NULL;
		puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
}
//输出全部信息 
void print(void)
{
	if(first->next==NULL)
		printf("err\n");
	current=first->next;
	while(current!=NULL)
	{
		printf("%s:%s\n",current->name,current->xuehao);
		current=current->next;
	}
	puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
}
//删除信息 
int cut(void)
{
	char a[TSIZE];
	puts("please enter the xinxi:");
	scanf("%s",a);
	if(first->next==NULL)
		printf("err\n");
	current=first;
	while(current->next!=NULL)
	{
		prev=current->next;
		if(!strcmp(a,prev->xuehao)||!strcmp(a,prev->name))
		{
			current->next=prev->next;
			puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
			return 0;
		}
		current=prev;
	}
	puts("err");
	puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
	return 0;
}
//更改信息 
int change(void)
{
	char a[TSIZE];
	puts("please enter the xinxi:");
	scanf("%s",a);
	if(first->next==NULL)
		printf("err\n");
	current=first->next;
	while(current!=NULL)
	{
		if(!strcmp(a,current->xuehao)||!strcmp(a,current->name))
		{
			puts("input name:");
			scanf("%s",current->name);
			puts("input xuehao:");
			scanf("%s",current->xuehao);
			puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
			return 0;
		}
		current=current->next;
	}
	puts("err");
	puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
	return 0;
}
//查看信息 
void check(void)
{
	char a[TSIZE];
	puts("please enter the xinxi:");
	scanf("%s",a);
	if(first->next==NULL)
		printf("err\n");
	current=first->next;
	while(current!=NULL)
	{
		if(!strcmp(a,current->xuehao)||!strcmp(a,current->name))
		{
			printf("%s:%s\n",current->name,current->xuehao);
			break;
		}
		current=current->next;
	}
	puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
}
//中间添加信息 
int add(void)
{
	char a[TSIZE];
	puts("please enter the before xinxi:");
	scanf("%s",a);
	if(first->next==NULL)
		printf("err\n");
	current=first->next;
	while(current!=NULL)
	{
		if(!strcmp(a,current->xuehao)||!strcmp(a,current->name))
		{
			struct xinxi *prev1;
			prev1=(struct xinxi*)malloc(sizeof(struct xinxi));
			prev=current->next;
			current->next=prev1;
			prev1->next=prev;
			current=prev1;
			puts("input name:");
			scanf("%s",current->name);
			puts("input xuehao:");
			scanf("%s",current->xuehao);
			puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
			return 0;
		}
		current=current->next;
	}
	puts("err");
	puts("1 input 2 print 3 cut 4 change 5 check 6 add q break.");
	return 0;
}
```

# 学生管理系统2

```c
#define _CRT_SECURE_NO_WARNINGS
//学生管理系统
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define NUMBER 45

struct subject {
    int km;
    char kmname[12];
    struct subject* next;
};
struct teacher {
    char name[20];
    char kmname[12];
    struct teacher* next;
};
struct student {
    char name[20];//姓名
    char number[9];//学号
    char sex[6];//性别
    char major[15];//专业
    char telephone[12];//电话
    char home[50];//家庭住址
    struct subject* first;
    struct student* next;
};
struct Mima {
    char account[9];
    char mima[7];
    struct Mima* next;
};

void Administrator(void);//初始界面
void establish_zh1(void);//管理员创建

int enter1(void);//管理员登录
void change_zh1(void);//修改管理员密码
void Administrator1(void);//管理主界面，管理员
void control11(struct student* head_xsxx, struct Mima* head_xsmm, struct teacher* head_tcxx, struct Mima* head_tcmm,int count_tc);//控制面板，管理员
int establish_zh2(struct Mima* head_tcmm, struct teacher* head_tcxx, struct student* head_xsxx);//创建教师账户
void check_zh2(struct Mima* head_tcmm, struct teacher* head_tcxx);//查看教师账户
void change_zh2(struct Mima* head_tcmm, struct teacher* head_tcxx);//修改姓名，教师密码改为默认密码
void check_xx21(struct teacher* head_tcxx);//查看教师信息
int delete_zh2(struct Mima* head_tcmm, struct teacher* head_tcxx, struct student* head_xsxx);//删除教师
void establish_zh3(struct Mima* head_xsmm, struct student* head_xsxx, struct teacher* head_tcxx);//创建学生账户
void check_zh3(struct Mima* head_xsmm, struct student* head_xsxx);//查看学生账户
void change_zh3(struct Mima* head_xsmm, struct student* head_xsxx);//修改姓名，学生密码改为默认密码
void check_xx31(struct student* head, struct teacher* head1, int p);//查看学生信息，管理员，教师
void delete_zh3(struct Mima* head_xsmm, struct student* head_xsxx);//删除学生

int input_zh2(struct Mima* head);//教师账户信息文件读取
void input_xx2(struct teacher* head);//教师信息文件读取
void save_zh2(struct Mima* head);//教师账户文件保存
void save_xx2(struct teacher* head);//教师信息文件保存
void Administrator2(struct Mima* current);//管理主界面，教师
void control2(struct student* head_xsxx, struct Mima* head_xsmm, struct teacher* head_tcxx, struct Mima* head_tcmm,int count_tc);//控制面板，教师
void change_zh22(struct Mima* current);//修改教师密码
void change_xx32(struct student* head_xsxx, int n);//修改学生成绩

void input_zh3(struct Mima* head);//学生账户信息文件读取
void input_xx3(struct student* head);//学生信息文件读取
void Administrator3(struct Mima* current);//管理主界面，学生
void control3(struct student* head_xsxx,struct Mima* head_xsmm,struct teacher* head_tcxx,struct Mima* head_tcmm,int count_tc);//控制面板，学生
void change_zh33(struct Mima* current);//修改学生密码
void save_zh3(struct Mima* head);//学生账户文件保存
void save_xx3(struct student* head);//学生信息文件保存
void check_xx33(struct student* head, int p);//查看学生信息，学生
void change_xx33(struct student* head_xsxx);//修改学生信息


int main(void) {
    int count_tc;
    char ch;
    establish_zh1();
    
    struct student* head_xsxx;
    struct Mima* head_xsmm;
    struct teacher* head_tcxx;
    struct Mima* head_tcmm;
    head_xsxx = (struct student*)malloc(sizeof(struct student));
    head_xsmm = (struct Mima*)malloc(sizeof(struct Mima));
    head_tcxx = (struct teacher*)malloc(sizeof(struct teacher));
    head_tcmm = (struct Mima*)malloc(sizeof(struct Mima));
    head_xsxx->next = NULL;
    head_xsmm->next = NULL;
    head_tcxx->next = NULL;
    head_tcmm->next = NULL;
    input_xx3(head_xsxx);
    input_zh3(head_xsmm);
    input_xx2(head_tcxx);
    count_tc=input_zh2(head_tcmm);
Administrator();
    while (ch = getche()) {
        switch (ch) {
        case '1':system("cls"); control3(head_xsxx, head_xsmm, head_tcxx, head_tcmm, count_tc); return 0;
        case '2':system("cls"); control2(head_xsxx, head_xsmm, head_tcxx, head_tcmm, count_tc); return 0;
        case '3':system("cls"); control11(head_xsxx, head_xsmm, head_tcxx, head_tcmm, count_tc); return 0;
        case 'q':return 0;
        default:printf("请重新输入：\n"); break;
        }
    }
        
    return 0;
}
//初始界面
void Administrator(void) {
    int i, n = 39;
    printf("\n\n\n\n\n\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("欢迎来到学生管理系统");
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          1 学生登录              |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          2 教师登录              |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          3 管理员登录            |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          q 退出管理系统          |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 36; i++) {
        printf("+");
    }
    printf("\n");
}

//管理员板块

//管理员登录
int enter1(void) {
    FILE* fp;
    char mm[9];
    char input[9];
    fp = fopen("mima.txt", "r");
    fscanf(fp, "%s", mm);
    fscanf(fp, "%s", mm);
    printf("\n\n\n\n\n\n\n\t\t\t\t\t请输入管理员登录密码(q退出)：");
    scanf("%s", input);
    while (input[0]!='q') {
        if (!strcmp(mm, input))return 1;
        printf("密码错误，请重新输入(q退出)：");
        scanf("%s", input);
    }

    return 0;
}
//修改管理员密码
void change_zh1(void) {
    FILE* fp;
    char input[9];
    char mm[9];
    fp = fopen("mima.txt", "r");
    fscanf(fp, "%s", mm);
    fscanf(fp, "%s", mm);
    fclose(fp);
    printf("请输入旧密码：");
    scanf("%s", input);
    if (!strcmp(input, mm)) {
        printf("请输入新密码：");
        scanf("%s", input);
        printf("请重新输入新密码：");
        scanf("%s", mm);
        while (strcmp(input, mm)) {
            printf("密码错误\n");
            printf("请输入新密码：");
            scanf("%s", input);
            printf("请重新输入新密码：");
            scanf("%s", mm);
        }
        fp = fopen("mima.txt", "w");
        fprintf(fp, "%s %s\n", "管理员", input);
    }
}
//管理员创建
void establish_zh1(void) {
    FILE* fp;
    char ma[9];
    char account[7];
    fp = fopen("mima.txt", "r");
    if (fp == NULL) {
        printf("\n\n\n\n\n\n\n\t\t\t\t\t初次登录，请创建管理员账户!!!\n\
\t\t\t\t\t请创建管理员登录密码：");
        scanf("%s", ma);
        fp = fopen("mima.txt", "w");
        fprintf(fp, "%s %s\n", "管理员", ma);
        fclose(fp);
    }
    fp = fopen("mima.txt", "r");
    fscanf(fp, "%s", account);
    while (strcmp(account, "管理员")) {
        printf("\t\t\t\t\t文件读取错误，请重新创建管理员账户\n");
        system("pause");
        system("cls");
        printf("\n\n\n\n\n\n\n\t\t\t\t\t初次登录，请创建管理员账户!!!\n\
\t\t\t\t\t请创建管理员登录密码：");
        scanf("%s", ma);
        fp = fopen("mima.txt", "w");
        fprintf(fp, "%s %s\n", "管理员", ma);
        fclose(fp);
        fp = fopen("mima.txt", "r");
        fscanf(fp, "%s", account);
        fclose(fp);
    }
    system("cls");
}
//管理主界面，管理员
void Administrator1(void) {
    int i, n = 39;
    printf("\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 10; i++) {
        printf("+");
    }
    printf("管理员，欢迎登录");
    for (i = 0; i < 10; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          1 修改管理员密码        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          2 创建教师账户          |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          3 查看教师账户          |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          4 修改教师密码          |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          5 查看教师信息          |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          6 删除教师              |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          7 创建学生账户          |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          8 查看学生账户          |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          9 修改学生密码          |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          a 查看学生信息          |\n");//两种方式

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          b 删除学生              |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|          q 退出学生管理系统      |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 36; i++) {
        printf("+");
    }
    printf("\n");
}
//控制面板，管理员
void control11(struct student* head_xsxx, struct Mima* head_xsmm, struct teacher* head_tcxx, struct Mima* head_tcmm,int count_tc) {
    char ch;
    int n;
    n=enter1();
    if (n == 0)return;
    system("cls");
    Administrator1();
    while (ch = getche()) {
        switch (ch) {
        case '1':system("cls"); change_zh1();  break;
        case '2':system("cls"); count_tc = establish_zh2(head_tcmm, head_tcxx, head_xsxx); save_xx2(head_tcxx); save_zh2(head_tcmm); break;
        case '3':system("cls"); check_zh2(head_tcmm, head_tcxx); break;
        case '4':system("cls"); change_zh2(head_tcmm, head_tcxx); save_xx2(head_tcxx); save_zh2(head_tcmm); break;
        case '5':system("cls"); check_xx21(head_tcxx); break;
        case '6':system("cls"); count_tc=delete_zh2(head_tcmm, head_tcxx, head_xsxx); save_xx2(head_tcxx); save_zh2(head_tcmm); break;
        case '7':system("cls"); establish_zh3(head_xsmm, head_xsxx,head_tcxx); save_zh3(head_xsmm); save_xx3(head_xsxx); break;
        case '8':system("cls"); check_zh3(head_xsmm, head_xsxx); break;
        case '9':system("cls"); change_zh3(head_xsmm, head_xsxx); save_zh3(head_xsmm); save_xx3(head_xsxx); break;
        case 'a':system("cls"); check_xx31(head_xsxx,head_tcxx,count_tc); break;
        case 'b':system("cls"); delete_zh3(head_xsmm, head_xsxx); save_zh3(head_xsmm); save_xx3(head_xsxx); break;
        case 'q':return;
        default:printf("请重新输入：\n"); break;
        }
        system("cls");
        Administrator1();
    }
}
//创建教师账户
int establish_zh2(struct Mima* head_tcmm, struct teacher* head_tcxx, struct student* head_xsxx) {
    char input[9];
    struct Mima* current;
    struct teacher* current1;
    struct student* current2;
    struct subject* current3;
    int count_tc = 0;
    current = head_tcmm;
    current1 = head_tcxx;
    while (current->next != NULL && current1->next != NULL) {
        current = current->next;
        current1 = current1->next;
        count_tc++;
    }
    printf("请输入教师账号(q退出)：");
    while (scanf("%s", input) != 0 && input[0] != 'q') {
        getchar();
        current->next = (struct Mima*)malloc(sizeof(struct Mima));
        current1->next = (struct teacher*)malloc(sizeof(struct teacher));
        current = current->next;
        current1 = current1->next;
        strcpy(current->account, input);
        strncpy(current->mima, input + 2, 6);
        current->mima[6] = '\0';
        printf("请输入教师姓名：");
        scanf("%s", current1->name);
        printf("请输入教师所教科目名称：");
        scanf("%s", current1->kmname);
        current->next = NULL;
        current1->next = NULL;
        count_tc++;
        current2 = head_xsxx;
        while (current2->next != NULL) {
            current2 = current2->next;
            current3 = current2->first;
            while (current3->next != NULL) {
                current3 = current3->next;
            }
            current3->next = (struct subject*)malloc(sizeof(struct subject));
            current3 = current3->next;
            strcpy(current3->kmname, current1->kmname);
            current3->km = 0;
        }
        printf("请输入教师账号(q退出)：");
    }
    return count_tc;
}
//查看教师账户
void check_zh2(struct Mima* head_tcmm, struct teacher* head_tcxx) {
    char input[20];
    char ch;
    int t = 0;
    int i, n = 39;
    struct Mima* current;
    struct teacher* current1;
    current = head_tcmm;
    current1 = head_tcxx;
    if (head_tcmm->next == NULL || head_tcxx->next == NULL) {
        printf("\n\n\n\n\n\n\n                   \
                              账户列表为空!!!\n");
        system("pause");
        return;
    }

    printf("\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("教师账户信息查询");
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        a 全部信息展示        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        c 单人信息查询        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        q 退出查询            |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 32; i++) {
        printf("+");
    }
    printf("\n");

    ch = getch();
    switch (ch) {
    case 'a':while (current->next != NULL && current1->next != NULL) {
        current = current->next;
        current1 = current1->next;
        printf("姓名：%-10s账号：%-10s密码：%-8s\n", \
            current1->name, current->account, current->mima);
    }
            system("pause");
            return;
    case 'c':printf("请输入教师姓名或账号：");
        scanf("%s", input);
        while (current->next != NULL && current1->next != NULL) {
            current = current->next;
            current1 = current1->next;
            if (!strcmp(current1->name, input) || !strcmp(current->account, input)) {
                printf("姓名：%-10s账号：%-10s密码：%-8s\n", \
                    current1->name, current->account, current->mima);
                t = 1;
                break;
            }
        }
        if (t == 0)printf("查无此人\n");
        system("pause");
        return;
    case 'q':return;
    }
}
//修改姓名，教师密码改为默认密码
void change_zh2(struct Mima* head_tcmm, struct teacher* head_tcxx) {
    char input[20];
    char ch;
    int t = 0;
    int i, n = 39;
    struct Mima* current;
    struct teacher* current1;
    current = head_tcmm;
    current1 = head_tcxx;
    if (head_tcmm->next == NULL || head_tcxx->next == NULL) {
        printf("\n\n\n\n\n\n\n                   \
                              账户列表为空!!!\n");
        system("pause");
        return;
    }

    printf("\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("教师账户信息修改");
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        1 修改教师姓名        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        2 修改教师密码        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        q 退出修改            |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 32; i++) {
        printf("+");
    }
    printf("\n");

    printf("请输入学生姓名或账号(q退出)：");
    scanf("%s", input);
    while (current->next != NULL && current1->next != NULL) {
        current = current->next;
        current1 = current1->next;
        if (!strcmp(current1->name, input) || !strcmp(current->account, input)) {
            printf("姓名：%-10s账号：%-10s密码：%-8s\n", \
                current1->name, current->account, current->mima);
            t = 1;
            break;
        }
    }
    if (t == 0) {
        printf("查无此人\n");
        system("pause");
        return;
    }
    printf("选择修改项目：");
    ch = getch();
    switch (ch) {
    case '1':printf("姓名修改\n请输入学生姓名：");
        scanf("%s", current1->name);
        break;
    case '2':printf("密码还原\n");
        strncpy(current->mima, current->account + 2, 6);
        printf("密码已改为默认密码\n");
        system("pause");
        break;
    case 'q':break;
    }
}
//删除教师
int delete_zh2(struct Mima* head_tcmm, struct teacher* head_tcxx, struct student* head_xsxx) {
    char input[20];
    int count_tc = 0;
    char ch;
    int t = 0;
    struct Mima* current, * prev=NULL;
    struct teacher* current1, * prev1=NULL;
    struct student* current2;
    struct subject* current3, * prev3=NULL;
    current = head_tcmm;
    current1 = head_tcxx;
    if (head_tcmm->next == NULL || head_tcxx->next == NULL) {
        printf("\n\n\n\n\n\n\n                   \
                              账户列表为空!!!\n");
        system("pause");
        return 0;
    }
    printf("                         删除教师信息\n\
***************************************************************\n");
    printf("请输入教师账号(q退出)：");
    scanf("%s", input);
    while (current->next != NULL && current1->next != NULL) {
        prev = current;
        prev1 = current1;
        current = current->next;
        current1 = current1->next;
        count_tc++;
        if (!strcmp(current1->name, input) || !strcmp(current->account, input)) {
            printf("姓名：%-10s账号：%-10s密码：%-8s\n", \
                current1->name, current->account, current->mima);
            t = 1;
            break;
        }
    }
    if (t == 0) {
        printf("查无此人\n");
        system("pause");
        return count_tc;
    }
    printf("是否删除，是(Y)否(N)");
    ch = getch();
    if (ch == 'Y' || ch == 'y') {
        current2 = head_xsxx;
        while (current2->next!= NULL) {
            current2 = current2->next;
            current3 = current2->first;
            while (current3->next != NULL) {
                prev3 = current3;
                current3 = current3->next;
                if (!strcmp(current3->kmname, current1->kmname)) {
                    prev3->next = current3->next;
                    free(current3 );
                    break;
                }
            }
        }
        prev->next = current->next;
        prev1->next = current1->next;
        free(current);
        free(current1);
    }
    count_tc--;

    return count_tc;
}
//查看教师信息,管理员
void check_xx21(struct teacher* head_tcxx) {
    char input[20];
    char ch;
    int t = 0;
    int i, n = 39;
    struct teacher* current;
    current = head_tcxx;
    if (head_tcxx->next == NULL) {
        printf("\n\n\n\n\n\n\n                   \
                          教师信息列表为空!!!\n");
        system("pause");
        return;
    }

    printf("\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 10; i++) {
        printf("+");
    }
    printf("教师信息查询");
    for (i = 0; i < 10; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        a 全部信息展示        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        c 单人信息查询        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        q 退出查询            |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 32; i++) {
        printf("+");
    }
    printf("\n");

    ch = getch();
    switch (ch) {
    case 'a':while (current->next != NULL) {
        current = current->next;
        printf("姓名：%-30s所授科目：%s\n", current->name, current->kmname);
    }
        system("pause");
        return;
    case 'c':printf("请输入教师姓名：");
        scanf("%s", input);
        while (current->next != NULL) {
            current = current->next;
            if (!strcmp(current->name, input)) {
                printf("姓名：%-30s所授科目：%s\n", current->name, current->kmname);
                t = 1;
                break;
            }
        }
        if (t == 0)printf("查无此人\n");
        system("pause");
        return;
    case 'q':return;
    }
}
//创建学生账户
void establish_zh3(struct Mima* head_xsmm, struct student* head_xsxx, struct teacher* head_tcxx) {
    char input[9];
    char m[9] = { "未填写" };
    struct Mima* current;
    struct student* current1;
    struct teacher* current2;
    struct subject* current3;
    current = head_xsmm;
    current1 = head_xsxx;
    while (current->next != NULL && current1->next != NULL) {
        current = current->next;
        current1 = current1->next;
    }
    printf("请输入新生学号(即账号)(q退出)：");
    while (scanf("%s", input) != 0 && input[0] != 'q') {
        getchar();
        current->next= (struct Mima*)malloc(sizeof(struct Mima));
        current1->next= (struct student*)malloc(sizeof(struct student));
        current = current->next;
        current1 = current1->next;
        strcpy(current->account, input);
        strcpy(current1->number, input);
        strncpy(current->mima, input + 2, 6);
        current->mima[6] = '\0';
        printf("请输入学生姓名：");
        scanf("%s", current1->name);
        current->next = NULL;
        current1->next = NULL;
        strcpy(current1->home, m);
        strcpy(current1->sex, m);
        strcpy(current1->major, m);
        strcpy(current1->telephone, m);
        current1->first = (struct subject*)malloc(sizeof(struct subject));
        current2 = head_tcxx;
        current3 = current1->first;
        while (current2->next != NULL) {
            current3->next= (struct subject*)malloc(sizeof(struct subject));
            current2 = current2->next;
            current3 = current3->next;
            strcpy(current3->kmname, current2->kmname);
            current3->km = 0;
            current3->next = NULL;
        }
        
        printf("请输入新生学号(即账号)(q退出)：");
    }
}
//查看学生账户
void check_zh3(struct Mima* head_xsmm, struct student* head_xsxx) {
    char input[20];
    char ch;
    int t = 0;
    int i, n = 39;
    struct Mima* current;
    struct student* current1;
    current = head_xsmm;
    current1 = head_xsxx;
    if (head_xsmm->next == NULL || head_xsxx->next == NULL) {
        printf("\n\n\n\n\n\n\n                   \
                              账户列表为空!!!\n");
        system("pause");
        return;
    }

    printf("\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("学生账户信息查询");
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        a 全部信息展示        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        c 单人信息查询        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        q 退出查询            |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 32; i++) {
        printf("+");
    }
    printf("\n");

    ch = getch();
    switch (ch) {
    case 'a':while (current->next != NULL && current1->next != NULL) {
        current = current->next;
        current1 = current1->next;
        printf("姓名：%-10s账号：%-10s密码：%-8s\n",\
            current1->name,current->account,current->mima);
    } 
    system("pause");
    return;
    case 'c':printf("请输入学生姓名或账号：");
        scanf("%s", input);
        while (current->next != NULL && current1->next != NULL){
            current = current->next;
            current1 = current1->next;
            if (!strcmp(current1->name, input) || !strcmp(current->account, input)) {
                printf("姓名：%-10s账号：%-10s密码：%-8s\n", \
                    current1->name, current->account, current->mima);
                t = 1;
                break;
            }
        } 
    if (t == 0)printf("查无此人\n");
    system("pause");
    return;
    case 'q':return;
    }
}
//修改姓名，学生密码改为默认密码
void change_zh3(struct Mima* head_xsmm, struct student* head_xsxx) {
    char input[20];
    char ch;
    int t = 0;
    int i, n = 39;
    struct Mima* current;
    struct student* current1;
    current = head_xsmm;
    current1 = head_xsxx;
    if (head_xsmm->next == NULL || head_xsxx->next == NULL) {
        printf("\n\n\n\n\n\n\n                   \
                              账户列表为空!!!\n");
        system("pause");
        return;
    }

    printf("\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("学生账户信息修改");
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        1 修改学生姓名        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        2 修改学生密码        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        q 退出修改            |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 32; i++) {
        printf("+");
    }
    printf("\n");
    
    printf("请输入学生姓名或账号(q退出)：");
    scanf("%s", input);
    while (current->next != NULL && current1->next != NULL){
        current = current->next;
        current1 = current1->next;
        if (!strcmp(current1->name, input) || !strcmp(current->account, input)) {
            printf("姓名：%-10s账号：%-10s密码：%-8s\n", \
                current1->name, current->account, current->mima);
            t = 1;
            break;
        }
    } 
    if (t == 0) {
        printf("查无此人\n");
        system("pause");
        return;
    }
    printf("选择修改项目：");
    ch = getch();
    switch (ch) {
    case '1':printf("姓名修改\n请输入学生姓名：");
        scanf("%s", current1->name);
        break;
    case '2':printf("密码还原\n");
        strncpy(current->mima, current->account + 2, 6);
        printf("密码已改为默认密码\n");
        system("pause");
        break;
    case 'q':break;
    }
}
//删除学生
void delete_zh3(struct Mima* head_xsmm, struct student* head_xsxx) {
    char input[20];
    char ch;
    int t = 0;
    struct Mima* current, * prev=NULL;
    struct student* current1, * prev1=NULL;
    current = head_xsmm;
    current1 = head_xsxx;
    if (head_xsmm->next == NULL || head_xsxx->next == NULL) {
        printf("\n\n\n\n\n\n\n                   \
                              账户列表为空!!!\n");
        system("pause");
        return;
    }
    printf("                         删除学生信息\n\
***************************************************************\n");
    printf("请输入学生姓名或账号(q退出)：");
    scanf("%s", input);
    while (current->next != NULL && current1->next != NULL) {
        prev = current;
        prev1 = current1;
        current = current->next;
        current1 = current1->next;
        if (!strcmp(current1->name, input) || !strcmp(current->account, input)) {
            printf("姓名：%-10s账号：%-10s密码：%-8s\n", \
                current1->name, current->account, current->mima);
            t = 1;
            break;
        }
    } 
    if (t == 0) {
        printf("查无此人\n");
        system("pause");
        return;
    }
    printf("是否删除，是(Y)否(N)");
    ch = getch();
    if (ch == 'Y' || ch == 'y') {
        free(prev->next);
        free(prev1->next);
        prev->next = current->next;
        prev1->next = current1->next;
    }
}
//查看学生信息,管理员，教师
void check_xx31(struct student*head,struct teacher*head1,int p) {//p是学科数目，即老师数
    char input[20];
    char ch;
    int t = 0;
    int i, n = 39;
    int n1 = 37 + 12 * p + 24;
    int nlong;
    struct student* current;
    struct subject* current1;
    struct teacher* current2;
    current = head;
    current2 = head1;
    if (head->next == NULL) {
        printf("\n\n\n\n\n\n\n                   \
                          学生信息列表为空!!!\n");
        system("pause");
        return;
    }

    printf("\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 10; i++) {
        printf("+");
    }
    printf("学生信息查询");
    for (i = 0; i < 10; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        a 全部信息展示        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        c 单人信息查询        |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|        q 退出查询            |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 32; i++) {
        printf("+");
    }
    printf("\n");

    ch = getch();
    switch (ch) {
    case 'a':printf("+");
        nlong = n1;
        while (nlong--)printf("-");
        printf("+\n");
        printf("|学号      姓名      性别    专业        ");
        while (current2->next!=NULL) {
            current2 = current2->next;
            printf("%-12s", current2->kmname);
        }
        printf("电话         家庭住址|\n");
        printf("+");
        nlong = n1;
        while (nlong--)printf("-");
        printf("+\n");

        while (current->next != NULL) {
            current = current->next;
            printf("|%-10s%-10s%-8s%-12s", current->number, current->name, current->sex, current->major);
            current1 = current->first;
            while (current1->next != NULL) {
                current1 = current1->next;
                printf("%-12d", current1->km);
            }
            printf("%-13s%-8s|\n", current->telephone, current->home);
            printf("+");
            nlong = n1;
            while (nlong--)printf("-");
            printf("+\n");
        }
        system("pause");
        return;
    case 'c':printf("请输入学生姓名或学号：");
        scanf("%s", input);
        while (current->next != NULL) {
            current = current->next;
            if (!strcmp(current->name, input)||!strcmp(current->number,input)) {
                check_xx33(current, p);
                t = 1;
                break;
            }
        }
        if (t == 0)printf("查无此人\n");
        system("pause");
        return;
    case 'q':return;
    }
}

//教师板块

//教师信息文件读取
void input_xx2(struct teacher* head) {
    FILE* fp;
    struct teacher* current;
    char name[30];
    if ((fp = fopen("teacher_xx.txt", "r")) == NULL)
        return;
    current = head;
    while (fscanf(fp, "%s", name) != EOF) {
        current->next = (struct teacher*)malloc(sizeof(struct teacher));
        current = current->next;
        strcpy(current->name, name);
        fscanf(fp, "%s", current->kmname);
        current->next = NULL;
    }
    fclose(fp);
}
//教师账户文件读取
int input_zh2(struct Mima* head) {
    FILE* fp;
    struct Mima* current;
    int p=0;
    char account[9];
    if ((fp = fopen("teacher_zh.txt", "r")) == NULL)
        return 0;
    current = head;
    while (fscanf(fp, "%s", account) != EOF) {
        current->next = (struct Mima*)malloc(sizeof(struct Mima));
        current = current->next;
        strcpy(current->account, account);
        fscanf(fp, "%s", current->mima);
        current->next = NULL;
        p++;
    }
    fclose(fp);
    return p;
}
//教师信息文件保存
void save_xx2(struct teacher* head) {
    FILE* fp;
    struct teacher* current;
    if (head->next == NULL) {
        return;
    }
    fp = fopen("teacher_xx.txt", "w");
    while (fp == NULL)fp = fopen("teacher_xx.txt", "w");
    current = head;
    while (current->next != NULL) {
        current = current->next;
        fprintf(fp, "%s ", current->name);
        fprintf(fp, "%s\n", current->kmname);
    }
    fclose(fp);
}
//教师账户文件保存
void save_zh2(struct Mima* head) {
    FILE* fp;
    struct Mima* current;
    if (head->next == NULL)return;
    fp = fopen("teacher_zh.txt", "w");
    while (fp == NULL)fp = fopen("teacher_zh.txt", "w");
    current = head;
    while (current->next != NULL) {
        current = current->next;
        fprintf(fp, "%s %s\n", current->account, current->mima);
    }
    fclose(fp);
}
//管理主界面，教师
void Administrator2(struct Mima* current) {
    int i, n = 39;
    printf("\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("%s，欢迎登录", current->account);
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|         1 修改教师密码         |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|         2 修改学生成绩         |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|         3 查看学生信息         |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|         q 退出                 |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 34; i++) {
        printf("+");
    }
    printf("\n");
}
//控制面板，教师
void control2(struct student* head_xsxx, struct Mima* head_xsmm, struct teacher* head_tcxx, struct Mima* head_tcmm, int count_tc) {
    char input[9];
    int t = 0, n = 0;
    char ch;
    struct Mima* current;
    struct teacher* current1;
    printf("\n\n\n\n\n\n\n\t\t\t\t\t请输入教师账号(q退出)：");
    scanf("%s", input);
    current = head_tcmm;
    current1 = head_tcxx;
    while (current->next != NULL && current1->next != NULL) {
        current = current->next;
        current1 = current1->next;
        n++;
        while (!strcmp(current->account, input)) {
            printf("请输入密码：");
            scanf("%s", input);
            if (!strcmp(current->mima, input)) {
                t = 1;
                break;
            }
        }
        if (t == 1)break;
    }
    if (t == 0) {
        printf("查无此人");
        system("pause");
        return;
    }
    

    Administrator2(current);
    while (ch = getche()) {
        switch (ch) {
        case '1':system("cls"); change_zh22(current); save_zh2(head_tcmm); break;
        case '2':system("cls"); change_xx32(head_xsxx, n); save_xx3(head_xsxx); break;
        case '3':system("cls"); check_xx31(head_xsxx, head_tcxx, count_tc); break;
        case 'q':return;
        default:printf("请重新输入：\n"); system("pause"); break;
        }
        system("cls");
        Administrator2(current);
    }
}
//修改教师密码
void change_zh22(struct Mima* current) {
    char input[9];
    char mm[9];
    printf("请输入旧密码：");
    scanf("%s", input);
    if (!strcmp(input, current->mima)) {
        printf("请输入新密码：");
        scanf("%s", input);
        printf("请重新输入新密码：");
        scanf("%s", mm);
        while (strcmp(input, mm)) {
            printf("密码错误\n");
            printf("请输入新密码：");
            scanf("%s", input);
            printf("请重新输入新密码：");
            scanf("%s", mm);
        }
        strcpy(current->mima, mm);
    }
}
//修改学生成绩
void change_xx32(struct student*head_xsxx,int n) {
    struct student* current;
    struct subject* current1;
    int p;
    current = head_xsxx;
    while (current->next != NULL) {
        current = current->next;
        printf("姓名：%-10s",current->name);
        current1 = current->first;
        p = n;
        while (p--) {current1 = current1->next;}
        printf("%s成绩：%d\n", current1->kmname, current1->km);
        printf("请输入%s成绩：", current1->kmname);
        scanf("%d", &current1->km);
    }
}

//学生板块

//学生信息文件读取
void input_xx3(struct student* head) {
    FILE* fp;
    int cj;
    struct student* current;
    struct subject* current1;
    char name[30];
    char n[7];
    if ((fp = fopen("student_xx.txt", "r")) == NULL)
        return;
    current = head;
    while (fscanf(fp, "%s", name) != EOF) {
        current->next = (struct student*)malloc(sizeof(struct student));
        current = current->next;
        strcpy(current->name, name);
        fscanf(fp, "%s", current->number);
        fscanf(fp, "%s", current->sex);
        fscanf(fp, "%s", current->major);
        fscanf(fp, "%s", current->telephone);
        fscanf(fp, "%s", current->home);
        current->first = (struct subject*)malloc(sizeof(struct subject));
        current->first->next = NULL;
        current1 = current->first;
        while (fscanf(fp, "%d", &cj) != 0) {
            current1->next= (struct subject*)malloc(sizeof(struct subject));
            current1 = current1->next;
            current1->km = cj;
            fscanf(fp, "%s", current1->kmname);
            current1->next = NULL;
        }
        fscanf(fp, "%s", n);
        current->next = NULL;
    }
    fclose(fp);
}
//学生账户信息文件读取
void input_zh3(struct Mima* head) {
    FILE* fp;
    struct Mima* current;
    char account[9];
    if ((fp = fopen("student_zh.txt", "r")) == NULL)
        return;
    current = head;
    while (fscanf(fp, "%s", account) != EOF) {
        current->next = (struct Mima*)malloc(sizeof(struct Mima));
        current = current->next;
        strcpy(current->account, account);
        fscanf(fp, "%s", current->mima);
        current->next = NULL;
    }
    fclose(fp);
}
//学生账户文件保存
void save_zh3(struct Mima* head) {
    FILE* fp;
    struct Mima* current;
    if (head->next == NULL)return;
    fp = fopen("student_zh.txt","w");
    while (fp == NULL)fp = fopen("student_zh.txt", "w");
    current = head;
    while (current->next != NULL) {
        current=current->next;
        fprintf(fp, "%s %s\n", current->account, current->mima);
    }
    fclose(fp);
}
//学生信息文件保存
void save_xx3(struct student* head) {
    FILE* fp;
    struct student* current;
    struct subject* current1; 
    if (head->next == NULL) {
        printf("没有学生信息");
        return;
    }
    fp = fopen("student_xx.txt", "w");
    while (fp == NULL)fp = fopen("student_xx.txt", "w");
    current = head;
    while (current->next != NULL) {
        current = current->next;
        fprintf(fp, "%s ", current->name);
        fprintf(fp, "%s ", current->number);
        fprintf(fp, "%s ", current->sex);
        fprintf(fp, "%s ", current->major);
        fprintf(fp, "%s ", current->telephone);
        fprintf(fp, "%s ", current->home);
        current1 = current->first;
        while (current1->next != NULL) {
            current1 = current1->next;
            fprintf(fp, "%d ", current1->km);
            fprintf(fp, "%s ", current1->kmname);
        }
        fprintf(fp, "e\n");
    }
    fclose(fp);
}
//管理主界面，学生
void Administrator3(struct Mima* current) {
    int i, n = 39;
    printf("\n\n");
    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("%s，欢迎登录", current->account);
    for (i = 0; i < 8; i++) {
        printf("+");
    }
    printf("\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|         1 修改学生密码         |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|         2 修改学生信息         |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|         3 查看学生信息         |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    printf("|         q 退出                |\n");

    for (i = 0; i < n; i++) {
        printf(" ");
    }
    for (i = 0; i < 34; i++) {
        printf("+");
    }
    printf("\n");
}
//控制面板，学生
void control3(struct student* head_xsxx, struct Mima* head_xsmm, struct teacher* head_tcxx, struct Mima* head_tcmm, int count_tc) {
    char input[9];
    int t = 0;
    char ch;
    struct Mima* current;
    struct student* current1;
    printf("\n\n\n\n\n\n\n\t\t\t\t\t请输入学生账号(q退出)：");
    scanf("%s", input);
    current = head_xsmm;
    current1 = head_xsxx;
    while (current->next != NULL && current1->next != NULL) {
        current = current->next;
        current1 = current1->next;
        while (!strcmp(current->account, input)) {
            printf("请输入密码：");
            scanf("%s", input);
            if (!strcmp(current->mima, input)) {
                t = 1;
                break;
            }
        }
        if (t == 1)break;
    }
    if (t == 0) {
        printf("查无此人");
        system("pause");
        return;
    }
    Administrator3(current);
    while (ch = getche()) {
        switch (ch) {
        case '1':system("cls"); change_zh33(current); save_zh3(head_xsmm); break;
        case '2':system("cls"); change_xx33(current1); save_xx3(head_xsxx); break;
        case '3':system("cls"); check_xx33(current1,count_tc); break;
        case 'q':return;
        default:printf("请重新输入：\n"); system("pause"); break;
        }
        system("cls");
        Administrator3(current);
    }
}
//修改学生密码
void change_zh33(struct Mima* current) {
    char input[9];
    char mm[9];
    printf("请输入旧密码：");
    scanf("%s", input);
    if (!strcmp(input, current->mima)) {
        printf("请输入新密码：");
        scanf("%s", input);
        printf("请重新输入新密码：");
        scanf("%s", mm);
        while (strcmp(input, mm)) {
            printf("密码错误\n");
            printf("请输入新密码：");
            scanf("%s", input);
            printf("请重新输入新密码：");
            scanf("%s", mm);
        }
        strcpy(current->mima, mm);
    }


}
//修改学生信息
void change_xx33(struct student* current) {
    printf("请输入你的性别：");
    scanf("%s", current->sex);
    printf("请输入你的专业：");
    scanf("%s", current->major);
    printf("请输入你的电话：");
    scanf("%s", current->telephone);
    printf("请输入你的家庭住址：");
    scanf("%s", current->home);
}
//查看学生信息，学生
void check_xx33(struct student* current, int p) {
    int nlong;
    struct subject* current1;
    printf("姓名：%-30s性别：%s\n学号：%-30s专业：%s\n电话：%-30s家庭住址：%s\n", \
        current->name, current->sex, current->number, current->major, current->telephone, current->home);
    printf("+");
    nlong = 12 * p;
    while (nlong--)printf("-");
    printf("+\n");
    printf("|");
    current1 = current->first;
    while (current1->next != NULL) {
        current1 = current1->next;
        printf("%-12s", current1->kmname);
    }
    printf("|\n");
    printf("+");
    nlong = 12 * p;
    while (nlong--)printf("-");
    printf("+\n");
    printf("|");
    current1 = current->first;
    while (current1->next != NULL) {
        current1 = current1->next;
        printf("%-12d", current1->km);
    }
    printf("|\n");
    printf("+");
    nlong = 12 * p;
    while (nlong--)printf("-");
    printf("+\n");
    system("pause");
}
```