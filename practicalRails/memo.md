エラーメッセージ  
エラー処理  
seedファイル
### ch5 no.2005  
productionモードで起動  
```
config.hosts << "localhost"
```
config/production.rbに追加。  

### ch6
エラーの処理  
エラーメッセージのテンプレート作成

### 演習  
プリコンパイルの対象を追加した場合は  
アセットのプリコンパイルを行ってからproductionモードを開く。

### ch７
ユーザー認証  
rspec
```
     ActiveModel::UnknownAttributeError:
       unknown attribute 'hashed_password' for StaffMember.
```
StaffMemberモデルにhashed_passwordなんてありませんよ。
### 演習
seedデータを読み込む際にseed.rb
```
table_names = %w(administrators)
table_names.each do |table_name|
  path = Rails.root.join("db","seeds",Rails.env,"#{table_name}.rb")
    if File.exist?(path)
      puts "Creating #{table_name}...."
      require(path)
    end
  end
```
とする事でseeds/development/administrators.rbが読み込まれる。