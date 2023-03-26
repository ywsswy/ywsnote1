#include<stdio.h>  
#include<stdlib.h>
//二叉树结点  
struct BNode{  
    int data; 
    struct BNode *lchild,*rchild;  
};
 
int find(struct BNode *T,int key,struct BNode *f,struct BNode **p){
	if(T == NULL){
		*p = f;
		return 0;
	}
	else if(key == T->data){
		*p = T;
		return 1;
	}
	else if(key < T->data){
		return find(T->lchild,key,T,p);
	}
	else
		return find(T->rchild,key,T,p);
}
void add(struct BNode **T,int e){
	BNode * s = NULL;
	BNode * p = NULL;
	if(find(*T,e,NULL,&p) == 0){
		s = (BNode*)malloc(sizeof(BNode));
		s->data = e;
		s->lchild = NULL;
		s->rchild = NULL;
		if(p == NULL){
			*T = s;
		}
		else if(e < p->data){
			p->lchild = s;
		}
		else{
			p->rchild = s;
		}
	}
	return;
}
void inorder(BNode *T){
	if(T != NULL){
        inorder(T->lchild);
		printf("%d ",T->data);
        inorder(T->rchild);  
    }  
}
void preorder(BNode *T){
	if(T != NULL){
		printf("%d ",T->data);
		preorder(T->lchild);
		preorder(T->rchild);
	}
}
void postorder(BNode *T){
	if(T != NULL){
		postorder(T->lchild);
		postorder(T->rchild);
		printf("%d ",T->data);
	}
}
int main(){
    struct BNode * tree = NULL;
	add(&tree,45);
	add(&tree,24);
	add(&tree,53);
	add(&tree,12);
	add(&tree,90);
	postorder(tree);
    return 0;  
}
