##欢迎参与 Rails Guide 4.0.0 中文翻译

这个项目是从 [rails/rails][1] fork 过来，专门做 rails guide 4.0.0 的中文翻译。

###参与翻译
- fork 这个repo(默认分支是`4-0-stable`)
- cd railsguide-zh/guides/source/zh
- 在 [翻译进度][5] 中加入自己要翻译的文章
- 开始翻译
- 发pull-request 回来


###翻译注意事项

- 查看[翻译注意事项][6]

### 翻译进度

查看 [翻译进度][5]




###生成预览:

``` ruby
cd railsguide-zh/guides
bundle install
bundle exec rake guides:generate  GUIDES_LANGUAGE=zh
```
在 `railsguide-zh/guides/output` 下会生成文档.



###MarkDown 语法

原文和译文都采用 `markdown` 语法，可以参考 [markdown-en][2] 和 [markdown-zh][3]



### 问题和建议

有任何问题可以发邮件至 railsguide-zh@googlegroups.com ，或者[提issue][4]


###License

与 rails 一样 ，在  [MIT License](http://www.opensource.org/licenses/MIT) 下发布.




[1]:https://github.com/rails/rails.git
[2]:http://daringfireball.net/projects/markdown/
[3]:http://wowubuntu.com/markdown/
[4]:https://github.com/rails-learning/railsguide-zh/issues
[5]:https://github.com/rails-learning/railsguide-zh/issues/1
[6]:https://github.com/rails-learning/railsguide-zh/wiki/%E7%BF%BB%E8%AF%91%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9
