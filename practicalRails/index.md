### Chapter5
* [ディレクティブ](#anchor1)
* [layoutメソッド](#anchor2)
* [Regexp](#anchor3)
* [プリコンパイル](#anchor4)
### Chapter6
* [Exeptionクラス](#anchor5)
* [raiseメソッド](#anchor6)
* [rescue_from](#anchor7)
* [productionモードのログを見る(tail)](#anchor8)
* [requestオブジェクト](#anchor9)
* [ActiveSupport::Concern](#anchor10)
### Chapter7
* [LOWER関数](#anchor11)
* [シードデータ](#anchor12)
* [遅延初期化](#anchor13)
* [遅延初期化](#anchor14)
* [asオプション](#anchor15)








<a id="anchor1"></a>
### <a href="#anchor1">ディレクティブ</a>
- コメントの中の*=から始まる行の事で、  
ここでアセットパイプラインの設定を行っている。

```
/*
*= require_tree .
*= require_self 
*/
```
require_tree .は現在のディレクトリ以下全てをを対象とする。


<a id="anchor2"></a>
### <a href="#anchor2">layoutメソッド</a>
- レイアウトを決定する為のメソッド
  ```
  <!-- /application_controller.rb -->
  layout 'top'
  ```
  のように記述する事でtop.html.erbが呼び出される。  
  参考:https://pikawaka.com/rails/layout

<a id="anchor3"></a>
### <a href="#anchor3">Regexp</a>
正規表現のクラス。  
参考:https://docs.ruby-lang.org/ja/latest/class/Regexp.html#S_LAST_MATCH


<a id="anchor4"></a>
### <a href="#anchor4">プリコンパイル</a>
<a id="anchor5"></a>
### <a href="#anchor5">Exceptionクラス</a>
Ruby言語の例外を表現する為のクラス。

```
Exception → StandardError → ActiveRecord::ActiveRecordError
```
よく見る
ActiveRecord::ActiveRecordErrorNotFoundとかのおじいちゃん。

<a id="anchor6"></a>
### <a href="#anchor6">raise</a>

例外を発生させるメソッド
```
raise 例外のクラス名, 例外を説明するメッセージ(省略可)
```

<a id="anchor7"></a>
### <a href="#anchor7">rescue_from</a>

アクション内で発生した例外の処理方法を簡単に指定できるメソッド。
```
rescue_from StandardError,with: :rescue500

  private 
    def rescue500(e)
      render "errors/interal_server_error",status: 500
    end
```

引数にExceptionオブジェクトが渡される。  
親子関係にある例外を指定する場合、親（先祖）の方を先に指定しなくてはならない。
<a id="anchor8"></a>
### <a href="#anchor8">productionモードのログを見る</a>  
logディレクトリのproduction.logにログが記録させている。
```
tail -f apps/baukis2/log/productiton.log
```
ホストOS側のターミナルで実行する。


<a id="anchor9"></a>
### <a href="#anchor9">requestオブジェクト</a>  
ActionDispatch::Requestクラスのインスタンス。  
クライアントからのリクエストに関する情報を保持するオブジェクト。  
pathメソッド,urlメソッド,ipメソッドなどを持っている。
```
"あなたのIPアドレス(#{request.ip})からは利用できません。"
```

<a id="anchor10"></a>
### <a href="#anchor10">ActiveSupport::Concern</a>  
extend ActiveSupport::Concern  
concernsディレクトリに置くモジュールには必ずこの記述が必要になる。  
モジュールに２つの性質が加えられる

１,includeメソッドが使えるようになる  
```
  included do
    rescue_from StandardError,with: :rescue500
    rescue_from ApplicationController::Forbidden, with: :rescue403
    rescue_from ApplicationController::IpAddressRejected, with: :rescue403
    rescue_from ActiveRecord::RecordNotFound, with: :rescue404
  end
```
ブロック内のコードがモジュールを読み込んだクラスの文脈で評価される。  

2,ClassMethods  
サブクラスとしてClassMethodsというクラスを定義しておくと  
モジュールを読み込んだクラスのクラスメソッドとして取り込まれる。  
参考：https://qiita.com/h-shima/items/d772b4cbe7368ddb8255

<a id="anchor11"></a>
### <a href="#anchor11">LOWER関数</a>
SQLの関数で引数の文字列を小文字のアルファベットに変換して返す。  
```
    add_index :staff_members,"LOWER(email)",unique: true
```
今回の場合メールアドレスの大文字、小文字を区別しない為に使われている。

<a id="anchor12"></a>
### <a href="#anchor12">シードデータ</a>
Railsアプリケーションを正常に機能させる為に予めDBへ投入しておくデータ。

<a id="anchor13"></a>
### <a href="#anchor13">遅延初期化</a>
```
  def current_staff_member
      if session[:staff_member_id]
        @current_staff_member || = StaffMember.find_by(id:session[:staff_member_id])
      end
    end
```

<a id="anchor14"></a>
### <a href="#anchor14">helper_method</a>
  引数に指定したシンボルと同名のメソッドをヘルパーメソッドとして登録するメソッド。

  <a id="anchor15"></a>
### <a href="#anchor15">asオプション</a>
```
  namespace :staff do
    root "top#index"
    get "login" => "sessions#new", as: :login
```
ルーティングに名前を付ける。この場合:staff_loginというシンボルを用いてURLパスを参照できるようになる。