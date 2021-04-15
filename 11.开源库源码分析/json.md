```cpp
typedef enum{LEPT_NULL,LEPT_TRUE,LEPT_FALSE,LEPT_NUMBER,LEPT_STRING,LEPT_ARRAY,LEPT_OBJECT} lept_type;

typedef struct{
    lept_type type;
}lept_value;

int lept_parse(lept_value *v,const char* json);//解析json
//注意：传入的根节点v是由使用方负责分配的
一般用法是如下
lept_value v;
cont char json[]="...";
int res=lept_json(&v,json);
//返回值是以下这些枚举值，无错误返回LEPT_PARSE_OK
enum{
  LEPT_PARSE_OK=0,
  LEPT_PARSE_EXCEPT_VALUE,
  LEPT_PARSE_INVALID_VALUE,
  LEPT_PARSE_ROOT_NOT_SINGULAR
};

//一个访问结果的函数
lept_type lept_get_type(cont lept_value* V);

//编写宏函数时用do{}while(0);
```

