### 同样的介绍在对应的Java文档里，这里就直接解析代码吧。
## 不过我们需要知道一些东西，那就是指针的意识。
![这里写图片描述](http://img.blog.csdn.net/20170304234533568?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSmFja19fRnJvc3Q=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### 也就是我们的链表名就是头指针，它只是一个指向第一个结点的指针。在链表数据增删改查时，我们要时刻意识到这个！！所有操作的源头来自于这个！！
### 而且下面例子也要注意：例子是有头结点的！！
***
### 准备数据
```
//单链表存储结构 
#include<stdio.h>
#include<stdlib.h>
#define OK 1
#define ERROR 0

typedef int ElemType;
typedef int Status;


typedef struct Node{
    ElemType data;
    struct Node *next; 
} Node;
typedef struct Node *LinkList;
```
### 创建链表两种方式：头插法和尾插法（有头结点的插法）
```
//单链表的创建,随机产生n个随机数,建立带头结点的单链表(头插法) 
Status createListHead(LinkList *L, int n){
    LinkList p;
    int i;
    *L = (LinkList)malloc(sizeof(Node)); //建立链表 
    (*L)->next = NULL;  //定义一个头结点 
    for(i = 0; i < n; i++){     //生成结点 
        p = (LinkList)malloc(sizeof(Node));
        p->data = rand() % 100 + 1; //产生100以内的数字 
        p->next = (*L)->next;   
        (*L)->next = p; //插入到表头     
    }   
    return OK;  
}
```
```
//单链表的创建,随机产生n个随机数,建立带头结点的单链表(尾插法)
Status createListTail(LinkList *L,int n){
	LinkList pNew,q;//pNew作为分配的新结点暂存变量，q作为每次分配后的尾结点暂存变量 
	int i;
	*L=(LinkList)malloc(sizeof(Node));//建立以此结点新链表，创建一个结点 
	q=*L;
	for(i=0;i<n;i++){
		pNew = (LinkList)malloc(sizeof(Node));//产生一个新的节点
		pNew->data=2*i;//产生一个100以内的数字 
		q->next=pNew; //新节点添加到链表的末尾 
		q=pNew;	//新节点定义为尾节点 
	} 
	q->next=NULL;
	return OK;
}
```
### 返回线性表的长度
```
//获取链表的长度 
Status getListLength(LinkList L){
    int i = 0; //计数器
    while(L->next){
        L = L->next;
        i++;
    } 
    return i;
}
```
### 链表的读取
```
//单链表的读取,用e返回L中的第i个元素的值 , 0 < i < L.length
Status getElem(LinkList L,int i,ElemType *e){
	int j=1;
	LinkList p;
	p = L->next;//p为第一个节点
	while(p&&j<i){
		p=p->next;
		j++;
	}
	if(!p||i>j){//意思就是，如果想查的第i个超出了链表长i 
		return ERROR;
	}
	*e = p->data;
	return OK;
}
```
### 遍历单链表,输出值
```
void listTraverse(LinkList L){
	if(L->next==NULL){
		 printf("链表为空");
	}else{
		LinkList p;
		p=L->next;
//		while(p!=NULL){
		while(p){
			printf("%d ",p->data);
            p = p->next;  
		}
		printf("\n");   
	}
}
```
### 元素的插入：
```
//链表的插入 将e插入到第i个位置之前 0 < i < listLenght + 1 
//定位到第i个节点,生成一个新的节点,数据e保存在新的节点中,最后插入到链表中 
Status listInsert(LinkList *L, int i, ElemType e) {
	LinkList p,s;//p代表当前结点，s代表要插入的这个新结点 
	int j=1;
	p = *L; 
	while(p&&j<i){
		p=p->next;
		j++;
	}
	if(!p||i<j){
		return ERROR;
	}
	s=(LinkList)malloc(sizeof(Node));//生成一个新的节点
	s->data=e;
	s->next=p->next;
	p->next=s;
	return OK;
}
```
### 链表删除：
```
//链表删除 从链表中删除第i个元素,将删除的元素保存到e中 
Status ListDelete(LinkList *L, int i, ElemType *e){
	LinkList p,q;
	int j=1;
	p=*L;
	while(p&&j<i){
		p=p->next;
		j++;
	}
	if(!p||i<j){
		return ERROR;
	}
	q=p->next;
	*e=q->data;
	p->next=q->next;
	free(q);
	return OK;
}
```

### 删除所有的节点：
```
Status clearList(LinkList *L) {
	LinkList p,q;
	p=(*L)->next;
	while(p){
		q=p->next;
		free(p);
		p=q;
	}
	(*L)->next =NULL;
	return OK;
}
```
