void deleteNode(char content[20]){                                                      
    struct address_t *node = head;
    struct address_t *prev = NULL;
    if(node==NULL){
        return;
    }
    if (node != NULL && strcmp(toLowerStr(node->alias),toLowerStr(content))==0) {
        head = node->next;
        free(node);
        return;
    }
    while (node != NULL && strcmp(toLowerStr(node->alias),toLowerStr(content))!=0) {
            prev = node;
            node = node->next;
    }
    prev->next = node->next;
    free(node);
}

void updateNode(char content[20], int address[4]){                                       
    struct address_t *node = head;
    while(node != NULL){
        if(strcmp(toLowerStr(node->alias),toLowerStr(content))==0){
            node->part[0]=*(address);
            node->part[1]=*(address+1);
            node->part[2]=*(address+2);
            node->part[3]=*(address+3);
        }
    node = node->next;
    }
}

int searchNode(){                                                                        
    bool found=false;
    char content[20]="";
    printf("Please enter Alias : ");
    do{
        fflush(stdin);
        fgets(content, 20, stdin);
        content[strlen(content)-1]='\0';
        if(validateAlias(content)){                                                       
            struct address_t *node = head;
            while(node != NULL){
                if(strcmp(toLowerStr(node->alias),toLowerStr(content))==0){
                    printf("Alias found!!\n");
                    printf("%d",node->part[0]);
                    printf(".");
                    printf("%d",node->part[1]);
                    printf(".");
                    printf("%d",node->part[2]);
                    printf(".");
                    printf("%d",node->part[3]);
                    printf(" ");
                    printf("%s\n",node->alias);
                    found=true;
                }
                node = node->next;
            }
            if(!found){
                printf("Alias not found.\n");
            }
        }
        else{
            printf("Please enter valid alias : ");
        }
    }while(!validateAlias(content));
    return 0;
}

int addNode(){                                                                           
    char ipStr[25];
    char str[35]="";
    int address[4];
    char content[20];
    printf("Enter complete address : ");
    do{
        fflush(stdin);
        fgets(ipStr, 25, stdin);
        sscanf(ipStr, "%d.%d.%d.%d", address, address+1, address+2, address+3);
        if(validateAddress(address)){                                                      
            printf("Please enter Alias : ");
            do{
                fflush(stdin);
                fgets(content, 20, stdin);
                content[strlen(content)-1]='\0';
                if(validateAlias(content)){                                               
                    if(checkUnique(address,content)){                                    
                        strcat(ipStr," ");
                        strcat(ipStr,content);
                        initialListCreate(ipStr);
                    }
                    else{
                        printf(" already exists!!\n");
                    }
                }
                else{
                    printf("Please enter valid name : ");
                }
            }while(!validateAlias(content));
        }
        else{
            printf("Please enter valid address : ");
        }
    }while(!validateAddress(address));
    return 0;
}
int printList(){                                                                            
    struct address_t *node = head;
    int count=0;
    printf("\nPrinting all records present::\n");
    while(node != NULL){
        printf("%d.%d.%d.%d %s\n", node->part[0], node->part[1], node->part[2], node->part[3], node->alias);
        count++;
        node = node->next;
    }
    if(count==0){
        printf("No record present\n\n");
    }
    else{
        printf("Displaying %d result(s).\n\n",count);
    }
    return 0;
}

struct address_t* listCreate(char inputStr[35]){                                            
    struct address_t *current = NULL;
    struct address_t *node=(struct address_t*)malloc(sizeof(struct address_t));
    if(NULL==node){
        printf("Exception while creating node\n");
        return NULL;
    }
    sscanf(inputStr, "%d.%d.%d.%d %s", node->part, node->part+1, node->part+2, node->part+3, node->alias);
    node->next = NULL;
    current = node;
    head = current;
    return head;
}

struct address_t* initialListCreate(char inputStr[35]){                                     
    if(NULL == head){
        return (listCreate(inputStr));
    }
    struct address_t *node = (struct address_t*)malloc(sizeof(struct address_t));
    if(NULL == head){
        printf("\nNode creation failed\n");
        return    NULL;
    }
    sscanf(inputStr, "%d.%d.%d.%d %s", node->part, node->part+1, node->part+2, node->part+3, node->alias);
    node->next = head;
    head = node;
    return head;
}