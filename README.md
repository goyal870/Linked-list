//Program is for implementing linked list and File handling
//Program aims to get and store information of individual like firstname and lastname as record and store name in file as well as in linkedlist
//Node of linkedlist contains firstname, lastname and pointer to next node
//File used for read and write is records.txt
# include <stdio.h> 
# include <string.h> 
#include <stdlib.h>
struct Node
{
    char firstName[30];
	char lastName[30];
    struct Node* next;
};
struct Node* insertAtLast(struct Node* head, char firstname[], char lastname[]) 
{
	//Function to insert record at the last position of linked list
	struct Node* node=(struct Node*)malloc(sizeof(struct Node));
	struct Node *last = head;
	strcpy(node->firstName,firstname);
	strcpy(node->lastName,lastname);
	node->next = NULL;
	if(head==NULL) 
	{ 
		head=node;
		return head;
	}
	while(last->next!=NULL) 
	{
		last=last->next;
	}
	last->next=node;
	return head;
}

void printLinkedList(struct Node* root) 
{
	//function to print linked list
    while(root!=NULL)
	{
		printf("(");
        printf("%s %s", root->firstName,root->lastName);
        root=root->next;
        if(root!=NULL)
        printf(")->");
        else
        printf(")");
    }
    printf("\n");
} 

void readFromFile(int n)
{
	//function to open and read file and printing its content on console
	FILE *file;
	char firstName[32];
	char lastName[32];
	int i=0;
	file=fopen("records.txt","rt");
	if(!file)
	{
		printf("File could not be opened\n\a\a");
		return ;
	}
	while(!feof(file) && i!=n)
	{
		fscanf(file, "%s\t%s", firstName, lastName);
		printf("Record (#%d): %s %s\n",i+1,firstName,lastName);
		i++;
	}
	fclose(file);
	printf("\n");
}
int main()
{
	struct Node* head=NULL;										//Initialising head of linked list as NULL
	FILE *file;
	int i,n,choice;
	char firstName[30];
	char lastName[30];
	printf("Enter no. of records to save\n");   				// Number of records to save
	scanf("%d",&n);												// Accept value of n from user
	file = fopen("records.txt", "wt");							// opened the file records.txt
	if(!file)
	{
		printf("File could not be opened\n\a\a");
		getchar();
		return -1;
	}
	for(i=0;i<n;i++)
	{
		printf("Record #%d\n",i+1);									// Read data from user
		printf("Enter first name: ");
		scanf("%s",firstName);
		printf("Enter last name:  ");
		scanf("%s",lastName);
		printf("\n");
		
		head=insertAtLast(head,firstName,lastName);					// Inserting record in LinkedList
		
		fprintf(file, "%s\t%s\n", firstName, lastName);				//Saving record in file
	}
	
	fclose(file);													//Closing the file
	
	while(1)														//While loop to get users choice repeatedly
	{
		printf("Enter 1 to read from file\n");
		printf("Enter 2 to display Linked List\n");
		printf("Enter 3 to exit\n");
		scanf("%d",&choice);
		printf("\n");
		if(choice==1)
		readFromFile(n);											//FUnction call to read from file
		else if(choice==2)
		printLinkedList(head);										//Function call to print Linked List
		else if(choice==3)
		return 0;													//Exiting from program
		printf("\n\n");
	}
	return 0;
}


