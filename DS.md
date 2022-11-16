- [第二章——线性表](#第二章线性表)
  - [2.3 线性表的链式表示](#23-线性表的链式表示)
    - [单链表](#单链表)
    - [双链表](#双链表)
- [第三章——栈、队列和数组](#第三章栈队列和数组)
  - [3.1 栈](#31-栈)
    - [栈的顺序存储（顺序栈）](#栈的顺序存储顺序栈)
      - [共享栈](#共享栈)
    - [栈的链式存储（链式栈）](#栈的链式存储链式栈)
    - [应用——括号匹配问题](#应用括号匹配问题)
    - [应用——后缀表达式](#应用后缀表达式)
    - [应用——递归](#应用递归)
  - [3.2 队列](#32-队列)
    - [顺序实现](#顺序实现)
    - [链式实现](#链式实现)
    - [双端队列](#双端队列)
    - [应用——树的层次遍历](#应用树的层次遍历)
    - [应用——图的广度优先遍历](#应用图的广度优先遍历)
    - [应用——操作系统 FCFS(先来先服务)](#应用操作系统-fcfs先来先服务)
- [第四章——串](#第四章串)
  - [4.1 串的定义和实现](#41-串的定义和实现)
    - [串的顺序存储](#串的顺序存储)
    - [串的链式存储](#串的链式存储)
  - [4.2 串的模式匹配](#42-串的模式匹配)
    - [朴素模式匹配（定位操作）](#朴素模式匹配定位操作)
    - [KMP](#kmp)
    - [KMP算法优化——nextval数组](#kmp算法优化nextval数组)
- [第五章——树和二叉树](#第五章树和二叉树)
  - [二叉树的顺序存储](#二叉树的顺序存储)
  - [二叉树的链式存储](#二叉树的链式存储)
  - [二叉树的线索化](#二叉树的线索化)
  - [线索二叉树找前驱（后继）](#线索二叉树找前驱后继)
  - [树的存储方式](#树的存储方式)
  - [二叉排序树](#二叉排序树)
- [第六章——图](#第六章图)
  - [图的概念](#图的概念)

# 第二章——线性表
## 2.3 线性表的链式表示
### 单链表
```c++
#include <iostream>
using namespace std;

typedef struct LNode{   //定义单链表结点类型
    ElemType data;      //数据域
    struct LNode *next; //指针域
}LNode,*LinkList;

LinkList createList_Head(LinkList &L){  //使用头插法建立单链表
    LNode *s;
    int x;
    L=(LinkList)malloc(sizeof(LNode));  //创建头结点
    L->next=NULL;                       //初始为空链表
    cin>>x;
    while (x!=9999)
    {
        s=(LNode*)malloc(sizeof(LNode));
        s->data=x;
        s->next=L->next;
        L->next=s;
        cin>>x;
    }
    return L;
}

LNode *GetElem(LinkList L,int i){   //使用 下标 返回 该单链表结点
    int j=1;
    LNode *p=L->next;
    if(i==0)
        return L;
    if(i<1)
        return NULL;
    while(p&&j<i){
        p=p->next;
        j++;
    }
    return p;
}

LNode *LocateElem(LinkList L,int e){//使用 值data 返回 该点链表结点
    LNode *p=L->next;
    while(p!=NULL&&p->data!=e)
    {
        p=p->next;
    }
    return p;
}

LinkList InsertElem(LinkList &L,LNode *s,int n){
    LNode *p=GetElem(L,n-1);
    s->next=p->next;
    p->next=s;
    return L;
}

int main(){
    LinkList L;
    createList_Head(L);
    cout<<L;
    return 0;
}
```

### 双链表
```c++
#include <iostream>
using namespace std;

typedef struct DNode{           //定义双链表结点类型
    ElemType data;              //数据域
    struct DNode *prior,*next;  //前驱和后驱指针
}DNode,*DLinkList;

bool initDLinkList(DLinkList &L){
    L = (DNode*)malloc(sizeof(DNode));//分配一个头结点
    if(L==NULL)
        return false;
    L->piror=NULL;                    //头结点的piror永远指向NULL
    L->next=NUll;                     //头结点之后暂时没有结点
    return true;
}

bool insertNextDNode(DNode *p,DNode *s){ //在P节点后插入S结点
    if(p==NULL || s==NULL)
        return false;
    s->next=p->next;
    if(p->next != NULL)                  //如果p有后继结点
        p->next->prior=s;
    s->prior=p;    
    p->next=s;
    return true;
}

bool deleteNextDNode(DNode *p){ //删除p结点的后继结点 
    if(p==NULL) return false;
    DNode *q=p->next;           //找到p结点的后继结点q
    if(q==NULL) return false;   //p没有后继结点，q为空
    p->next=q->next;            
    if(q->next!=NULL)           //q不是最后一个结点
        q->next->prior=p;       
    free(q);                    //释放结点空间
    return true;
}

void DestroyDLinkList(DLinkList &L){    //循环释放各个数据结点
    while(L->next!=NULL)
        deleteNextDNode(L);
    free(L);
    L=NULL;
}

void checkAllDNode(){
    while(p!=NULL){         //后向遍历
        p=p->next;
    }
    while(p!=NULL){         //前向遍历
        p=p->prior;
    }
    while(p->prior!=NULL){  //前向遍历（跳过头结点）
        p=p->prior;
    }
}

void testDLinkList(){
    DLinkList L;
    initDLinkList(L);
    ......
}
```

# 第三章——栈、队列和数组
## 3.1 栈
### 栈的顺序存储（顺序栈）
```c++
#include <iostream>
using namespace std;

#define MaxSize 10      //定义栈中元素的最大个数
typedef struct{
    int data[MaxSize];  //静态数据存放栈中元素
    int top;            //栈顶指针
}SqStack;

void InitStack(SqStack &S){
    S.top = -1;             //初始化栈顶指针
}

bool StackEmpty(SqStack S){ //判断栈空
    if(S.top == -1)
        return true;        //栈空
    else
        return false;       //不空
}

bool Push(SqStack &S,int x){//进栈
    if(S.top == MaxSize-1)  //栈满
        return false;
    S.top = S.top + 1 ;     //指针+1
    S.data[S.top] = x;      //新元素进栈
    return true;
}

bool Pop(SqStack &S,int x){ //出栈
    if(S.top == -1)
        return false;       //栈空，报错
    x = S.data[S.top];      //栈顶元素出栈
    S.top = S.top - 1;      //指针-1
    return true;
}

//读栈顶指针
bool GetTop(SqStack S,int &x){
    if(S.top == -1)
        return false;
    x = S.data[S.top];      //x 记录栈顶元素
    return true;
}
```

#### 共享栈
```c++
#include <iostream>
using namespace std;

#define MaxSize 100     //定义栈中元素的最大个数
typedef struct{
    int data[MaxSize];  //静态数据存放栈中元素
    int top1;           //栈顶指针1
    int top2;           //栈顶指针2
}SqDouStack;

void InitStack(SqDouStack &S){
    S.top1 = -1;             //初始化栈顶指针
    S.top2 = MaxSize;
}

bool StackEmpty(SqDouStack S){ //判断栈空
    if(S.top1 == -1 && S.top2 == MaxSize)
        return true;        //栈空
    else
        return false;       //不空
}

bool Push(SqDouStack &S,int x,int StackNum){//进栈
    if(S.top1 + 1 == S.top2)  //栈满
        return false;
    if(StackNum == 1){
        S.top1 = S.top1 + 1;     //1指针+1
        S.data[S.top1] = x;      //新元素进栈1
    }
    else{
        S.top2 = S.top2 - 1;     //2指针-1
        S.data[S.top1] = x;      //新元素进栈2      
    }
    return true;
}

bool Pop(SqDouStack &S,int x,int StackNum){ //出栈
    if(StackNum == 1){
        if(S.top1 == -1)
            return false;       //栈空，报错
        x = S.data[S.top1];      //栈顶元素出栈
        S.top1 = S.top1 - 1;      //指针-1
        return true;
    }
    else{
        if(S.top2 == MaxSize)
            return false;       //栈空，报错
        x = S.data[S.top2];      //栈顶元素出栈
        S.top2 = S.top2 + 1;      //指针-1
        return true;
    }
}

//读栈顶指针
bool GetTop(SqDouStack S,int &x,int StackNum){
    if(StackNum == 1){
        if(S.top1 == -1)
            return false;
        x = S.data[S.top1];      //x 记录栈顶元素
        return true;
    }
    else{
        if(S.top2 == MaxSize)
            return false;
        x = S.data[S.top2];      //x 记录栈顶元素
        return true;
    }

}
```

### 栈的链式存储（链式栈）
```c++
#include <iostream>
using namespace std;

typedef struct Linknode{
    ElemType data;
    struct Linknode *next;
}*LiStack;

typedef struct LinkStack{
    LiStack top;
    int count;
}LinkStack;

bool Push(LinkStack *s, int x){
    LiStack p = (LiStack)malloc(sizeof(SNode));
    p -> data = x;
    p -> next = s -> top;
    s -> top = p;
    S -> count ++;
    return true; 
}

bool Pop(LinkStack *s, int& x){
    if(s -> top == NULL)
        return false;
    x = s->top->data;
    SLink p = s->top;
    s -> top = s->top->next;
    free(p);
    s->count--;
    return true;
}
```

### 应用——括号匹配问题
```c++
#include <iostream>
using namespace std;

#define MaxSize 10
typedef struct{
    char data[MaxSize];
    int top;
}SqStack;

// 初始化栈
void InitStack(SqStack &S)

// 判断栈是否为空
bool StackEmpty(SqStack S)

// 新元素入栈
bool Push(SqStack &S,char x)

// 栈顶元素出栈，用x返回
bool Pop(SqStack &S,char &x)

bool bracketCheck(char str[],int length){
    SqStack S;
    InitStack(S);                   // 初始化一个栈
    for(int i=0;i<length;i++){
        if(str[i]=='(' || str[i]=='{' ||str[i]=='['){
            Push(S,str[i])          // 扫描到左括号，入栈操作
        }else{
            if(StackEmpty(S)){      // 扫描到右括号，然而栈空，匹配错误
                return false;
            }

            char topElem;
            Pop(S,topElem);         // 栈顶元素出栈
            if(str[i]==')' && topElem!='(')
                return false;
            if(str[i]==']' && topElem!='[')
                return false;
            if(str[i]=='}' && topElem!='{')
                return false;
        }
    }
    return StackEmpty(S);           // 检索完全部括号后，栈空说明匹配成功
}
```

### 应用——后缀表达式
```c++

```

### 应用——递归
```c++
#include<iostream>
using namespace std;

int Fib(int n){
    if(n==0)
        return 0;
    else if(n==1)
        return 1;
    else
        return Fib(n-1)+Fib(n-2);
}

int main(){
    int x=Fib(4);
    cout<<x<<"  奥里给！！！"<<endl;
    return 0;
}
```

## 3.2 队列
### 顺序实现
```c++
#include <iostream>
using namespace std;
#define MaxSize 10
typedef struct SqQuene{
    int data[MaxSize];
    int front,rear;
};

//初始化队列
void InitQueue(SqQuene& Q){
    Q.front = Q.rear = 0;
}

//判断队列是否为空
bool QueueEmpty(SqQuene Q){
    if(Q.front == Q.rear)
        return true;
    else
        return false;
}

//入队
bool EnQueue(SqQuene &Q,int x){
    if((Q.rear+1)%MaxSize == Q.front) //判断队满
        return false;
    Q.data[Q.rear] = x;               //新元素插入队尾
    Q.rear = (Q.rear + 1)%MaxSize;    //队尾指针加1取模
    return true;
}

//出队
bool DeQueue(SqQuene &Q,int &x){
    if(Q.front==Q.rear)         //队空则报错
        return false;
    x = Q.data[Q.front];
    Q.front=(Q.front + 1)%MaxSize;
    return true;
}

//获得队头元素的值,用x返回
bool GetHead(SqQuene Q,int& x){
    if(Q.front == Q.rear)
        return false;       //队空则报错
    x = Q.data[Q.front];
    return true;
}
```

### 链式实现
```c++
#include <iostream>
using namespace std;

typedef struct LinkNode{    //队列结点
    int data;
    struct LinkNode *next;
}LinkNode;

typedef struct{             //链式队列
    LinkNode *front,*rear;  //队头和队尾指针
}LinkQueue;

// 初始化队列(带头结点)
void InitQueue(LinkQueue &Q){
    // 初始化,front rear 都指向头结点
    Q.front = Q.rear = (LinkNode *)malloc(sizeof(LinkNode));
    Q.front->next = NULL;
}

// 初始化队列（不带头结点）
void InitQueue(LinkQueue &Q){
    // 初始化,front rear 都指向NULL
    Q.front = Q.rear = NULL;
}

// 判断队列是否为空(带头结点)
bool IsEmpty(LinkQueue Q){
    if(Q.front == Q.rear)
        return true;
    else
        return false;
}

// 判断队列是否为空（不带头结点）
bool IsEmpty(LinkQueue Q){
    if(Q.front == NULL)
        return true;
    else
        return false;
}

// 新元素入队（带头结点）
void EnQueue(LinkQueue &Q,int x){
    LinkNode *s=(LinkNode *)malloc(sizeof(LinkNode));
    s->data = x;
    s->next = NULL;
    Q.rear->next = s;   //新结点插入到rear之后
    Q.rear = s;         //修改队尾指针
}

// 新元素入队（不带头结点）
void EnQueue(LinkQueue &Q,int x){
    LinkNode *s = (LinkNode*)malloc(sizeof(LinkNode));
    s->data = x;
    s->next = NULL;
    if(Q.front==NULL){      //在空队列中插入第一个元素
        Q.front = s;
        Q.rear = s;
    }else{
        Q.rear->next = s;   //新结点插入到rear结点之后
        Q.rear = s;         //修改rear队尾指针
    }
}

// 队头元素出队（带头结点）
bool DeQueue(LinkQueue& Q,int& x){
    if(Q.front == Q.rear)       // 队空
        return false;
    LinkNode *p = Q.front->next;// p指向此次出队的队头结点
    x = p->data;                // 用 x 返回队头的数据
    Q.front->next = p->next;    // 修改头结点的next指针

    if(Q.rear == p)             // 如果是最后一个结点出队
        Q.rear = Q.front;       // 修改 rear 指针 
    free(p);                    // 释放结点空间
    return true;
}

// 队头元素出队（不带头结点）
bool DeQueue(LinkQueue& Q,int& x){
    if(Q.front == NULL)
        return false;       // 队空
    LinkNode *p = Q.front;  // p指向此次出队的队头结点
    x = p->data;            // 用变量 x 返回队头的数据
    Q.front = p->next;      // 修改 front 指针
    if(Q.rear == p){        // 倘若是队中最后一个结点出队
        Q.front = NULL;     // front 指向 NULL
        Q.rear = NULL;      // rear 指向 NULL
    }
    free(p);                // 释放结点空间
    return true;
}

```


### 双端队列
```c++

```

### 应用——树的层次遍历
```c++

```

### 应用——图的广度优先遍历
```c++

```

### 应用——操作系统 FCFS(先来先服务)
```c++

```

# 第四章——串
## 4.1 串的定义和实现
### 串的顺序存储
```c++
#include<iostream>
using namespace std;

#define MAXLEN 255
typedef struct SString{
    char ch[MAXLEN];
    int length;
};

typedef struct HString{
    char *ch;
    int length;
};

void initHString(HString &s){
    S.ch=(char *)malloc(MAXLEN*sizeof(char));
    S.length=0;
}

//赋值操作，将串T赋值为chars
bool StrAssign(SString &T,chars){

}

//复制操作，将串S复制得到串T
bool StrCopy(){

}

//判空操作，S空串返回TRUE，否则False
bool StrEmpty(){
    
}

//求串长，返回串的元素个数
bool StrLength(){
    
}

//求子串，用Sub返回串S的第pos个字符起长度为len的子串
bool SubString(SString &Sub,SString S,int pos,int len){
    if(pos+len-1 > S.length)        //子串
        return false;
    for(int i = pos;i<pos+len;i++)
        Sub.ch[i-pos+1] = S.ch[i];
    Sub.length = len;
    return true;
}

//比较操作,若S>T,则返回值>0;若S=T,则返回值=0;若S<T,则返回值<0
int StrCompare(SString S,SString T){
    for(int i=1; i<S.length && i<T.length; i++){
        if(S.ch[i]!=T.ch[i])
            return S.ch[i]-T.ch[i];
    }
    cout<<"字符串长度不相等,其差值为"<<S.length-T.length;
    return S.length-T.length;
}

//定位操作。若主串S中存在与串T值相同的子串，则返回它在主串S中第一次出现的位置，否则函数值为0
int Index(SString S,SString T){
    int i=1;
    SString sub;
    while(i<=S.length-T.length+1){
        SubString(sub,S,i,T.length);
        if(StrCompare(sub,T)!=0)
            ++i;
        else
            return i;
    }
    return 0;
}

//清空操作，将S清空为空串
bool ClearString(){
    
}

//销毁操作，回收存储空间
bool DestoryString(){

}

//串连接，用T返回由S1和S2联结而成的新串
bool Concat(){

}
```

### 串的链式存储
```c++
#include<iostream>
using namespace std;

#define MAXLEN 255
typedef struct StringNode{
    char ch;        //每个结点存储1个字符，存储密度低，每个字符1B，每个指针4B
    char ch[4];     //存储密度提高
    struct StringNode *next;
}StringNode,*String;

```

## 4.2 串的模式匹配

### 朴素模式匹配（定位操作）

主串的扫描指针 i 经常回溯，导致时间开销增加，<font color='red'>最坏时间复杂度 O ( n m ) </font>

```c++
int Index(SString S,SString T){
    int k=1;
    int i=k,j=1;
    while(i<=S.length&&j<=T.length){
        if(S.ch[i]==T.ch[j]){
            ++i;
            ++j;        //继续比较后继字符
        }else{
            k++;        //检查后一个字符串
            i=k;
            j=1;
        }
        if(j>T.length)
            return k;
        else
            return 0;
    }
}
```

### KMP

改进思路：当子串和模式串不匹配时，主串指针不回溯，只有模式串指针回溯 j = next [ j ]

<font color='red'>算法平均时间复杂度：O ( n + m )</font>

```c++
int Index_KMP(SString S,SString T,int next[]){
    int i=1,j=1;
    while(i<=S.length&&j<=T.length){
        if(j==0 || S.ch[i]==T.ch[i]){
            ++i;
            ++j;
        }else{
            j=next[j];
        }
        if(j>T.length)
            return i-T.length;
        else
            return 0; 
    }
}
```
💡 next数组：当模式串的第 j 个字符匹配失败时，令模式串跳到 next [ j ] 再继续匹配   
模式串向右移动看匹配程度

> 串的前缀：包含第一个字符，且不包含最后一个字符的子串
串的后缀：包含最后一个字符，且不包含第一个字符的子串
> 

💡 当第 j 个字符匹配失败，由前 1 ～ j - 1 个字符组成的串记为 S ,则：  
next [ j ] = S 的最长相等前后缀长度 + 1  
>特别的，next [ 1 ] = 0 且 next [ 2 ] = 1


### KMP算法优化——nextval数组

# 第五章——树和二叉树

## 二叉树的顺序存储
```c++
#define MaxSize 100
struct TreeNode{
    ElemType value; //结点中的数据元素
    bool isEmpty;   //结点是否为空
};

TreeNode t[MaxSize];

void initTree{      //初始化
    for(int i=0;i<MaxSize;i++){
        t[i].isEmpty=true;
    }
}
```

## 二叉树的链式存储
```c++
struct ElemType{
    int value;
};

typedef struct BiTNode{
    ElemType data;                  //数据域
    struct BiTNode *lchild,*rchild; //左右孩子指针
    struct BiTNode *parent;         //父结点指针
}BiTNode,*BiTree;

BiTree root = NULL;

root = (BiTree)malloc(sizeof(BiTNode));
root -> data ={1};
root -> lchild = NULL;
root -> rchild = NULL;

// 先序遍历
void PreOrder(BiTree T){
    if(T != NULL){
        visit(T);           //访问根结点
        PreOrder(T->lchild);//递归遍历左子树
        PreOrder(T->rchild);//递归遍历右子树
    }
}

//中序遍历
void InOrder(BiTree T){
    if(T != NULL){
        InOrder(T->lchild);//递归遍历左子树
        visit(T);           //访问根结点
        InOrder(T->rchild);//递归遍历右子树
    }
}

//后序遍历
void PostOrder(BiTree T){
    if(T != NULL){
        PostOrder(T->lchild);//递归遍历左子树
        PostOrder(T->rchild);//递归遍历右子树
        visit(T);           //访问根结点       
    }
}

typedef struct BiTNode{
    char data;                  //数据域
    struct BiTNode *lchild,*rchild; //左右孩子指针
}BiTNode,*BiTree;

//链式队列结点
typedef struct LinkNode{
    BiTNode *data;
    struct LinkNode *next;
}LinkNode;

typedef struct{
    LinkNode *front,*rear; //队头队尾
}LinkQueue;

// 层序遍历
void LevelOrder(BiTree T){
    LinkQueue Q;
    InitQueue(Q);               //初始化辅助队列
    BiTree p;
    EnQueue(Q,T);               //将根结点入队
    while(!IsEmpty(Q)){         //队列不空则循环
        DeQueue(Q,p);           //队头结点出队
        visit(p);               //访问出队结点
        if(p->lchild!=NULL)
            EnQueue(Q,p->lchild);//左孩子入队
        if(p->rchild!=NULL)
            EnQueue(Q,p->rchild);//右孩子入队
    }
}
```

## 二叉树的线索化

- 用土办法找到中序前驱
```c++
#include <iostream>
using namespace std;

//中序遍历
void InOrder(BiTree T){
    if(T!=NULL){
        InOrder(T->lchild); //递归遍历左子树
        visit(T);           //访问根结点
        InOrder(T->rchild); //递归遍历右子树
    }
}

void visit(BiTNode *q){
    if(q==p)            //当前访问结点刚好是结点p
        final = pre;    //找到p的前驱
    else
        pre = q;        //pre指向当前访问的结点
}

//辅助的全局变量，用于查找结点p的前驱
BiTNode *p;         //p指向目标结点
BiTNode *pre=NULL;  //指向当前访问结点的前驱
BiTNode *final=NULL;//用于记录最终结果

```

- 中序线索化
```c++
#include <iostream>
using namespace std;

typedef struct ThreadNode{
    ElemType data;
    struct ThreadNode *lchild,*rchild;
    int ltag,rtag;      //左、右线索标志，初始化为0，假设都有左右孩子
}ThreadNode,*ThreadTree;

void InThread(ThreadTree T){
    if(T!=NULL){
        InThread(T->lchild);
        visit(T);
        InThread(T->rchild);
    }
}

void visit(ThreadNode *q){
    if(q->lchild == NULL){  //左子树为空，建立前驱线索
        q->lchild = pre;
        q->ltag = 1;
    }
    if(pre != NULL && pre->rchild == NULL){
        pre->rchild = q;    //建立前驱结点的后继线索
        pre->rtag = 1;
    }
    pre = q;
}

//全局变量 pre ，指向当前访问结点的前驱
ThreadNode *pre = NULL;

```

- 先序线索化
> 先序线索化 **有个坑**，在处理根结点之后，处理左孩子之前，需要判断改当前遍历结点的 **ltag** 标志是否为0，如果为0，则lchild不是前驱线索
>> 【综上是为了避免陷入遍历的 **死循环**】
```c++
#include <iostream>
using namespace std;

typedef struct ThreadNode{
    ElemType data;
    struct ThreadNode *lchild,*rchild;
    int ltag,rtag;      //左、右线索标志，初始化为0，假设都有左右孩子
}ThreadNode,*ThreadTree;

//先序遍历二叉树，一边遍历一边线索化
void PreThread(ThreadTree T){
    if(T!=NULL){
        visit(T);               //先处理根结点
        if(T->ltag == 0){
            PreThread(T->lchild);//如果lchild不是前驱线索
        }
        PreThread(T->rchild);
    }
}

void visit(ThreadNode *q){
    if(q->lchild == NULL){      //左子树为空，建立前驱线索
        q->lchild = pre;
        q->ltag = 1;
    }
    if(pre != NULL && pre->rchild == NULL){
        pre->rchild = q;        //建立前驱结点的后继线索
        pre->rtag = 1;
    }
    pre = q;
}

//全局变量 pre ，指向当前访问结点的前驱
ThreadNode *pre = NULL;
```

- 后序线索化
```c++
#include <iostream>
using namespace std;

//全局变量pre，指向当前访问结点的前驱
ThreadNode *pre=NULL;

//后序线索化二叉树T
void CreatePostThread(ThreadTree T){
    pre=NULL;                   //pre初始化为NULL
    if(T!=NULL){                //非空二叉树才能线索化
        PostThread(T);          //后续线索化二叉树
        if(pre->lchild==NULL){
            pre->rtag=1;        //处理遍历的最后一个结点
        }
    }
}

//后序遍历二叉树，一边遍历一边线索化
void PostThread(ThreadTree T){
    if(T!=NULL){
        PostThread(T->lchild);  //后序遍历左子树
        PostThread(T->rchild);  //后序遍历右子树
        visit(T);               //访问根结点
    }
}

void visit(ThreadNode *q){
    if(q->lchild==NULL){        //左子树为空，建立前驱线索
        q->lchild = pre;
        q->ltag=1;
    }
    if(pre!=NULL && p->rchild==NULL){
        pre->rchild = q;        //建立前驱结点的后继线索
        pre -> rtag = 1;
    } 
    pre = q;
}
```

## 线索二叉树找前驱（后继）

- 中序线索二叉树找中序后继
```c++
//找到以P为根的子树中，第一个被中序遍历的结点
ThreadNode *FirstNode(ThreadNode *p){
    while(p->ltag==0)       //循环寻找最左下结点（不一定是叶节点）
        p=p->lchild;
    return p;
}

//在中序线索二叉树中找到结点p的后继结点
ThreadNode *NextNode(ThreadNode *p){
    id(p->rtag==0)          //右子树的最左👈下结点
        return FirstNode(p->rchild);
    else                    //rtag==1，直接返回后继线索
        return p->rchild;
}

//对中序线索二叉树进行中序遍历（利用线索实现的非递归算法）
void InOrder(ThreadNode *T){
    for(ThreadNode *p=FirstNode(T);p!=NULL;p=NextNode(p))
        visit(p);
}
```

- 中序线索二叉树找中序前驱
```c++
//找到以p为根的子树中，最后一个被中序遍历的结点
ThreadNode *LastNode(ThreadNode *p){
    while(p->rtag==0)       //循环寻找最右👉下结点（不一定是叶节点）
        p=p->rchild;
    return p;
}

//在中序线索二叉树中找到结点p的前驱结点
ThreadNode *PreNode(ThreadNode *p){
    id(p->ltag==0)          //左子树的最右下结点
        return FirstNode(p->lchild);
    else                    //rtag==1，直接返回前驱线索
        return p->lchild;
}

//对中序线索二叉树进行逆向中序遍历（利用线索实现的非递归算法）
void RevInOrder(ThreadNode *T){
    for(ThreadNode *p=LastNode(T);p!=NULL;p=PreNode(p))
        visit(p);
}
```

## 树的存储方式
- 双亲表示法（顺序存储）
```c++
#define MAX_TREE_SIZE 100           //树中最多结点树

typedef struct{                     //树的结点定义
    ElemType data;                  //数据元素
    int parent;                     //双亲位置域
}PTNode;

typedef struct{                     //树的结构类型
    PTNode nodes[MAX_TREE_SIZE];    //双亲表示
    int n;                          //结点数
}PTree;
```

- 孩子表示法（顺序+链式存储）
```c++
struct CTNode{
    int child;                  //孩子结点在数组中的位置
    struct CTNode *next;        //下一个孩子
};

typedef struct{
    ElemType data;
    struct CTNode *firstChild;  //第一个孩子
}CTBox;

typedef struct{
    CTBox nodes[MAX_TREE_SIZE];
    int n,r;
}CTree;
```

- 孩子兄弟表示法（链式存储）
```c++
typedef struct CSNode{
    ElemType data;                          //数据域
    struct CSNode *firstChild,*nextSibling; //第一个孩子和右兄弟指针
}CSNode,*CSTree;
```

## 二叉排序树
```c++
//二叉排序树结点
typedef struct BSTNode{
    int key;
    struct BSTNode *lchild,*rchild;
}BSTNode,*BSTree;

//在二叉排序树中查找值为 key 的结点
BSTNode *BST_Search(BSTree T,int key){
    while(T!=NULL && key != T->key){    //若树空或等于根结点值，则结束循环
        if(key < (T->key))
            T=T->lchild;                //小于，则在左子树上查找
        else
            T=T->rchild;                //大于，则在右子树上查找
    }
    return T;
}

//在二叉排序树中查找值为 key 的结点（递归实现）
BSTNode *BSTSearch(BSTree T,int key){
    if(T==NULL)
        return NULL;    //查找失败
    if(key==T->key)
        return T;          //查找成功
    else if(key < T->key)
        return BSTSearch(T->lchild,key);    //在左子树中查找
    else
        return BSTSearch(T->rchild,key);    //在右子树中查找
}

//在二叉排序树插入关键字为 k 的新结点（递归实现）
int BST_Insert(BSTree &T,int k){
    if(T==NULL){                            //原树为空，新插入的结点为根结点
        T=(BSTree)malloc(sizeof(BSTNode));
        T->key = k;
        T->lchild=T->rchild=NULL;
        return 1;                               //返回 1 ，插入成功
    }
    else if(k==T->key)                    //树中存在相同关键字的结点，插入失败，返回 0
        return 0;
    else if( k < T->key)                   //插入到 T 的左子树 
        return BST_Insert(T->lchild,k);
    else                                        //插入到 T 的右子树
        return BST_Insert(T->rchild,k);
```

# 第六章——图


KMP优化算法没看  视频P37
线索二叉树找前驱后驱还没看完 视频P48