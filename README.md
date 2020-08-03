# c0
c0编译器 “编译原理”是大学计算机类专业的重要专业基础课。内容主要包括编译系统的结构、工程流程以及编译程序各组成部分的设计原理和实现技术。涉及程序的词法分析、语法分析、语义分析及语法制导翻译、运行时环境、目标代码生成、代码优化等理论和技术。通过本课程的学习和实践使学生既掌握编译理论和方法方面的基本知识，也获得设计、实现、分析和移植编译程序方面的初步能力。
# 右上角star收藏
 编译原理实验 C0编译器的
设计与实现

 一、实验目的
 “编译原理”是大学计算机类专业的重要专业基础课。内容主要包括编译系统的结构、工程流程以及编译程序各组成部分的设计原理和实现技术。涉及程序的词法分析、语法分析、语义分析及语法制导翻译、运行时环境、目标代码生成、代码优化等理论和技术。通过本课程的学习和实践使学生既掌握编译理论和方法方面的基本知识，也获得设计、实现、分析和移植编译程序方面的初步能力。

二、实验流程图及编译程序结构

1.C0的解释执行结构

C0语言解释执行程序
C0语言目标程序
 

输入数据
输出数据


2.C0编译程序结构
 
 
3.执行过程

    

三、词法分析器的设计与实现：

（1）功能描述：
对给定的程序通过词法分析器进行单词符号的识别，并以二元式(单词种别码，单词符号的属性值)显示，进行下一步的语义分析程序的执行的使用。具体本程序则是通过对给定路径的文件的分析后以单词符号和文字提示显示。

（2）常量的定义：
  
public static final symbol[] keywords = new symbol[]{ifsym,whilesym,intsym,voidsym,elsesym,scanfsym,printfsym,returnsym};//保留字

 public static final symbol[] single = new symbol[]{ becomes,plus, minus, times, slash, lparen, rparen, comma, semicolon, lbrace, rbrace};//单字符

词法分析类型的定义保留字symbol 
nul("", 0),ident("ident", 1),number("number", 2),plus("+", 3),
minus("-", 4), times("*", 5),slash("/", 6),lparen("(", 7),
rparen(")", 8),comma(",", 9), semicolon(";", 10),
 ifsym("if", 11), whilesym("while", 12),lbrace("{", 13),rbrace("}", 14),
intsym("int", 15), voidsym("void", 16),elsesym("else", 17),
scanfsym("scanf", 18),printfsym("printf", 19), returnsym("return", 20), becomes("=", 21);

（3）以层次图形式描述模块的组成及调用关系












（4）主要函数的设计要求（功能、参数、返回值）： 
 
 Lex(String path) //初始化并读取全部代码 ,输入路径，返回全部代码的code字符串“string类型”
getsym() // 识别一个单词，返回取出的单词
LexReturn(Constant.symbol wordLexType, T value) //生成单词类型wordLexType;单词值value;

（5）词法分析器的实现
首先输入路径，初始化并读取全部代码 到一个字符串中去
1.如果没有读取进入代码就返回空
2.如果是空格换行和tab，则取下一个字符
3.如果是字母就获取完整单词，遍历关键字表,判断是哪个关键字
  若搜索失败则非指定关键词，为普通标识符，继续进行下一步的判断      
4.如果是数字，获取完整数字，返回常量
5.如果是单字符 
若为左括号，进行当前位置后移+1；
右括号则当前位置左移减一。
最后返回操作符到 LexReturn函数，生成单词类型wordLexType;单词值value;
6.以上都不满足的话返回空


（6）状态转换图： 
 

 



四、语法语义分析器的设计与实现：

（1）功能描述：
在词法分析的基础上将单词序列组合成各类语法短语，如“程序”，“语句”，“表达式”等等.语法分析程序判断源程序在结构上是否正确. 对结构上正确的源程序进行上下文有关性质的审查，进行类型审查。语义分析是审查源程序有无语义错误，为代码生成阶段收集类型信息

（2）常量的定义：

/**
 * 符号常量
 */
