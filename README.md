# この環境の作成方法

```
mkdir your_app
cd your_app
bundle init
Gemfileのrailsのコメントアウトを取る。
bundle install --path vendor/bundle
bundle config --local build.mysql2 "--with-ldflags=-L/usr/local/opt/openssl/lib —with-cppflags=-I/usr/local/opt/openssl/include"
rails new . --skip-coffee --skip-sprockets --skip-turbolinks --webpack --database=mysql -T
Gemfileでwebpackerの下あたりにgem ‘react-rails’を追加
bundle install
rails webpacker:install:react
rails generate react:install
rails g react:component HelloWorld greeting:string
rails generate controller Welcomes
config/routes.rbに以下を追加する。
root ’welcomes#index’ 
rails db:migrate
```

# react-railsを使ってwebpackでビルドしたreact fileを呼ぶ。

```
app/views/welcomes/index.html.erbに以下の記述をする
<%= react_component(“HelloWorld”, { greeting: “Hello” }) %>

app/views/layouts/application.html.erbのjavascript_pack_tagを以下を変更する。
<%= javascript_pack_tag ‘application’ %>
タブを複数開き、一方ではbin/webpack-dev-server、もう一方ではrails sをする
```

# webpackがビルドしたjsファイルを直接呼ぶ。

```
public/packs/manifest.jsonからhello_react.jsのpathを取得してjavascript_include_tagで直接呼んでみる。（これはHello React!を返す。）

app/views/layouts/application.html.erbからjavascript_pack_tagをコメントアウトする。
app/views/welcomes/index.html.erbからreact_componentをコメントアウトする。
この段階だとHello React!は消える
かわりにpublic/packs/manifest.jsonからhello_react.jsのpathを取得し、
<%= javascript_include_tag '/packs/hello_react-ダイジェスト' %>
と書くと、Hello React!が出てくる。
ちなみにapplication.jsにするとconsole.logに'Hello World from Webpacker’と出る。
```

# frontend開発をrails側から分けてみる
```
app/views/layouts/application.html.erbとindex.html.erbを元に戻し
mv app/javascript frontend
config/webpacker.ymlにてsource_pathをfrontendに変更する。
するとGreeting: Helloが出る。
```
