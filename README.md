##欢迎参与 Rails Guide 4.0.0 中文翻译

这个项目是从 [rails/rails][1] fork 过来，专门做 rails guide 4.0.0 的中文翻译。

###参与翻译
- fork 这个repo(默认分支是`4-0-stable`)
- cd railsguide-zh/guides/source/zh
- 翻译你感兴趣的文章
- 发pull-request 回来

###生成预览:

``` ruby
cd railsguide-zh/guides
bundle install
bundle exec rake guides:generate  GUIDES_LANGUAGE=zh
```
在 `railsguide-zh/guides/output` 下会生成文档.



###MarkDown 语法

原文和译文都采用 `markdown` 语法，可以参考 [markdown-en][2] 和 [markdown-zh][3]



### 问题

有任何问题可以发邮件至 railsguide-zh@googlegroups.com ，或者[提issue][4]







[1]:https://github.com/rails/rails.git
[2]:http://daringfireball.net/projects/markdown/
[3]:http://wowubuntu.com/markdown/
[4]:https://github.com/rails-learning/railsguide-zh/issues
