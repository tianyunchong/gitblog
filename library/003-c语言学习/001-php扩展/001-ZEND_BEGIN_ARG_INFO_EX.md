# 宏定义ZEND_BEGIN_ARG_INFO_EX, ZEND_ARG_INFO, ZEND_END_ARG_INFO
1. 代码实际案例
```c
ZEND_BEGIN_ARG_INFO_EX(arginfo_bcadd, 0, 0, 2)
    ZEND_ARG_INFO(0, left_operand)
    ZEND_ARG_INFO(0, right_operand)
    ZEND_ARG_INFO(0, scale)
ZEND_END_ARG_INFO()
```

2. 代码宏定义
```c
#define ZEND_BEGIN_ARG_INFO_EX(name, _unused, return_reference, required_num_args)    \
    static const zend_arg_info name[] = {                                                                        \
        { NULL, 0, NULL, required_num_args, 0, return_reference, 0, 0 },
#define ZEND_ARG_INFO(pass_by_ref, name)                            { #name, sizeof(#name)-1, NULL, 0, 0, pass_by_ref, 0, 0 },
#define ZEND_END_ARG_INFO()        };
```

3. zend_arg_info结构体定义
```c
typedef struct _zend_arg_info {
    const char *name;                /* 参数的名称*/
    zend_uint name_len;                /* 参数名称的长度*/
    const char *class_name;            /* 类名 */
    zend_uint class_name_len;        /* 类名长度*/
    zend_bool array_type_hint;        /* 数组类型提示 */
    zend_bool allow_null;            /* 是否允许为NULL　*/
    zend_bool pass_by_reference;    /* 是否引用传递 */
    zend_bool return_reference;        /* 返回值是否为引用形式 */
    int required_num_args;          /* 必要参数的数量 */
} zend_arg_info;
```

4. 测试使用
```c
//测试使用下
//实际作用，宏定义，定义一个zend_arg_info类型的数组，下面是输出第一个zend_arg_info常量的类名长度
#include <stdio.h>
#include "Zend/zend_API.h"
int main() {
    ZEND_BEGIN_ARG_INFO_EX(arginfo_client___construct, 0, 0, 5)
        ZEND_ARG_INFO(0, url)
        ZEND_ARG_INFO(0, async)
    ZEND_END_ARG_INFO()
    printf("%d\n", arginfo_client___construct[0].class_name_len);
    printf("%s\n", arginfo_client___construct[1].name);
    printf("%s\n", arginfo_client___construct[2].name);
    return 1;
}
```
