# Struct

```cpp
struct [TAG] /*结构体的标签*/{
    member definition;
}[one or more structure variables];/*一个或多个结构体变量的定义*/

typedef struct _Books{
    char title[50];
    char author[50];
    char subject[100];
    int book_id;
}Books;
typedef struct _Books books;
//定义 "struct _Books" 为类型  "books"
```

