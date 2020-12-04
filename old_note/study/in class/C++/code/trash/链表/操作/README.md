#include <stdio.h>
#include <stdlib.h>
struct link *AppendNode(struct link *head);
void DisplyNode(struct link *head);
void DeleteMemory(struct link *head);
struct link *DeleteNode(struct link *head, int nodeData);
struct link
{
	int data;
	struct link *next;
};
int main(){
	struct link *head = NULL;
	head = AppendNode(head);
	head = AppendNode(head);
	DisplyNode(head);
	printf("一共有%d个\n",cal(head));
	head = DeleteNode(head,3);
	DisplyNode(head);
	printf("一共有%d个\n",cal(head));
	return 0;
}
int cal(struct link*head){
	int i = 0;
	struct link *p = head;
	while(p!=NULL){
		i++;
		p = p->next;
	}
	return i;
}
/* 函数功能：新建一个节点并添加到链表末尾，返回添加节点后的链表的头指针 */
struct link *AppendNode(struct link *head)
{
	struct link *p = NULL;
	struct link *pr = head;
	p = (struct link *)malloc(sizeof(struct link)); /* 让p指向新建节点 */
	printf("Input node data:");
	p->next = 0;
	scanf("%d", &p->data);        	 /* 输入节点数据 */
	if (head == NULL) 	           /* 若原链表为空表，则将新建节点置为首节点 */
	{
		head = p; 		
	}
	else                  	           /* 若原链表为非空，则将新建节点添加到表尾 */
	{		
		while (pr->next != NULL)/* 若未到表尾，则移动pr直到pr指向表尾 */
		{
			pr = pr->next;     /* 让pr指向下一个节点 */
		}
		pr->next = p; 			 /* 将新建节点添加到链表的末尾 */
	}
	return head;               	 /* 返回添加节点后的链表的头节点指针 */
}
void DisplyNode(struct link *head)
{
	struct link *p = head;
	printf("SHOW:\n");
	while (p != NULL){
		printf("%d\n", p->data);
		p = p->next;
	}                 
}
struct link *DeleteNode(struct link *head, int nodeData){
	struct link *p = head;//当前节点
	struct link *pr = head;//前一个节点
	while (p != NULL){
		if(nodeData == p->data){
			if (p == head){
					head = p->next;
					pr = head;
			}		
			else{
					pr->next = p->next;				
			}		
			free(p);
			p = head;
		}
		else{
			pr = p;
			p = p->next;
		}
	}
	return head;
}
