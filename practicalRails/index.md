### Chapter5
* [ディレクティブ](#anchor1)
* [layoutメソッド](#anchor2)
* [Regexp](#anchor3)
* [プリコンパイル](#anchor4)
### Chapter6
* [Exeptionクラス](#anchor5)
* [raiseメソッド](#anchor6)
* [rescue_form](#anchor7)
* [productionモードのログを見る(tail)](#anchor8)
* [requestオブジェクト](#anchor9)







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
### <a href="#anchor7">rescue_form</a>

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