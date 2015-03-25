---
layout: post
title:  "Welcome to Jekyll! 中文显示效果"
date:   2015-02-20 22:32:46
categories: jekyll update
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```

```javascript
var mySingleton = function () {

    /* 这里声明私有变量和方法 */
    var privateVariable = 'something private';
    function showPrivate() {
        console.log(privateVariable);
    }

    /* 公有变量和方法（可以访问私有变量和方法） */
    return {
        publicMethod: function () {
            showPrivate();
        },
        publicVar: 'the public can see this!'
    };
};

var single = mySingleton();
single.publicMethod();  // 输出 'something private'
console.log(single.publicVar); // 输出 'the public can see this!'
```

{% highlight c %}
#include<stdio.h> 
#include<windows.h>//基本型态定义。支援型态定义函数。使用者界面函数 图形装置界面函数。
#include<conio.h>    //用户通过按键盘产生的对应操作 (控制台） 
#include<stdlib.h> 
#include<time.h> //日期和时间头文件 
#define LEN 30
#define WID 25 
int Snake[LEN][WID] = {0};   //数组的元素代表蛇的各个部位 
char Sna_Hea_Dir = 'a';//记录蛇头的移动方向
int Sna_Hea_X, Sna_Hea_Y;//记录蛇头的位置
int Snake_Len = 3;//记录蛇的长度
clock_t Now_Time;//记录当前时间，以便自动移动
int Wait_Time ;//记录自动移动的时间间隔
int Eat_Apple = 1;//吃到苹果表示为1
int Level ;
int All_Score = -1;
int Apple_Num = -1;
HANDLE hConsole = GetStdHandle(STD_OUTPUT_HANDLE);  //获取标准输出的句柄 <windows.h>
//句柄 ：标志应用程序中的不同对象和同类对象中的不同的实例 方便操控，
void gotoxy(int x, int y)//设置光标位置
 {
     COORD pos = {x,y};  //定义一个字符在控制台屏幕上的坐标POS
     
    SetConsoleCursorPosition(hConsole, pos);    //定位光标位置的函数<windows.h>
 
}
 
void Hide_Cursor()//隐藏光标 固定函数 
 {
    CONSOLE_CURSOR_INFO cursor_info = {1, 0}; 
    SetConsoleCursorInfo(hConsole, &cursor_info);    
 }
 
void SetColor(int color)//设置颜色
 {
     SetConsoleTextAttribute(hConsole, color);
//是API设置字体颜色和背景色的函数 格式：SetConsoleTextAttribute(句柄,颜色);
 }

void Print_Snake()//打印蛇头和蛇的脖子和蛇尾
 {
     int iy, ix, color;
     for(iy = 0; iy < WID; ++iy)
         for(ix = 0; ix < LEN; ++ix)
         {
 
            if(Snake[ix][iy] == 1)//蛇头
             {
                 SetColor(0xf);            //oxf代表分配的内存地址  setcolor:34行自定义设置颜色的函数 
                 gotoxy(ix*2, iy);
                 printf("※");
             }
             if(Snake[ix][iy] == 2)//蛇的脖子
             {
                 color = rand()%15 + 1;  //rand()函数是产生随机数的一个随机函数。C语言里还有 srand()函数等。
//头文件:stdlib.h 
                 if(color == 14)
                     color -= rand() % 13 + 1;  //变色 
                 SetColor(color);
                 gotoxy(ix*2, iy);
                 printf("■");
             }
             if(Snake[ix][iy] == Snake_Len)
             {
                 gotoxy(ix*2, iy);
                 SetColor(0xe);
                 printf("≈");
             }
         }
 }
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].[hao123](http://www.hao123.com)

**测试中文显示效果**

【司汤达说】①若要品尝`快乐`，不可缺的条件是心无不安；②一个人只要有纯洁的心灵，无愁无恨，他的青春时期定可延长；③恋人永远是胆战心惊的；④不知隐讳，即不知爱。1842年的今天，法国作家司汤达逝世，生前他写下这样的墓志铭：“活过、爱过、写过”。你读过他的《红与黑》吗？

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
