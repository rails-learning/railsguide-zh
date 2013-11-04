Rails 命令行
======================

Rails 提供了你需要的每一个命令行工具:

通过这篇 guide 你将了解:

* 如何创建一个 Rails 应用.
* 如何生成 models, controllers, database migrations, and unit tests.
* 如何启动一个开发环境的 server.
* 如何通过可交互的 shell 与 objects 交互。
* 如何配置你新创建的应用。

--------------------------------------------------------------------------------

注意: 这篇 guide 假定你已经读过 [Getting Started with Rails Guide](getting_started.html), 了解了一些 Rails 的基本知识。

Command Line 入门
-------------------

这里是一些 Rails 日常应用中非常重要的 commands , 按常用频率排序如下：

* `rails console`
* `rails server`
* `rake`
* `rails generate`
* `rails dbconsole`
* `rails new app_name`

让我们通过创建一个简单的 Rails 程序来看看每个 commands 的作用。

### `rails new`

安装完 Rails 之后的第一件事是运行 `rails new` 来创建一个新的 Rails 应用。

INFO: 如果你还没有安装 Rails ,可以通过 `gem install rails` 来安装 Rails。

```bash
$ rails new commandsapp
     create
     create  README.rdoc
     create  Rakefile
     create  config.ru
     create  .gitignore
     create  Gemfile
     create  app
     ...
     create  tmp/cache
     ...
        run  bundle install
```

通过这么一个简单的命令， Rails 为你创建一大堆的东西。你已经得到了整个Rails的目录结构，里面包含了你运行我们的示例程序所有的代码。

### `rails server`

`rails server` (`rails s`) 命令会启动一个名叫 WEBrick 的 web 服务器。运行这个命令之后，你就可以通过浏览器来访问你的应用了。

INFO: WEBrick isn't your only option for serving Rails. We'll get to that [later](#server-with-different-backends).


不需要任何额外的工作, `rails server` 就可以运行我们新建的闪亮的 Rails 应用 :-) :

```bash
$ cd commandsapp
$ rails server
=> Booting WEBrick
=> Rails 4.0.0 application starting in development on http://0.0.0.0:3000
=> Call with -d to detach
=> Ctrl-C to shutdown server
[2012-05-28 00:39:41] INFO  WEBrick 1.3.1
[2012-05-28 00:39:41] INFO  ruby 1.9.2 (2011-02-18) [x86_64-darwin11.2.0]
[2012-05-28 00:39:41] INFO  WEBrick::HTTPServer#start: pid=69680 port=3000
```

