# Data Structures and Applications Lab (C Programs)

Part A – Programs 1 to 6, written in C.

**Compile and run:**

```bash
gcc program.c -o program
./program
```

## Contents

1. [Library Book Management](#1-library-book-management)
2. [Stack Operations (Array)](#2-stack-operations-using-array)
3. [Printer Queue](#3-printer-queue)
4. [Singly Linked List of Student Records](#4-singly-linked-list-of-student-records)
5. [Binary Tree Creation and Traversals](#5-binary-tree-creation-and-traversals)
6. [Graph of Cities (DFS and BFS)](#6-graph-of-cities-using-adjacency-matrix)

---

## 1. Library Book Management

Structure `Book` with dynamic memory allocation for N books. Functions: `create()`, `display()`, `search()`, `issueBook()`, `returnBook()`, with a menu-driven interface.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define TITLE 80
#define NAME 60

typedef struct {
    int Book_ID;
    char Title[TITLE], Author[NAME];
    float Price;
    int available; /* 1 = Available, 0 = Issued */
} Book;

Book *books = NULL;
int n = 0;

void clearLine(void) {
    int c;
    while ((c = getchar()) != '\n' && c != EOF) {}
}

void readLine(char *s, int size) {
    if (fgets(s, size, stdin)) s[strcspn(s, "\n")] = '\0';
}

void create(void) {
    int i;
    printf("Number of books to add: ");
    if (scanf("%d", &n) != 1 || n <= 0) {
        n = 0;
        puts("Invalid number.");
        clearLine();
        return;
    }
    clearLine();
    free(books);
    books = (Book *)calloc((size_t)n, sizeof(Book));
    if (!books) {
        puts("Memory allocation failed.");
        n = 0;
        return;
    }
    for (i = 0; i < n; ++i) {
        printf("\nBook %d ID: ", i + 1);
        scanf("%d", &books[i].Book_ID);
        clearLine();
        printf("Title: ");
        readLine(books[i].Title, TITLE);
        printf("Author: ");
        readLine(books[i].Author, NAME);
        printf("Price: ");
        scanf("%f", &books[i].Price);
        clearLine();
        books[i].available = 1;
    }
}

int findBook(int id) {
    int i;
    for (i = 0; i < n; ++i)
        if (books[i].Book_ID == id) return i;
    return -1;
}

void showBook(Book b) {
    printf("%-8d %-24s %-18s %8.2f %s\n", b.Book_ID, b.Title, b.Author,
           b.Price, b.available ? "Available" : "Issued");
}

void display(void) {
    int i, count = 0;
    if (!books || n == 0) {
        puts("No book records. Add records first.");
        return;
    }
    puts("ID       Title                    Author             Price    Status");
    for (i = 0; i < n; ++i)
        if (books[i].available) {
            showBook(books[i]);
            count++;
        }
    if (!count) puts("No books are currently available.");
}

void search(void) {
    int id, i;
    printf("Enter Book ID: ");
    scanf("%d", &id);
    i = findBook(id);
    if (i < 0)
        puts("Book not found.");
    else {
        puts("ID       Title                    Author             Price    Status");
        showBook(books[i]);
    }
}

void issueBook(void) {
    int id, i;
    printf("Enter Book ID to issue: ");
    scanf("%d", &id);
    i = findBook(id);
    if (i < 0) puts("Book not found.");
    else if (!books[i].available) puts("Book is already issued.");
    else {
        books[i].available = 0;
        puts("Book issued successfully.");
    }
}

void returnBook(void) {
    int id, i;
    printf("Enter Book ID to return: ");
    scanf("%d", &id);
    i = findBook(id);
    if (i < 0) puts("Book not found.");
    else if (books[i].available) puts("Book is already available.");
    else {
        books[i].available = 1;
        puts("Book returned successfully.");
    }
}

int main(void) {
    int ch;
    do {
        puts("\n1.Add Book Records 2.Display Available Books 3.Search by ID");
        puts("4.Issue a Book 5.Return a Book 6.Exit");
        printf("Choice: ");
        if (scanf("%d", &ch) != 1) break;
        switch (ch) {
            case 1: create(); break;
            case 2: display(); break;
            case 3: search(); break;
            case 4: issueBook(); break;
            case 5: returnBook(); break;
            case 6: puts("Exiting."); break;
            default: puts("Invalid choice.");
        }
    } while (ch != 6);
    free(books);
    return 0;
}
```

**Sample output**

```
1.Add Book Records 2.Display Available Books 3.Search by ID
4.Issue a Book 5.Return a Book 6.Exit
Choice: 1
Number of books to add: 2
Book 1 ID: 101
Title: C Programming
Author: Dennis Ritchie
Price: 450
Book 2 ID: 102
Title: Data Structures
Author: Seymour
Price: 520
Choice: 2
ID       Title                    Author             Price    Status
101      C Programming            Dennis Ritchie       450.00 Available
102      Data Structures          Seymour              520.00 Available
Choice: 4
Enter Book ID to issue: 101
Book issued successfully.
Choice: 6
Exiting.
```

---

## 2. Stack Operations using Array

Menu-driven stack of integers (array implementation, size `MAX`): push, pop, palindrome check, overflow/underflow demonstration, display.

```c
#include <stdio.h>

#define MAX 5

int stack[MAX], top = -1;

void push(int x) {
    if (top == MAX - 1) {
        puts("Stack Overflow!");
        return;
    }
    stack[++top] = x;
    printf("%d pushed.\n", x);
}

void pop(void) {
    if (top == -1) {
        puts("Stack Underflow!");
        return;
    }
    printf("Popped: %d\n", stack[top--]);
}

void display(void) {
    int i;
    if (top == -1) {
        puts("Stack is empty.");
        return;
    }
    printf("Stack (top to bottom): ");
    for (i = top; i >= 0; --i) printf("%d ", stack[i]);
    putchar('\n');
}

void palindrome(void) {
    char s[200];
    int i, len, ok = 1;
    printf("Enter a word/string (no spaces): ");
    scanf("%199s", s);
    for (len = 0; s[len] != '\0'; ++len) {}
    for (i = 0; i < len / 2; ++i)
        if (s[i] != s[len - 1 - i]) {
            ok = 0;
            break;
        }
    puts(ok ? "Palindrome." : "Not a palindrome.");
}

int main(void) {
    int ch, x;
    do {
        puts("\n1.Push 2.Pop 3.Check Palindrome 4.Demonstrate Overflow/Underflow 5.Display 6.Exit");
        printf("Choice: ");
        if (scanf("%d", &ch) != 1) break;
        switch (ch) {
            case 1: printf("Element: "); scanf("%d", &x); push(x); break;
            case 2: pop(); break;
            case 3: palindrome(); break;
            case 4:
                pop();
                while (top < MAX - 1) push(10 + top + 1);
                push(99);
                break;
            case 5: display(); break;
            case 6: puts("Exiting."); break;
            default: puts("Invalid choice.");
        }
    } while (ch != 6);
    return 0;
}
```

**Sample output**

```
1.Push 2.Pop 3.Check Palindrome 4.Demonstrate Overflow/Underflow 5.Display 6.Exit
Choice: 1
Element: 10
10 pushed.
Choice: 1
Element: 20
20 pushed.
Choice: 5
Stack (top to bottom): 20 10
Choice: 2
Popped: 20
Choice: 3
Enter a word/string (no spaces): level
Palindrome.
Choice: 6
Exiting.
```

---

## 3. Printer Queue

Simulates a printer queue (circular array): add job, process job, display pending jobs, count jobs, overflow/underflow.

```c
#include <stdio.h>
#include <string.h>

#define MAX 5
#define LEN 80

char q[MAX][LEN];
int front = 0, rear = -1, count = 0;

void addJob(void) {
    if (count == MAX) {
        puts("Queue Overflow! Printer queue is full.");
        return;
    }
    printf("Enter print job name: ");
    scanf(" %79[^\n]", q[(rear + 1) % MAX]);
    rear = (rear + 1) % MAX;
    count++;
    puts("Print job added.");
}

void processJob(void) {
    if (!count) {
        puts("Queue Underflow! No jobs to process.");
        return;
    }
    printf("Processing job: %s\n", q[front]);
    front = (front + 1) % MAX;
    count--;
    if (!count) {
        front = 0;
        rear = -1;
    }
}

void display(void) {
    int i, idx;
    if (!count) {
        puts("No pending print jobs.");
        return;
    }
    puts("Pending print jobs:");
    for (i = 0; i < count; ++i) {
        idx = (front + i) % MAX;
        printf("%d. %s\n", i + 1, q[idx]);
    }
}

int main(void) {
    int ch;
    do {
        puts("\n1.Add Job 2.Process Job 3.Display Jobs 4.Count Jobs 5.Test Underflow 6.Exit");
        printf("Choice: ");
        if (scanf("%d", &ch) != 1) break;
        switch (ch) {
            case 1: addJob(); break;
            case 2: processJob(); break;
            case 3: display(); break;
            case 4: printf("Jobs waiting: %d\n", count); break;
            case 5:
                if (!count) processJob();
                else puts("Queue is not empty; process jobs to test underflow.");
                break;
            case 6: puts("Exiting."); break;
            default: puts("Invalid choice.");
        }
    } while (ch != 6);
    return 0;
}
```

**Sample output**

```
1.Add Job 2.Process Job 3.Display Jobs 4.Count Jobs 5.Test Underflow 6.Exit
Choice: 1
Enter print job name: Report.pdf
Print job added.
Choice: 1
Enter print job name: Notes.docx
Print job added.
Choice: 3
Pending print jobs:
1. Report.pdf
2. Notes.docx
Choice: 4
Jobs waiting: 2
Choice: 2
Processing job: Report.pdf
Choice: 6
Exiting.
```

---

## 4. Singly Linked List of Student Records

Create using front insertion, display with node count, insert/delete at both ends, search by USN.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define S 30

typedef struct Node {
    char usn[S], name[S], branch[S];
    int sem;
    struct Node *next;
} Node;

Node *head = NULL;

Node *readNode(void) {
    Node *p = (Node *)malloc(sizeof(Node));
    if (!p) {
        puts("Memory allocation failed.");
        exit(1);
    }
    printf("USN: ");
    scanf("%29s", p->usn);
    printf("Name (one word): ");
    scanf("%29s", p->name);
    printf("Branch: ");
    scanf("%29s", p->branch);
    printf("Semester: ");
    scanf("%d", &p->sem);
    p->next = NULL;
    return p;
}

void frontInsert(void) {
    Node *p = readNode();
    p->next = head;
    head = p;
}

void endInsert(void) {
    Node *p = readNode(), *t;
    if (!head) {
        head = p;
        return;
    }
    for (t = head; t->next; t = t->next) {}
    t->next = p;
}

void deleteFront(void) {
    Node *t;
    if (!head) {
        puts("List is empty.");
        return;
    }
    t = head;
    head = head->next;
    free(t);
    puts("First record deleted.");
}

void deleteEnd(void) {
    Node *t, *prev = NULL;
    if (!head) {
        puts("List is empty.");
        return;
    }
    for (t = head; t->next; t = t->next) prev = t;
    if (prev) prev->next = NULL;
    else head = NULL;
    free(t);
    puts("Last record deleted.");
}

void display(void) {
    Node *t = head;
    int count = 0;
    if (!t) puts("List is empty.");
    puts("USN\tName\tBranch\tSem");
    while (t) {
        printf("%s\t%s\t%s\t%d\n", t->usn, t->name, t->branch, t->sem);
        t = t->next;
        count++;
    }
    printf("Number of nodes: %d\n", count);
}

void search(void) {
    char key[S];
    Node *t = head;
    printf("Enter USN to search: ");
    scanf("%29s", key);
    while (t && strcmp(t->usn, key)) t = t->next;
    if (!t) puts("Student not found.");
    else printf("Found: %s %s %s Semester %d\n", t->usn, t->name, t->branch, t->sem);
}

int main(void) {
    int ch;
    do {
        puts("\n1.Front Insert 2.Display/Count 3.End Insert 4.Delete Front 5.Delete End 6.Search USN 7.Exit");
        printf("Choice: ");
        if (scanf("%d", &ch) != 1) break;
        switch (ch) {
            case 1: frontInsert(); break;
            case 2: display(); break;
            case 3: endInsert(); break;
            case 4: deleteFront(); break;
            case 5: deleteEnd(); break;
            case 6: search(); break;
            case 7: puts("Exiting."); break;
            default: puts("Invalid choice.");
        }
    } while (ch != 7);
    while (head) {
        Node *t = head;
        head = head->next;
        free(t);
    }
    return 0;
}
```

**Sample output**

```
1.Front Insert 2.Display/Count 3.End Insert 4.Delete Front 5.Delete End 6.Search USN 7.Exit
Choice: 1
USN: 1AB23CS001
Name (one word): Anu
Branch: CSE
Semester: 3
Choice: 3
USN: 1AB23CS002
Name (one word): Ravi
Branch: CSE
Semester: 3
Choice: 2
USN         Name    Branch  Sem
1AB23CS002  Ravi    CSE     3
1AB23CS001  Anu     CSE     3
Number of nodes: 2
Choice: 6
Enter USN to search: 1AB23CS001
Found: 1AB23CS001 Anu CSE Semester 3
Choice: 7
Exiting.
```

---

## 5. Binary Tree Creation and Traversals

Create a binary tree from level-order input; preorder, inorder and postorder traversals.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *left, *right;
} Node;

Node *newNode(int x) {
    Node *p = (Node *)malloc(sizeof(Node));
    if (!p) {
        puts("Memory allocation failed.");
        exit(1);
    }
    p->data = x;
    p->left = p->right = NULL;
    return p;
}

Node *createTree(void) {
    int n, i, x;
    Node **q, *root;
    printf("Number of nodes: ");
    scanf("%d", &n);
    if (n <= 0) return NULL;
    q = (Node **)malloc((size_t)n * sizeof(Node *));
    if (!q) {
        puts("Memory allocation failed.");
        exit(1);
    }
    printf("Enter %d integer values in level order: ", n);
    scanf("%d", &x);
    root = newNode(x);
    q[0] = root;
    for (i = 1; i < n; ++i) {
        scanf("%d", &x);
        q[i] = newNode(x);
        if (i % 2 == 1) q[(i - 1) / 2]->left = q[i];
        else q[(i - 1) / 2]->right = q[i];
    }
    free(q);
    return root;
}

void preorder(Node *r) {
    if (r) {
        printf("%d ", r->data);
        preorder(r->left);
        preorder(r->right);
    }
}

void inorder(Node *r) {
    if (r) {
        inorder(r->left);
        printf("%d ", r->data);
        inorder(r->right);
    }
}

void postorder(Node *r) {
    if (r) {
        postorder(r->left);
        postorder(r->right);
        printf("%d ", r->data);
    }
}

void freeTree(Node *r) {
    if (r) {
        freeTree(r->left);
        freeTree(r->right);
        free(r);
    }
}

int main(void) {
    Node *root = NULL;
    int ch;
    do {
        puts("\n1.Create Binary Tree 2.Preorder 3.Inorder 4.Postorder 5.Exit");
        printf("Choice: ");
        if (scanf("%d", &ch) != 1) break;
        switch (ch) {
            case 1: freeTree(root); root = createTree(); break;
            case 2:
                if (!root) puts("Create tree first.");
                else { preorder(root); putchar('\n'); }
                break;
            case 3:
                if (!root) puts("Create tree first.");
                else { inorder(root); putchar('\n'); }
                break;
            case 4:
                if (!root) puts("Create tree first.");
                else { postorder(root); putchar('\n'); }
                break;
            case 5: puts("Exiting."); break;
            default: puts("Invalid choice.");
        }
    } while (ch != 5);
    freeTree(root);
    return 0;
}
```

**Sample output**

```
1.Create Binary Tree 2.Preorder 3.Inorder 4.Postorder 5.Exit
Choice: 1
Number of nodes: 5
Enter 5 integer values in level order: 1 2 3 4 5
Choice: 2
1 2 4 5 3
Choice: 3
4 2 5 1 3
Choice: 4
4 5 2 3 1
Choice: 5
Exiting.
```

---

## 6. Graph of Cities using Adjacency Matrix

Create an adjacency matrix for N cities and print all nodes reachable from a starting node using DFS and BFS.

```c
#include <stdio.h>

#define MAX 20

int a[MAX][MAX], n, visited[MAX];
char city[MAX][30];

void dfs(int v) {
    int i;
    visited[v] = 1;
    printf("%s ", city[v]);
    for (i = 0; i < n; ++i)
        if (a[v][i] && !visited[i]) dfs(i);
}

void bfs(int start) {
    int q[MAX], f = 0, r = 0, i, v;
    for (i = 0; i < n; ++i) visited[i] = 0;
    q[r++] = start;
    visited[start] = 1;
    while (f < r) {
        v = q[f++];
        printf("%s ", city[v]);
        for (i = 0; i < n; ++i)
            if (a[v][i] && !visited[i]) {
                visited[i] = 1;
                q[r++] = i;
            }
    }
}

int main(void) {
    int i, j, ch, s;
    printf("Number of cities (max %d): ", MAX);
    scanf("%d", &n);
    if (n < 1 || n > MAX) {
        puts("Invalid number.");
        return 1;
    }
    for (i = 0; i < n; ++i) {
        printf("City %d name: ", i);
        scanf("%29s", city[i]);
    }
    puts("Enter adjacency matrix (0/1), row by row:");
    for (i = 0; i < n; ++i)
        for (j = 0; j < n; ++j)
            scanf("%d", &a[i][j]);
    puts("Adjacency matrix:");
    for (i = 0; i < n; ++i) {
        for (j = 0; j < n; ++j) printf("%d ", a[i][j]);
        putchar('\n');
    }
    printf("Starting node index (0 to %d): ", n - 1);
    scanf("%d", &s);
    if (s < 0 || s >= n) {
        puts("Invalid starting node.");
        return 1;
    }
    do {
        puts("\n1.DFS 2.BFS 3.Exit");
        printf("Choice: ");
        scanf("%d", &ch);
        for (i = 0; i < n; ++i) visited[i] = 0;
        if (ch == 1) {
            printf("DFS reachable nodes: ");
            dfs(s);
            putchar('\n');
        } else if (ch == 2) {
            printf("BFS reachable nodes: ");
            bfs(s);
            putchar('\n');
        } else if (ch == 3) puts("Exiting.");
        else puts("Invalid choice.");
    } while (ch != 3);
    return 0;
}
```

**Sample output**

```
Number of cities (max 20): 4
City 0 name: A
City 1 name: B
City 2 name: C
City 3 name: D
Enter adjacency matrix (0/1), row by row:
0 1 1 0
0 0 0 1
0 0 0 1
0 0 0 0
Starting node index (0 to 3): 0
1.DFS 2.BFS 3.Exit
Choice: 1
DFS reachable nodes: A B D C
Choice: 2
BFS reachable nodes: A B C D
Choice: 3
Exiting.
```