public static enum symbol {
    nul(""),
    ident("ident"),
    number("number"),
    plus("+"),
    minus("-"),
    times("*"),
    slash("/"),
    lparen("("),
    rparen(")"),
    comma(","),
    semicolon(";"),
    ifsym("if"),
    whilesym("while"),
    lbrace("{"),
    rbrace("}"),
    intsym("int"),
    voidsym("void"),
    elsesym("else"),
    scanfsym("scanf"),
    printfsym("printf"),
    returnsym("return"),
    becomes("=");
    private String text;
    symbol(String text) {
        this.text = text;
    }
    public String getText() {
        return this.text;
    }
    @Override
    public String toString() {
        return this.text;
    }
List<TableStruct> nametable;//名字表
String identVale;//保存标识符



（3）以层次图形式描述模块的组成及调用关系

	





	















（4）主要函数的设计要求（功能、参数、返回值）： 

/**
*构造方法 指定进行分析的文件
 * @param path 要读的文件路径
 */
public Block( String path)
/**
 * 开始进行分析
 */
public void doBlock() 
/**
 * 查找标识符在名字表的位置
 * @param ident 要查找的名字
 * @return 找到则返回在名字表中的位置否则返回-1
 */
int position(String ident) 
/**
 * 赋值语句处理//函数调用
 */
private void assignDeal() 
/**
 * 输入语句处理
 */
private void scanfDeal() 
/**
 * 打印语句处理
 */
private void printDeal()
/**
 * 条件语句处理
 * @param varAssignPos 变量声明的相对位置
 * @return 变量的相对位置
 */
private int ifDeal(int varAssignPos)
/**
 * 循环语句处理
 * @param varAssignPos 变量声明的相对位置
 * @return 变量的相对位置
 */
private int whileDeal(int varAssignPos)
/**
 * 变量声明语句处理
 * @param lev          当前所在层，0代表全局变量，1代表局部变量
 * @param varAssignPos 变量声明的相对位置
 * @param identVale    标识符的值
 * @return 变量声明的位置
 */
private int intDeal(int lev, int varAssignPos, String identVale)
/**
 * 返回语句处理
 */
private void returnDeal()
/**
 * 语句处理
 * @param varAssignPos 变量声明的相对位置
 */
private int statement(int varAssignPos)
/**
 * 表达式处理
 */
private void expression()
/**
 * 项处理
 */
private void term()
/**
 * 因子处理
 */
private void factor() 


/**
 * 在名字表中加入一项
 *
 * @param type         类型（变量声明还是过程声明）
 * @param lev          所在层
 * @param varAssignPos 变量在该层的相对地址
 * @param identVale 标识符
 */
private void enter(Constant.nameTable type, int lev, int varAssignPos, String identVale)


（5）语法语义分析器的实现
由构造方法传入需要进行分析的程序的路径，再传给lex程序，
执行doBlock方法进行分析，
1如果检测int 标识符 逗号或分号就进行全局变量声明
2如果检测到int或void 标识符和（）{就进行过程声明并进行过程内部处理
2.1	如果标识符为main就修改第一个跳转地址为当前地址
2.2	如果没有检测到右大括号就一直进行语句处理
  2.2.1如果是标识符就准备进行赋值语句处理
		 1）先取得标识符在名字表中的位置
		 2）如果不存在就提示标识符不存在的错误
  	 3）如果是过程就生成call指令
		 4）如果是变量经常到等号后先进行表达式处理然后生成sto指令
  2.2.2如果检测到scanf就进行输入语句处理
		 1) 从名字表中找到标识符的位置
		 2）生成read指令
		 3）生成sto指令
  2.2.3如果检测到printf就进行打印语句处理
		 1）进行表达式处理
 	 2) 生成wrt指令
  2.2.4如果检测到if就进行条件语句处理
		 1) 生成条件跳转指令
		 2）进行语句处理
  2.2.5如果检测到while就进行循环语句处理
		 1）进行表达式处理
		 2）进行语句处理
  2.2.6如果检测到int就进行局部变量的声明处理
		 1）如果检测到逗号就一直进行变量声明
		 2）检测到分号就退出
2.3	如果检测到右大括号并且计数为零就退出语句处理

（6）流程图
 

五、解释器的设计与实现：

解释器：
（1）功能描述：
解释器程序用来将生成的代码编译执行。

（2）常量的定义：
ArrayList<Instruction> code;//程序地址寄存器
int[] runStack;//运行栈
int top_Addr = 0;//栈顶指针
int base_Addr = 0;//基址寄存器
int current_Addr = 0;//指令寄存器当前正在解释执行的目标指令
目标代码存在currentCode中
private int getLocation(Instruction currentCode)获取变量地址：
如果层数是0就返回全局变量地址，否则就返回当前函数的变量地址。
public void paser()  解释执行


（3）栈式指令系统表

指令	操作	运行栈
LIT   0  a	将常数值取到栈顶，a为常数值 	顶指针指向a
LOD  t  a	将变量值取到栈顶，a为相对地址，t为层数	顶指针指向a
STO  t  a	将栈顶内容送入某变量单元中，a为相对地址，t为层数	将栈顶的值存到a