仅用了三个命令，我们就启动了 Rails server 监听 3000 端口。启动你的浏览器打开 [http://localhost:3000](http://localhost:3000)， 你就可以看到一个基本的 Rails 应用运行。

INFO：你也可以使用 `rails s` 来启动服务器。


使用 `-p` 选项可以为服务器指定特定的监听端口(默认是3000)。`-e` 选项可以改变默认的配置的开发环境。

```bash
$ rails server -e production -p 4000
```

`-b` 选项绑定 Rails 到特定的 IP, 默认是 0.0.0.0。`-d` 选项可以将 server 作为一个 daemon 启动。

### `rails generate`

`rails generate` 命令会使用模板创建一大堆东西。运行 `rails generate` 会给出一系列可用的
generators:


INFO: 你可以用 "g" 作为 "generator" 的简写来运行 generator 命令: `rails g`

```bash
$ rails generate
Usage: rails generate GENERATOR [args] [options]

...
...

Please choose a generator below.

Rails:
  assets
  controller
  generator
  ...
  ...
```

NOTE: 你可以通过 generator gems 安装更多的 generators， 有些 generators 插件你可能会安装，但是，你也可以创建你自己的哦~

使用 generators 可以帮助你省下大量写 **模板代码** 的时间，那些为了保证你的程序可以运行的代码。

我们来使用 controller generator 来创建自己的 controller。该用什么命令呢？让我们来问一下 generator：

INFO：所有的 Rails 控制台工具都包含帮助文档。像大多数 *NIX 工具一样，你可以添加 `--help` 或者 `-h` 来查看帮助，例如 `rails server --help`。

```bash
$ rails generate controller
Usage: rails generate controller NAME [action action] [options]

...
...

Description:
    ...

    To create a controller within a module, specify the controller name as a
    path like 'parent_module/controller_name'.

    ...

Example:
    `rails generate controller CreditCard open debit credit close`

    Credit card controller with URLs like /credit_card/debit.
        Controller: app/controllers/credit_card_controller.rb
        Test:       test/controllers/credit_card_controller_test.rb
        Views:      app/views/credit_card/debit.html.erb [...]
        Helper:     app/helpers/credit_card_helper.rb
```

controller generator 期待我们以 `generator controller ControllerName action1 action2` 格式来传递参数。我们来创建一个带有 **hello** action 的 `Greetings` controller, 用来输出一些问候语。 

```bash
$ rails generate controller Greetings hello
     create  app/controllers/greetings_controller.rb
      route  get "greetings/hello"
     invoke  erb
     create    app/views/greetings
     create    app/views/greetings/hello.html.erb
     invoke  test_unit
     create    test/controllers/greetings_controller_test.rb
     invoke  helper
     create    app/helpers/greetings_helper.rb
     invoke    test_unit
     create      test/helpers/greetings_helper_test.rb
     invoke  assets
     invoke    coffee
     create      app/assets/javascripts/greetings.js.coffee
     invoke    scss
     create      app/assets/stylesheets/greetings.css.scss
```

这个命令生成了什么？它在应用程序下生成了一系列的目录，创建了一个 controller 文件,一个 view 文件, 一个 functional test 文件，一个 和 view 相关的 helper ，一个 JavaScript 文件和一个 stylesheet 文件。

打开 controller 进行如下修改： ( 在`app/controllers/greetings_controller.rb`）

```ruby
class GreetingsController < ApplicationController
  def hello
    @message = "Hello, how are you today?"
  end
end
```

修改 view , 显示这条 message  (in `app/views/greetings/hello.html.erb`):

```erb
<h1>A Greeting for You!</h1>
<p><%= @message %></p>
```

执行 `rails server` 来启动你的 Server 。

```bash
$ rails server
=> Booting WEBrick...
```

URL 在 [http://localhost:3000/greetings/hello](http://localhost:3000/greetings/hello)。

INFO：在一个正常的，原生的 Rails 应用中，你的 URL 会符合这种模式 http://(host)/(controller)/(action),http://(host)/(controller) 这个 URL 会被对应到 **index** 这个 action。

Rails 同样包含了 data modes 的 generator。

```bash
$ rails generate model
Usage:
  rails generate model NAME [field[:type][:index] field[:type][:index]] [options]

...

Active Record options:
      [--migration]            # Indicates when to generate migration
                               # Default: true

...

Description:
    Create rails files for model generator.
```

NOTE: 关于可用的 field types 列表， 请参考 [API documentation](http://api.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/TableDefinition.html#method-i-column) `TableDefinition` 的 column 方法。


我们可以创建一个 scaffold ，而不是直接生成一个 model (后面会做)。Rails 中，一个 **scaffold** 是一个包含了 model, database migration , controller ,views 以及 test suite 的集合。

我们来创建一个叫做 "HighScore" 的简单 resource, 用来记录我们玩游戏的最高分数。

```bash
$ rails generate scaffold HighScore game:string score:integer
    invoke  active_record
    create    db/migrate/20120528060026_create_high_scores.rb
    create    app/models/high_score.rb
    invoke    test_unit
    create      test/models/high_score_test.rb
    create      test/fixtures/high_scores.yml
    invoke  resource_route
     route    resources :high_scores
    invoke  scaffold_controller
    create    app/controllers/high_scores_controller.rb
    invoke    erb
    create      app/views/high_scores
    create      app/views/high_scores/index.html.erb
    create      app/views/high_scores/edit.html.erb
    create      app/views/high_scores/show.html.erb
    create      app/views/high_scores/new.html.erb
    create      app/views/high_scores/_form.html.erb
    invoke    test_unit
    create      test/controllers/high_scores_controller_test.rb
    invoke    helper
    create      app/helpers/high_scores_helper.rb
    invoke      test_unit
    create        test/helpers/high_scores_helper_test.rb
    invoke  assets
    invoke    coffee
    create      app/assets/javascripts/high_scores.js.coffee
    invoke    scss
    create      app/assets/stylesheets/high_scores.css.scss
    invoke  scss
    create    app/assets/stylesheets/scaffolds.css.scss
```

这个 generator 会检查目录下的 models,controllers,helpers,layouts,functional and unit tests, stylesheets, 为 HighScore 创建 views, controllers, mode 和 database migration（创建 `high_scores` table and fields)。创建 **resource** 的 route 以及为一切新创建的东西添加新的测试。

<!--
The migration requires that we **migrate**, that is, run some Ruby code (living in that `20120528060026_create_high_scores.rb`) to modify the schema of our database. Which database? The sqlite3 database that Rails will create for you when we run the `rake db:migrate` command. We'll talk more about Rake in-depth in a little while.
-->

migration 过程需要我们执行 **migrate** ， 即运行一些 Ruby 代码 （ 在 `20120528060026_create_high_scores.rb` 里面) 来修改我们数据库的 schema。 什么数据库？
当我们运行 `rake db:migrate` 命令的时候 Rails 会帮我们创建 sqlite3 数据库。我们将在 后面的 Rake in-depth 更多介绍。

```bash
$ rake db:migrate
==  CreateHighScores: migrating ===============================================
-- create_table(:high_scores)
   -> 0.0017s
==  CreateHighScores: migrated (0.0019s) ======================================
```

<!--
INFO: Let's talk about unit tests. Unit tests are code that tests and makes assertions about code. In unit testing, we take a little part of code, say a method of a model, and test its inputs and outputs. Unit tests are your friend. The sooner you make peace with the fact that your quality of life will drastically increase when you unit test your code, the better. Seriously. We'll make one in a moment.
-->

INFO：来聊聊 unit tests 吧。Unit tests 是为我们业务逻辑代码编写的一些 tests 和 assertions 。在 uint test 中，我们用一小段代码，调用 model 中的一个方法，测试它的输入和输出。 Unit tests 是你的朋友。对你的代码进行单元测试可以大大增强你代码的质量，越早认识到这个事实，对你越有好处。的确如此。很快我们就会写一个例子。

<!--
Let's see the interface Rails created for us.
-->

让我们来看看 Rails 给我们创建的接口。

```bash
$ rails server
```

<!--
Go to your browser and open [http://localhost:3000/high_scores](http://localhost:3000/high_scores), now we can create new high scores (55,160 on Space Invaders!)
-->

启动你的浏览器，打开 [http://localhost:3000/high_scores](http://localhost:3000/high_scores) ， 现在我们就可以创建新的 high scores 了（在 Space Invaders 上创建 50, 160）

### `rails console`

<!--
The `console` command lets you interact with your Rails application from the command line. On the underside, `rails console` uses IRB, so if you've ever used it, you'll be right at home. This is useful for testing out quick ideas with code and changing data server-side without touching the website.
-->

`console` 命令让我们可以用 command line 来和 Rails application 交互。`rails console`使用 IRB，所以，如果你曾经用过，那就太好啦。借助这个工具我们可以在不去修改 website 的情况下修改 server 端的数据,快速测试我们新的想法，。

<!--
INFO: You can also use the alias "c" to invoke the console: `rails c`.
-->

INFO: 你可以使用简写 "c" 来启动 console： `rails c`。

<!--
You can specify the environment in which the `console` command should operate.
-->

你可以配置 `console` command 的环境。

```bash
$ rails console staging
```

<!--
If you wish to test out some code without changing any data, you can do that by invoking `rails console --sandbox`.
-->
如果你想测试一些代码而不想任何数据收到影响，可以使用 `rails console --sandbox`。

```bash
$ rails console --sandbox
Loading development environment in sandbox (Rails 4.0.0)
Any modifications you make will be rolled back on exit
irb(main):001:0>
```

### `rails dbconsole`
<!--
`rails dbconsole` figures out which database you're using and drops you into whichever command line interface you would use with it (and figures out the command line parameters to give to it, too!). It supports MySQL, PostgreSQL, SQLite and SQLite3.
-->

`rails dbconsole` 能查找出当前使用的是那种 database ，并且启动与之交互的命令行（并带有合适的参数!)。支持 MySQL, PostgreSQL, SQLite 和 SQLite3。

<!--
INFO: You can also use the alias "db" to invoke the dbconsole: `rails db`.
-->

INFO: 可以用简写 "db" 来启动 dbconsole: `rails db`。

### `rails runner`

<!--
`runner` runs Ruby code in the context of Rails non-interactively. For instance:
-->
`runner` 在非交互的 Rails 环境下运行一些 Ruby 代码。 例如：

```bash
$ rails runner "Model.long_running_method"
```

<!--
INFO: You can also use the alias "r" to invoke the runner: `rails r`.
-->

INFO: 可以使用简写 "r" 来启动 runner: `rails r`。

<!--
You can specify the environment in which the `runner` command should operate using the `-e` switch.
-->

你可以使用 `-e` option 来指定 `runner` command 运行的环境。

```bash
$ rails runner -e staging "Model.long_running_method"
```

### `rails destroy`

<!--
Think of `destroy` as the opposite of `generate`. It'll figure out what generate did, and undo it.
-->

`destory` 是 `generate` 的逆操作。它会计算出 generate 做了哪些，然后 undo it。

<!--
INFO: You can also use the alias "d" to invoke the destroy command: `rails d`.
-->

INFO: 你可以使用简写 "d" 来启动 destroy command： `rails d`。

```bash
$ rails generate model Oops
      invoke  active_record
      create    db/migrate/20120528062523_create_oops.rb
      create    app/models/oops.rb
      invoke    test_unit
      create      test/models/oops_test.rb
      create      test/fixtures/oops.yml
```
```bash
$ rails destroy model Oops
      invoke  active_record
      remove    db/migrate/20120528062523_create_oops.rb
      remove    app/models/oops.rb
      invoke    test_unit
      remove      test/models/oops_test.rb
      remove      test/fixtures/oops.yml
```

Rake
----

<!--
Rake is Ruby Make, a standalone Ruby utility that replaces the Unix utility 'make', and uses a 'Rakefile' and `.rake` files to build up a list of tasks. In Rails, Rake is used for common administration tasks, especially sophisticated ones that build off of each other.
-->

Rake 是 Ruby Make, 一个用来替代吗 Unix 工具 'make' 的独立 Ruby 工具， 使用 `Rakefile` 和 `.rake` 文件来完成一系列的任务。在 Rails 中，Rake 用来完成一些常见的管理任务，尤其是一些互相依赖的复杂任务。

<!--
You can get a list of Rake tasks available to you, which will often depend on your current directory, by typing `rake --tasks`. Each task has a description, and should help you find the thing you need.
-->

输入 `rake --tasks` 你就可以获取当前可用的的 Rake 任务列表，当然，这取决于你当前的环境。每个任务都有一个描述，可以帮助你找到你需要的东西。

```bash
$ rake --tasks
rake about              # List versions of all Rails frameworks and the environment
rake assets:clean       # Remove compiled assets
rake assets:precompile  # Compile all the assets named in config.assets.precompile
rake db:create          # Create the database from config/database.yml for the current Rails.env
...
rake log:clear          # Truncates all *.log files in log/ to zero bytes (specify which logs with LOGS=test,development)
rake middleware         # Prints out your Rack middleware stack
...
rake tmp:clear          # Clear session, cache, and socket files from tmp/ (narrow w/ tmp:sessions:clear, tmp:cache:clear, tmp:sockets:clear)
rake tmp:create         # Creates tmp directories for sessions, cache, sockets, and pids
```

### `about`

<!--
`rake about` gives information about version numbers for Ruby, RubyGems, Rails, the Rails subcomponents, your application's folder, the current Rails environment name, your app's database adapter, and schema version. It is useful when you need to ask for help, check if a security patch might affect you, or when you need some stats for an existing Rails installation.
-->

`rake about` 会给出当前工具的版本信息，包括： Ruby, RubyGems， Rails，Rails 的子模块，你的应用车程序文件夹，当前 Rails 环境的名称，database ，schema。 这些信息是非常有用的，比如： 当你要查看一个安全性的 Patch 是否会影响到你，或者你需要当前安装的 Rails 的一些统计信息。

```bash
$ rake about
About your application's environment
Ruby version              1.9.3 (x86_64-linux)
RubyGems version          1.3.6
Rack version              1.3
Rails version             4.0.0
JavaScript Runtime        Node.js (V8)
Active Record version     4.0.0
Action Pack version       4.0.0
Action Mailer version     4.0.0
Active Support version    4.0.0
Middleware                Rack::Sendfile, ActionDispatch::Static, Rack::Lock, #<ActiveSupport::Cache::Strategy::LocalCache::Middleware:0x007ffd131a7c88>, Rack::Runtime, Rack::MethodOverride, ActionDispatch::RequestId, Rails::Rack::Logger, ActionDispatch::ShowExceptions, ActionDispatch::DebugExceptions, ActionDispatch::RemoteIp, ActionDispatch::Reloader, ActionDispatch::Callbacks, ActiveRecord::Migration::CheckPending, ActiveRecord::ConnectionAdapters::ConnectionManagement, ActiveRecord::QueryCache, ActionDispatch::Cookies, ActionDispatch::Session::EncryptedCookieStore, ActionDispatch::Flash, ActionDispatch::ParamsParser, Rack::Head, Rack::ConditionalGet, Rack::ETag
Application root          /home/foobar/commandsapp
Environment               development
Database adapter          sqlite3
Database schema version   20110805173523
```

### `assets`

<!--
You can precompile the assets in `app/assets` using `rake assets:precompile` and remove those compiled assets using `rake assets:clean`.
-->

你可以使用 `rake assets:precompile` 预编译 `app/assets` 下的 assets , 使用 `rake assets:clean` 删除预编译的 assets.

### `db`

<!--
The most common tasks of the `db:` Rake namespace are `migrate` and `create`, and it will pay off to try out all of the migration rake tasks (`up`, `down`, `redo`, `reset`). `rake db:version` is useful when troubleshooting, telling you the current version of the database.
-->

Rake namespace  `db:` 最常用的 task 是： `migrate` 和 `create`,这两个命令可以使你从其他的 task (`up`, `down`, `redo`,`reset`) 中解放出来。 调试问题的时候，`rake db:version` 是一个有用的命令，它可以告诉你数据库的当前版本。

<!--
More information about migrations can be found in the [Migrations](migrations.html) guide.
-->

关于 migrations 更多的信息，查看 [Migrations](migrations.html)

### `doc`
<!--
The `doc:` namespace has the tools to generate documentation for your app, API documentation, guides. Documentation can also be stripped which is mainly useful for slimming your codebase, like if you're writing a Rails application for an embedded platform.
-->

`doc:` 包含了为你的应用 app, api, guides 生成文档的工具。同样也可以帮助你将文档从代码中剥离，当你为嵌入式平台编写 Rails 应用的时候，这种方式可以帮助你精简代码。

<!--
* `rake doc:app` generates documentation for your application in `doc/app`.
* `rake doc:guides` generates Rails guides in `doc/guides`.
* `rake doc:rails` generates API documentation for Rails in `doc/api`.
-->

* `rake doc:app` 在 `doc/app` 生成你应用程序的文档。
* `rake doc:guides` 在 `doc/guides` 下生成 guides。
* `rake doc:rails` 在 `doc/api` 下生成 api 文档。

### `notes`

<!--
`rake notes` will search through your code for comments beginning with FIXME, OPTIMIZE or TODO. The search is done in files with extension `.builder`, `.rb`, `.erb`, `.haml` and `.slim` for both default and custom annotations.
-->

`rake notes` 可以帮助你查找代码中以 FIXME, OPTIMIZE 和 TODO 开头的 comments。查找会在在扩展名是 `.builder`,`.rb`,`.erb.`,`.haml` 和 `.slim` 的文件中进行。

```bash
$ rake notes
(in /home/foobar/commandsapp)
app/controllers/admin/users_controller.rb:
  * [ 20] [TODO] any other way to do this?
  * [132] [FIXME] high priority for next deploy

app/models/school.rb:
  * [ 13] [OPTIMIZE] refactor this code to make it faster
  * [ 17] [FIXME]
```

<!--
If you are looking for a specific annotation, say FIXME, you can use `rake notes:fixme`. Note that you have to lower case the annotation's name.
-->

如果你要查找一个特定的 annotation, 如 FIXME , 你可以用 `rake notes:fixme`.注意 annotaion 名字要小写。

```bash
$ rake notes:fixme
(in /home/foobar/commandsapp)
app/controllers/admin/users_controller.rb:
  * [132] high priority for next deploy

app/models/school.rb:
  * [ 17]
```
<!--
You can also use custom annotations in your code and list them using `rake notes:custom` by specifying the annotation using an environment variable `ANNOTATION`.
-->

你也可以在代码中使用自定义的 annotations , 使用 `rake notes:custom` 并且给定环境变量  `ANNOTATION` 一个指定的值。

```bash
$ rake notes:custom ANNOTATION=BUG
(in /home/foobar/commandsapp)
app/models/post.rb:
  * [ 23] Have to fix this one before pushing!
```

<!--
NOTE. When using specific annotations and custom annotations, the annotation name (FIXME, BUG etc) is not displayed in the output lines.
-->

注意: 当你使用特定的 specific annotation 和 自定义的 annotation 的时候，annotation 的名字 （FIXME,BUG 等)是不会再输出中显示的。

<!--
By default, `rake notes` will look in the `app`, `config`, `lib`, `bin` and `test` directories. If you would like to search other directories, you can provide them as a comma separated list in an environment variable `SOURCE_ANNOTATION_DIRECTORIES`.
-->

`rake notes` 默认会在 `app`, `config`, `lib`,`bin`,`test` 目录下查找。如果想加入其他目录，可以传入在环境变量 `SOURCE_ANNOTATION_DIRECTORIES` 中， 以逗号分隔。

```bash
$ export SOURCE_ANNOTATION_DIRECTORIES='spec,vendor'
$ rake notes
(in /home/foobar/commandsapp)
app/models/user.rb:
  * [ 35] [FIXME] User should have a subscription at this point
spec/models/user_spec.rb:
  * [122] [TODO] Verify the user that has a subscription works
```

### `routes`

<!--
`rake routes` will list all of your defined routes, which is useful for tracking down routing problems in your app, or giving you a good overview of the URLs in an app you're trying to get familiar with.
-->

`rake routes` 可以列出所有的 routes, 这对于你跟踪 app 的 routing 问题是很有帮助的，也可以给你呈现一个所有 URLs 的概览。

### `test`

<!--
INFO: A good description of unit testing in Rails is given in [A Guide to Testing Rails Applications](testing.html)
-->

INFO: 关于 unit testing in Rails 的介绍请参考 [A Guide to Testing Rails Applications](testing.html)

<!--
Rails comes with a test suite called `Test::Unit`. Rails owes its stability to the use of tests. The tasks available in the `test:` namespace helps in running the different tests you will hopefully write.
-->

Rails 自带了一个 `Test::Unit` 的测试组件。Rails 自身拥有测试的能力。`test:` 下的 tasks 可以帮助你运行各种不同的 tests。 

### `tmp`

<!--
The `Rails.root/tmp` directory is, like the *nix /tmp directory, the holding place for temporary files like sessions (if you're using a file store for files), process id files, and cached actions.
-->

`Rails.root/tmp` 目录，和 *nix/tmp 目录一样，保存一些临时文件，如： sesions，进程 id 或者 cached actions。

<!--
The `tmp:` namespaced tasks will help you clear the `Rails.root/tmp` directory:
-->

`tmp:` 下的 tasks 可以帮助你清除 `Rails.root/tmp` 目录:

* `rake tmp:cache:clear` clears `tmp/cache`.
* `rake tmp:sessions:clear` clears `tmp/sessions`.
* `rake tmp:sockets:clear` clears `tmp/sockets`.
* `rake tmp:clear` clears all the three: cache, sessions and sockets.

### Miscellaneous

<!--
* `rake stats` is great for looking at statistics on your code, displaying things like KLOCs (thousands of lines of code) and your code to test ratio.
* `rake secret` will give you a pseudo-random key to use for your session secret.
* `rake time:zones:all` lists all the timezones Rails knows about.
-->

* `rake stats` 是一个显示你代码统计信息的工具，显示类似 KLOCs(千行代码）和你的测试代码比率。
* `rake secret` 会产生一个随机的密钥作为你的会话密钥。
* `rake time:zones:all` 将列出 Rails 知道的所有的时区信息。


### Custom Rake Tasks

<!--
Custom rake tasks have a `.rake` extension and are placed in `Rails.root/lib/tasks`.
-->

自定义的 rake tasks 有一个 `.rake` 的扩展名，并且要放置在 `Rails.root/lib/tasks` 目录下。

```ruby
desc "I am short, but comprehensive description for my cool task"
task task_name: [:prerequisite_task, :another_task_we_depend_on] do
  # All your magic here
  # Any valid Ruby code is allowed
end
```

To pass arguments to your custom rake task:

```ruby
task :task_name, [:arg_1] => [:pre_1, :pre_2] do |t, args|
  # You can use args from here
end
```

You can group tasks by placing them in namespaces:

```ruby
namespace :db do
  desc "This task does nothing"
  task :nothing do
    # Seriously, nothing
  end
end
```

Invocation of the tasks will look like:

```bash
rake task_name
rake "task_name[value 1]" # entire argument string should be quoted
rake db:nothing
```

NOTE: If your need to interact with your application models, perform database queries and so on, your task should depend on the `environment` task, which will load your application code.

The Rails Advanced Command Line
-------------------------------

More advanced use of the command line is focused around finding useful (even surprising at times) options in the utilities, and fitting those to your needs and specific work flow. Listed here are some tricks up Rails' sleeve.

### Rails with Databases and SCM

When creating a new Rails application, you have the option to specify what kind of database and what kind of source code management system your application is going to use. This will save you a few minutes, and certainly many keystrokes.

Let's see what a `--git` option and a `--database=postgresql` option will do for us:

```bash
$ mkdir gitapp
$ cd gitapp
$ git init
Initialized empty Git repository in .git/
$ rails new . --git --database=postgresql
      exists
      create  app/controllers
      create  app/helpers
...
...
      create  tmp/cache
      create  tmp/pids
      create  Rakefile
add 'Rakefile'
      create  README.rdoc
add 'README.rdoc'
      create  app/controllers/application_controller.rb
add 'app/controllers/application_controller.rb'
      create  app/helpers/application_helper.rb
...
      create  log/test.log
add 'log/test.log'
```

We had to create the **gitapp** directory and initialize an empty git repository before Rails would add files it created to our repository. Let's see what it put in our database configuration:

```bash
$ cat config/database.yml
# PostgreSQL. Versions 8.2 and up are supported.
#
# Install the pg driver:
#   gem install pg
# On OS X with Homebrew:
#   gem install pg -- --with-pg-config=/usr/local/bin/pg_config
# On OS X with MacPorts:
#   gem install pg -- --with-pg-config=/opt/local/lib/postgresql84/bin/pg_config
# On Windows:
#   gem install pg
#       Choose the win32 build.
#       Install PostgreSQL and put its /bin directory on your path.
#
# Configure Using Gemfile
# gem 'pg'
#
development:
  adapter: postgresql
  encoding: unicode
  database: gitapp_development
  pool: 5
  username: gitapp
  password:
...
...
```

It also generated some lines in our database.yml configuration corresponding to our choice of PostgreSQL for database.

NOTE. The only catch with using the SCM options is that you have to make your application's directory first, then initialize your SCM, then you can run the `rails new` command to generate the basis of your app.
