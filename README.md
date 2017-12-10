# Objective-C-Style-Guide
1.缩紧与格式
    ·[强制] 只用空格，用4个空格表示一个缩进。
    ·[强制] 用逗号分隔多项时，每个逗号后使用1个空格进行分隔
    ·[建议] 左大括号 { 不单独占据一行，放置在上一行的末尾，可以在 { 前增加一个空格
2.声明与定义
    ·[强制] -，+ 与返回类型之间必须有一个空格，在参数列表中，除了参数之间不要有任何间距。
    ·[强制] * 号前面必须要加空格，增加可读性。
    ·[建议] 如果有参数太多无法放在同一行内， 最好每个参数各自一行。如果采用多行，每个参数建议按照 : 进行对齐。
    ·[强制] 访问修饰符 @public， @private，@protected必须缩进2个空格。
3.协议
    ·[强制] 在类型标识符和封装在尖括号中的 Protocols 名称之间要有空格。
4.Blocks
    ·[强制] 块内的代码应缩进4个空格。
    ·[建议] 因为具体block长度的不同，可以有以下几种风格：
        如果block一行就能放下，就不需要换行
        如果必须要换行，那么 } 需要和block所在行的第一个字符对齐
        block中的代码块应该缩进4个空格。
        如果block很大，比如超过20行，建议单独拿出来赋给一个本地变量
        如果block没有参数，那字符 ^{ 之间就不应有空格。如果有参数，字符 ^{ 之间同样没有空格，但是字符 ) { 之间需要有一个空格。
        包含内联block的函数调用可以在缩进4个空格的基础上左对齐，尤其是调用中包含多个内联block。
    实例
    //整个block放在一行的
    [operation setCompletionBlock:^{ [self onOperationDone]; }];

    //多行时缩进四个空格，{要和block所在行的第一个字符对齐
    [operation setCompletionBlock:^{
        [self.delegate newDataAvailable];
    }];

    //在C函数中使用block时遵循和Objective-C同样的对齐和缩进原则
    dispatch_async(fileIOQueue_, ^{
        NSString *path = [self sessionFilePath];
        if (path) {
          // ...
        }
    });

    // 方法参数与block声明能放到一行时。注意比较^(SessionWindow *window) {和上面的^{。
    [[SessionService sharedService]
        loadWindowWithCompletionBlock:^(SessionWindow *window) {
            if (window) {
              [self windowDidLoad:window];
            } else {
              [self errorLoadingWindow];
            }
        }];

    //方法参数与block声明不能放到一行时。
    [[SessionService sharedService]
        loadWindowWithCompletionBlock:
            ^(SessionWindow *window) {
                if (window) {
                  [self windowDidLoad:window];
                } else {
                  [self errorLoadingWindow];
                }
            }];

    // 较长的Block可声明为变量。
    void (^largeBlock)(void) = ^{
        // ...
    };
    [operationQueue_ addOperationWithBlock:largeBlock];

    // 一次调用中包含多个内联block。
    [myObject doSomethingWith:arg1
        firstBlock:^(Foo *a) {
            // ...
        }
        secondBlock:^(Bar *b) {
            // ...
        }];
5.Container Literals
    ·[强制] 使用容器（数组和字典）常量，如果其内容被分为多行，应该缩进4个空格。
    ·[建议] 如果内容单行就能放下，在左大括号之后和右大括号之前各添加一个空格。
示例
    NSArray *array = @[ [foo description], @"Another String", [bar description] ];
    NSDictionary *dict = @{ NSForegroundColorAttributeName : [NSColor redColor] };
    ·[建议] 如果内容跨越多行，将左括号和声明放在同一行，之后换行的内容缩进4个空格，并将右括号单独放在新的一行里和声明行对齐。
    ·[强制] 对于字典常量，在 : 之前不加空格，之后至少一个空格（或者空格的数量能够满足对齐的要求）
示例
      NSDictionary *option1 = @{
          NSFontAttributeName: [NSFont fontWithName:@"Helvetica-Bold" size:12],
          NSForegroundColorAttributeName: fontColor
      };

      NSDictionary *option2 = @{
          NSFontAttributeName:            [NSFont fontWithName:@"Arial" size:12],
          NSForegroundColorAttributeName: fontColor
      };
6.命名规范
  6.1 文件名
    ·[强制] 实现Category的文件名需包含类名，如 NSString+Utils.h 或 NSTextView+Autocomplete.h。
  6.2 Objective-C++
    为了最小化在混合开发Cocoa/Objective-C和C++时由命名风格造成的冲突，遵循正在实现方法的风格。如果正在实现的方法是在@implementation 块中, 使用           Objective-C的命名规范。如果正在实现的方法是在C++的class中，则采用C++的命名规范。
  6.3 类名
    ·[强制] 类名（包括Category和Protocol名）以大写字母开始，通过大小写而不是下划线分隔。
    ·[建议] 在设计可跨越多个应用之间共享的代码时，推荐使用前缀，（例如，GTMSendMessage）。还有很多外部依赖库的大型应用中设计的类最好也使用前缀。
    ·[强制] 若使用前缀，则前缀缩写要大于2个字符
  6.4 Category名
    ·[强制] 类别（Category）名中需要加入被扩展的类名。
    解释
      比如我们要给NSString类加一个解析的功能，我们创建一个category，命名为GTMStringParsingAdditions，并且放在名为NSString+Parsing.h的文件中。
    ·[强制] 类名与左括号中间需要有一个空格。
    示例
      //扩展一个framework类：
      @interface NSString (GTMStringParsingAdditions)
      - (NSString *)foobarString:
      @end

      //使方法和属性私有化
      @interface FoobarViewController ()
      @property(nonatomic, retain) NSView *dongleView;
      - (void)performLayout;
      @end
  6.5 Objective-C 方法名
      ·[强制] 方法名应该以小写字母开头，混合大小写。每个命名参数也应该以小写字母开头。
      ·[建议] 方法名称应该尽可能读起来像句子，意味着你应该选择能够搭配方法名的参数名。（比如：convertPoint:fromRect:replaceCharactersInRange:withString:）。
      ·[建议] getter类型的方法不加get前缀。
      ·[强制] 在所有参数前面添加关键字
      示例
      - (void)sendAction:(SEL)aSelector toObject:(id)anObject forAllCells:(BOOL)flag;  // 推荐的
      - (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;   // 错误的
      ·[建议] 确保参数前面的关键字可以正确描述参数
      ·[强制] 当需要基于已有的一个方法创建新方法时，请将新的关键字添加到原有方法后面
      ·[建议] 尽量不要使用“and” 描述参数
  6.6 变量名
    ·[强制] 变量名以小写字母开头，混合大小写以区分单词。
    6.6.1 普通变量名
    6.6.2 成员类变量
      ·[建议] 类成员变量的命名是在普通变量的名字前，添加一个下划线做前缀，比如_usernameTextField。
    6.6.3 常量
      ·[建议] 常量名（宏，本地常变量等）首字母应该以小写的k开头，然后使用混合大小写区分单词，枚举值前面需要以枚举名为前缀。例如：
      示例
        const int kNumberOfFiles = 12;
        NSString *const kUserKey = @"kUserKey";
        enum DisPlayTinge {
           DisplayTingeGreen = 1
           DisplayTingeBlue = 2
        };
7. 注释
  7.1 文件注释
    ·[建议] 创建文件时会生成默认的注释。如果看了文件名还不懂该文件是干什么，可以有选择的在一个文件开头写一段关于内容的描述。
  7.2 声明部分的注视
    ·[建议] 每个接口，类别，协议的声明都应该有个伴随的注释，来描述它的作用以及它如何融入整体环境。注释遵循apple doc风格，以/**开始，*/结束。
    ·[强制] 声明部分的注释如果以“//”开头，且与代码同一行，在 “//” 前加空格。
    ·[建议] 公共接口的每个方法，都应该有注释来解释它的作用、参数、返回值以及其它影响。
  7.3 实现部分的注释
    ·[强制] 实现部分的注释如果以“//”开头，且与代码同一行，在 “//“ 前加空格。
8. Cocoa 和 Objective-C 特性
  8.1 重载指定构造函数
    ·[强制] 当写子类的时候，如果需要init...方法，必须重载父类的指定构造函数。
  8.2 重载  NSObject 的方法
    ·[建议] 如果重载了NSObject类的方法，建议把它们放在@implementation内的前边，这也是惯例。 通常适用（但不局限）于 init...，copyWithZone:，以及dealloc方法。所有init...应放在一起，后面跟着其他NSObject的方法。
  8.3 初始化
    ·[建议] 不要在 init 方法中，将成员变量初始化为 0或 nil。如果一个对象可被复用，状态需要全部重置，这时可引入 reset方法。
    ·[强制] 局部变量不会被编译器初始化，所有局部变量使用前必须初始化。
  8.4 避免调用+ new
    ·[强制] 不要调用NSObject的类方法new，也不要在子类中重载它。使用alloc和init方法创建并初始化对象。
  8.5 保持公共 API 简单
    ·[建议] 保持公共API简单，避免“包含一切式“的API。
  8.6 #import与#include
    ·[强制] 使用#import来引用Objective-C/Objective-C++头文件，使用#include引用C/C++头文件。
  8.7 使用根框架
    ·[强制] 包含根框架，而非单独的文件。
    示例
    #import <Foundation/Foundation.h>     // good
    #import <Foundation/NSArray.h>        // avoid
    #import <Foundation/NSString.h>
    ...
  8.8 在init和dealloc中避免使用存取方法
    ·[建议] 在init和dealloc方法执行的过程中，子类可能会处在一个不稳定状态，所以这些方法中应避免调用存取方法，应在这些方法中直接对成员变量进行赋值或释放操作。
    示例
    - (instancetype)init {
        self = [super init];
        if (self) {
            _bar = [[NSMutableString alloc] init];  // good
        }
        return self;
    }

    - (void)dealloc {
        [_bar release];                           // good
        [super dealloc];
    }
    - (instancetype)init {
        self = [super init];
        if (self) {
            self.bar = [NSMutableString string];  // avoid
        }
        return self;
    }

    - (void)dealloc {
        self.bar = nil;                         // avoid
        [super dealloc];
    }
  8.9 按照声明顺序销毁实例变量
    ·[建议] dealloc中对象被释放的顺序应该与他们在@interface中声明的顺序一致，这有助于代码检查。
  8.10 setter中对NSString进行copy
    ·[建议] 接受NSString作为参数的setter，应该copy它所接受的字符串。
    示例
    - (void)setFoo:(NSString *)aFoo {
        [_foo autorelease];
        _foo = [aFoo copy];
    }
  8.11 避免抛出异常
    ·[建议] 不要@throw Objective-C 异常，但要注意从第三方的调用或者系统调用捕捉异常。如果确实使用了异常，请注释期望什么方法抛出异常。
  8.12 BOOL的使用
    ·[强制] 不要直接将 BOOL 值与 YES 、NO进行比较。
    ·[建议] 对 BOOL 使用逻辑运算符（&&, || 和 !）是合法的，返回值也可以安全地转换成 BOOL。
    示例
    - (BOOL)isBold {
        return [self fontTraits] & NSFontBoldTrait;
    }

    - (BOOL)isValid {
        return [self stringValue];
    }
    - (BOOL)isBold {
        return ([self fontTraits] & NSFontBoldTrait) ? YES : NO;
    }

    - (BOOL)isValid {
        return [self stringValue] != nil;
    }

    - (BOOL)isEnabled {
        return [self isValid] && [self isBold];
    }

    同样，不要直接比较 YES/NO 和 BOOL 变量。
    示例
    BOOL great = [foo isGreat];
    if (great) {
      // ...be great!
    }
  8.13 属性
    8.13.1 命名
    ·[建议] 属性所关联的实例变量的命名必须遵守以下划线作为前缀的规则。属性的名字应该与成员变量去掉下划线后缀的名字一模一样。@property后面紧跟括号，不留空格。
    示例
    @interface MyClass : NSObject
    @property(copy, nonatomic) NSString *name;
    @end

    @implementation MyClass
    // No code required for auto-synthesis, else use:
    //   @synthesize name = _name;
    @end
  8.13.2 位置
    ·[强制] 属性的声明必须紧靠着类接口中的实例变量语句块。属性的定义必须在 @implementation 的类定义的最上方。他们的缩进与包含他们的 @interface 以及 @implementation 语句一样。
    示例
    @interface MyClass : NSObject {
      @private
        NSString *_name;
    }
    @property(copy, nonatomic) NSString *name;
    @end

    @implementation MyClass
    @synthesize name = _name;

    - (instancetype)init {
      ...
    }
    @end
  8.13.3 字符串应用copy属性
    ·[强制] 应总是用 copy 属性声明 NSString 属性.
  8.13.4 没有实例变量的接口
    ·[强制] 没有声明任何实例变量的接口，应省略空大括号。
    示例
    @interface MyClass : NSObject
    // Does a lot of stuff
    - (void)fooBarBam;
    @end
    @interface MyClass : NSObject {
    }
    // Does a lot of stuff
    - (void)fooBarBam;
    @end
  8.14 自动synthesize实例变量
      ·[建议] 优先考虑使用自动 synthesize 实例变量。
      示例
// Header file
@protocol Thingy
@property(nonatomic, copy) NSString *widgetName;
@end

@interface Foo : NSObject<Thingy>
// A guy walks into a bar.
@property(nonatomic, copy) NSString *bar;
@end

// Implementation file
@interface Foo ()
@property(nonatomic, retain) NSArray *baz;
@end

@implementation Foo
@synthesize widgetName = _widgetName;
@end
