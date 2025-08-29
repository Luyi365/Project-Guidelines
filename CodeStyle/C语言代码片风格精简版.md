## C语言代码片风格精简版


### 变量

- 整体遵循全小写下划线命名法；
- 语法结构采用「名词/形容词 + 名词」的结构，如`user_id`；
- 下面是对不同变量类型的命名示例：
    ```c
    int user_id;   
    static int user_id;        //: 静态
    const int user_id;         //: 常量
    extern int user_id;        //: 全局
    int *user_id, **user_id;   //: 指针
    extern int *user_id;       //: 全局指针
    struct UserInfo user_info; //: 结构体变量
    ```

### 结构体

- 整体遵循帕斯卡命名法；
- 语法结构采用「名词/形容词 + 名词」的结构，如`UserInfo`；
- 使用`typedef`定义的新类型后缀加`_t`；
- 枚举类型成员前缀加`结构体名_`，如`UserInfo_BaseID`；
- 如果遇到知名缩写词，其大写更能反映单词整体性，则使用全大写的形式，如使用`ID`而非`Id`；
- 下面是对不同结构体类型的命名示例：
    ```c
    typedef struct UserInfo {
    } UserInfo_t, *UserInfo_t, **UserInfo_t;    // 尽量避免在结构体类型声明时声明指针类型
    typedef union UserInfo {       //: 联合体
    } UserInfo_t;   
    typedef enum UserInfo {        //: 枚举
        UserInfo_BaseID,
    } UserInfo_t;  
    ```

### 函数

- 整体遵循全小写下划线命名法；
- 语法结构采用「标识 + 动词 + 名词 + （子名词）」的结构，如`id_do_thing_sub()`；
- 下面是对不同函数类型的命名示例：
    ```c
    void id_do_thing_sub()         
    {
    }
    static void id_do_thing_sub()   //: 静态函数
    {
    }
    inline void id_do_thing_sub() { //: 内联函数
    }
    static int _id_do_thing_sub() { //: 句柄注册的接口函数（元方法），只能是static类型
    }
    void _id_do_register()          //: 操作寄存器函数
    {
    }
    void __asm_do_thing()           //: 汇编转成 C 语言的操作函数
    {
    }
    ```

### 宏

- 整体遵循全大写下划线命名法；
- 语法结构与变量、函数保持一致，只是把小写改成了大写，如`USER_ID`；
- 下面是对不同宏类型的命名示例：
    ```c
    #define USER_ID                 //: 变量宏
    #define USER_INFO         \     //: 结构体定义宏
        UserInfo_t name = {   \
            ...;              \
        }
    #define FUN_MACRO_1 function()  //: 函数宏
    #define FUN_MACRO_2 do {  \     //: 复杂函数宏
        ...;                  \
    } while(1)
    #define _STD_MACRO              //: 构建预处理宏或C标准保留
    #define __ASM_MACRO             //: 汇编转成C语言的操作宏
    #define __HEAD_MACRO__          //: 头文件保护宏或C标准保留
    #define _PRE_MACRO_             //: 构建预处理宏
    ```

### 缩进

- 整体遵循4空格为单位缩进，严禁使用制表符(tab)；
- 下面是对标准逻辑类型的缩进示例：
    ```c
    if() {              //: if格式
    }
    else if() {
    }
    else {
    }

    switch()            //: switch格式
    {
        case xxx: ; break;    // 如果所有case内容都仅占一行则用此格式
        case xxx:             // 否则用此格式
            ...;
            break;
        case xxxx: {          // 如果case中带有括号则用此格式
            ...;   
        } break;
        default : break;
    }

    while()              //: while格式
    {
    }

    do {                 //: do-while格式
    } while()  
    ```

### 注释

- `/** ... */`：用于文件头文档注释和<.h>文件中函数头注释，示例：
    ```c
    /**
    * @file filename.c
    * @author your name (you@domain.com)
    * @brief Brief description.
    * @version 0.1
    * @date YYYY-MM-DD
    *
    * @note Note.
    */

    /**
    * @brief Brief description.
    *
    * @param param Description.
    * @return Return description.
    */
   extern function();
    ```
- `/// ...`：单行注释，用于<.c>文件中函数头注释，后面可以跟@标签，示例：
    ```c
    /// @brief Brief description.
    /// @note Note.
    function() {}
    ```
- `/* ... */`：通用注释；
- `// ...`：用于函数过程单行注释和变量或结构体成员注释，后面不可以跟@标签；
- `///< ...`：用于代码行尾的简短注释；

### 缩写

- **推荐缩写场景**
	- 函数形参名
	- 宏参数名
	- 知名缩写词
- **不推荐缩写场景**
	- 全局变量
	- 函数名
	- 结构体成员名
- **待定**
	- 局部变量
	- 结构体名
	- 宏名
	
### 其他

- 下面是常见语法和逻辑示例：
    ```c
    /* C模板文件名以.inc结尾 */
    template.inc

    /* 长语句逻辑判断符位置置前 */
    if(c == 'a'
        || c == 'b'
        || c == 'c') {
    }

    /* 逻辑返回值 */
    1(true)     // 真
    0(false)    // 假

    /* 错误处理返回值 */
    0           // 无错误（成功）
    !0          // 有错误（错误类型）
    ```
