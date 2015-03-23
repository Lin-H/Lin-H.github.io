---
layout: post
title:  "Welcome to Jekyll! 中文显示效果"
date:   2015-02-20 22:32:46
categories: jekyll update
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

{% highlight javascript %}
function isIdCardNo(num) {
    if (isNaN(num)) {
        alert("输入的不是数字！");
        return false;
    }
    var len = num.length,
        re;
    if (len == 15)
        re = new RegExp(/^(/d {
            6
        })() ? (/d{2})(/d {
            2
        })(/d{2})(/d {
            3
        }) $ / );
else if (len == 18)
    re = new RegExp(/^(/d {
        6
    })() ? (/d{4})(/d {
        2
    })(/d{2})(/d {
        3
    })(/d)$/);
else {
    alert("输入的数字位数不对！");
    return false;
}
var a = num.match(re);
if (a != null) {
    if (len == 15) {
        var D = new Date("19" + a[3] + "/" + a[4] + "/" + a[5]);
        var B = D.getYear() == a[3] && (D.getMonth() + 1) == a[4] && D.getDate() == a[5];
    } else {
        var D = new Date(a[3] + "/" + a[4] + "/" + a[5]);
        var B = D.getFullYear() == a[3] && (D.getMonth() + 1) == a[4] && D.getDate() == a[5];
    }
    if (!B) {
        alert("输入的身份证号 " + a[0] + " 里出生日期不对！");
        return false;
    }
}
return true;
}
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].[hao123](http://www.hao123.com)

**测试中文显示效果**

【司汤达说】①若要品尝`快乐`，不可缺的条件是心无不安；②一个人只要有纯洁的心灵，无愁无恨，他的青春时期定可延长；③恋人永远是胆战心惊的；④不知隐讳，即不知爱。1842年的今天，法国作家司汤达逝世，生前他写下这样的墓志铭：“活过、爱过、写过”。你读过他的《红与黑》吗？

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