CAL  0  a	

调用函数，a为函数地址	当前基址入栈(用作动态链，当前指令地址入栈，RA用作返回地址，新的基址为当前顶指针指向的地址跳转到a.
INT  0  a	在运行栈中为被调用的过程开辟a个单元的数据区	顶指针加a
JMP  0  a	无条件跳转至a地址	跳转到a
JPC  0  a	条件跳转，当栈顶值为0，则跳转至a地址，否则顺序执行	栈顶值为0时跳转到a
ADD  0  0	次栈顶与栈顶相加，退两个栈元素，结果值进栈	退两个栈将所得数放到栈顶
SUB  0  0	次栈顶减去栈顶，退两个栈元素，结果值进栈	退两个栈将所得数放到栈顶
MUL  0  0	次栈顶乘以栈顶，退两个栈元素，结果值进栈	退两个栈将所得数放到栈顶
DIV  0  0	次栈顶除以栈顶，退两个栈元素，结果值进栈	退两个栈将所得数放到栈顶
RED  0  0	从命令行读入一个输入置于栈顶	将输入的值放到栈顶
WRT  0  0	栈顶值输出至屏幕并换行	
RET  0  0	函数调用结束后,返回调用点并退栈	
 		
























通过上述的栈式指令，对比生成代码中的指令依次执行程序，最后输出结果。

附-：出错处理：

（1）功能描述：
出错处理程序用来记录和处理程序编译过程中遇到的词法和语法错误，并在编译结束的时候将错误输出。

（2）常量的定义：
public static void showError(int error) 判断出错的类型

（3）出错报错的内容：	 
1缺少左括号
2缺少右括号
3缺少分号
4不是变量




5不能作为函数的名字
6该标识符未声明
7不能作为过程名
8当前数字超过了规定上限
9语句格式错误,开头应为变量或函数名
10缺少赋值符号
11不是标识符
12不是过程
13缺少左大括号
14缺少右大括号
15超出数字允许的最大长
16没有主函数
17已经存在主函数
18不能是void过程
19已存在的标识符
20 void不能有返回值
21不是语句

六、关键部分描述及问题解决
1.运行时存储组织和管理
  
临时工作单元
局部变量
Return的保留单元
RA（返回地址）
DL(动态链)


2.语法语义分析

在词法分析的基础上将单词序列组合成各类语法短语，如“程序”，“语句”，“表达式”等等.语法分析程序判断源程序在结构上是否正确. 对结构上正确的源程序进行上下文有关性质的审查，进行类型审查。语义分析是审查源程序有无语义错误，为代码生成阶段收集类型信息

七、实验测试运行案例 
（两个一组，前面是测试c0程序，后面是生成名字表，虚拟机代码和执行结果）

测试案例1:

int c()
{
    return 1;
}
void main()
{
   printf(c());
}
 


测试案例2:



 
 



测试案例3:

 
 



测试案例4:

int a()
{
   return 1;
}
int b()
{
   return a();
}
void main(){
    printf( b());
}

 


更多测试案例已经附赠打包在程序源码中 *.txt 

八、各自的贡献

我的CSDN博客https://blog.csdn.net/qq_27500493
欢迎反馈联系么么哒
改改再交，别都交的一样。大兄弟！

九、实验心得

通过这次实验，我们对词法分析、语法分析、语义分析、目标代码生成解释等理论和技术有较好的掌握。通过三人团队合作，进行分模块的进行合作，我们体会了团队合作的乐趣与力量。
在将一个大的编译程序分成主要的三大模块（词法分析，语法语义分析,解释程序）后，进行协作完成。在规定统一的接口以后进行编写程序，我们可以很好将各个模块结合起来。通过分工合作，我们真正的把软件工程课上学的知识运用到了这个大程序上面。但是在开发设计之前，没有进行充分的考虑程序编写中可能出现的问题，而是在不断的编写中进行查阅相关资料，最终才合力完成该程序的编写。
我们下次在进行这样的大程序的编写时候，应该提前做好设计，将软件工程学到的各种软件开发方法运用进去，真正做到学以致用。在完成这个程序后，我们了解到编译原理的魅力所在，激发了我们要解决更多更难问题的决心。
 
C0编译器编译原理实验 C0编译器的 设计与实现原创-当年我们交的作业被评为优秀作业！！！要不是毕业好几年我都不舍得分享出来！！！还有配套代码在我的博客
CTRL+D收藏一下或者关注走一波-有你所需！不断更新！
其他相关下载，配套代码以及PPT。稳妥的小老弟
https://me.csdn.net/download/qq_27500493

