## C语言代码风格精简版


### 变量

- 整体遵循全小写下划线命名法；
- 语法结构采用「名词/形容词 + 名词」的结构，如`user_id`；
- 下面是对不同变量类型的命名示例：
    ```c
    int user_id;               //: 变量
    static int user_id;        //: 静态变量
    const int user_id;         //: 常数变量
    extern int user_id;        //: 全局变量
    int *user_id, **user_id;   //: 指针变量
    extern int *user_id;       //: 全局指针
    struct UserInfo user_info; //: 结构体变量
    ```

### 结构体

- 整体遵循帕斯卡命名法；
- 语法结构采用「名词/形容词 + 名词」的结构，如`UserInfo`；
- 仅源文件内部使用则纯`struct`形式即可，若对外暴露则头文件前置声明`typedef`类型；
- 使用`typedef`定义的新类型后缀加`_t`；
- 枚举类型成员前缀加`结构体名_`，如`UserInfo_BaseID`；
- 如果遇到业内惯例的缩写词，其大写更能反映单词整体性，则使用全大写的形式，如使用`ID`而非`Id`；
- 下面是对不同结构体类型的命名示例：
    ```c
    typedef struct UserInfo UserInfo_t;         // 结构体前置声明
    typedef struct UserInfo {      //: 结构体
    } UserInfo_t, *UserInfo_t, **UserInfo_t;    // 尽量避免在结构体类型声明时声明指针类型
    typedef union UserInfo {       //: 联合体
    } UserInfo_t;   
    typedef enum UserInfo {        //: 枚举体
        UserInfo_BaseID,
    } UserInfo_t;  
    ```

### 函数

- 整体遵循全小写下划线命名法；
- 语法结构采用「标识 + 动词 + 名词 + （子名词）」的结构，如`id_do_thing_sub()`；
- 特殊函数命名作为例外：句柄注册接口函数（元方法）使用`_id_do_thing_sub()`且只能为`static`；寄存器操作函数使用`_id_do_register()`；汇编转C语言的操作函数使用`__asm_do_thing()`；
- 下面是对不同函数类型的命名示例：
    ```c
    void id_do_thing_sub(buf, len)  //: 函数（形参采用截断式缩写）
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
- 普通功能宏不使用下划线前后缀，如`USER_ID`、`FUN_MACRO_1`；
- 头文件保护宏约定为「文件名_H」形式，如`USER_INFO_H`（业界主流；内核/POSIX传统用`_文件名_H`，但该前缀属C标准保留区，不推荐用户代码使用）；
- 构建预处理、汇编转C等实现类别宏使用语义化前缀区分，如`PRE_`、`ASM_`，不使用`_`/`__`前缀；
- 编译器与C标准保留宏（如`__STDC__`、`__LINE__`）由编译器/标准库定义，禁止用户自定义；
- 下面是对不同宏类型的命名示例：
    ```c
    #define USER_ID(X)              //: 函数宏（带形参的宏）
    #define USER_INFO         \     //: 结构体定义宏
        UserInfo_t name = {   \
            ...;              \
        }
    #define FUN_MACRO_1 function()  //: 函数宏（展开为函数调用）
    #define FUN_MACRO_2 do {  \     //: 复杂函数宏
        ...;                  \
    } while(0)
    #define PRE_MACRO               //: 构建预处理宏（语义前缀PRE_）
    #define ASM_MACRO               //: 汇编转成C语言的操作宏（语义前缀ASM_）
    #define USER_INFO_H             //: 头文件保护宏（约定：文件名_H）
    ```

### 缩进

- 整体遵循4空格为单位缩进，严禁使用制表符(tab)；
- 大括号位置按语句特性区分：`if`/`else`/`for`/`while()`/`do` 采用同行`{`，`switch` 与 `while(1)` 采用换行`{`；
- 函数体大括号按函数特性区分：普通函数换行`{`，`inline` 内联函数与句柄注册接口函数（元方法）同行`{`（见函数示例）；
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
        default: break;
    }

    for() {              //: for格式
    }                

    while() {            //: while格式
    }
    while(1)             //: while(1)格式（仅 while(1) 需要换行，其余同行）
    {            
    }

    do {                 //: do-while格式
    } while()  
    ```

### 注释

- `/** ... */`：主推的 Doxygen 块式注释，用于文件头文档注释和函数头注释（.h 与 .c 统一使用），示例：
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
- `/// ...`：Doxygen 单行注释，可作为函数头注释的便捷形式（可选），后面可以跟@标签，示例：
    ```c
    /// @brief Brief description.
    /// @note Note.
    function() {}
    ```
- `/* ... */`：通用注释；
- `// ...`：用于函数过程单行注释和变量或结构体成员注释，后面不可以跟@标签；
- `///< ...`：用于代码行尾的简短注释；

### 缩写

> 缩写强度分为三级，从低到高分别为：  
> 不缩写 - 保留单词全称`receive`  
> 截断式缩写 - 约定俗成的命名`recv`  
> 首字母缩写 - 取单词的第一个字母`r`  

- **不缩写的场景**
	- 全局变量
	- 函数名
	- 结构体成员名
	- 宏名
- **截断式缩写的场景**
	- 函数形参名
	- 业内的命名惯例
    - 结构体名
- **首字母缩写的场景**
	- 局部变量
	- 宏参数名

### 顺序

> 文件内容优先按依赖关系排列；以下顺序为默认规则。
> 因类型或函数调用依赖需要时，允许调整相邻项的顺序。

#### .h 文件

1. 文件头注释；
2. 头文件保护宏；
3. #include（项目内 → 标准库 → 第三方）；
4. 宏定义（常量、配置开关）；
5. 类型相关声明（前置声明 → typedef / enum → struct / union）；
6. 对外全局变量声明（extern）；
7. 对外函数声明；
8. 关闭头文件保护宏。

#### .c 文件

1. 文件头注释；
2. #include（本模块头文件 → 项目内 → 标准库 → 第三方）；
3. 宏定义（仅当前源文件使用的常量、配置开关）；
4. 类型定义与结构体定义（仅当前源文件使用时）；
5. 对外全局变量定义；
6. 静态全局变量定义；
7. 静态函数前置声明；
8. 对外函数实现；
9. 静态函数实现。

#### 函数

> 对外函数默认按对象或设备的操作流程排列；因调用依赖需要时，允许调整相邻项的顺序。
> 不适用的阶段可省略，状态机与硬件时序要求优先于本规则。

1. 创建/初始化/打开；
2. 配置/启动；
3. 添加/插入/写入；
4. 获取/读取/查找；
5. 取出/移除/删除；
6. 停止/关闭/销毁；
7. 文件私有的静态辅助函数。

### 其他

- 下面是常见语法和逻辑示例：
    ```c
    /* C模板文件名以.inc结尾 */
    template.inc

    /* 若NDEBUG被定义，则断言不会触发 */
    assert(X)   ((void)0)   //将其替换

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
