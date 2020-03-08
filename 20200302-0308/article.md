## Node/Express
### POSTで受け取ったbodyが空になっている
https://blog.ryo4004.net/web/306/  
body-parserのjson()を実行しないと空になるらしい。  

## PHP/Laravel
### インストール
composerコマンドからインストールできる。  
```
composer create-project laravel/laravel プロジェクト名 --prefer-dist
```
--prefer-dist オプションをつけると圧縮版がインストールされるのでオススメ。  

### 初期設定
諸々英語になっているので日本語に言語設定を変更する。  
config が設定ファイルになっている。  
**config/app.php**
timezone => 'Asia/Tokyo'
locale => 'ja'

**config/database.php**
charset => 'utf8'

デバッグバーをインストールする。  
```
composer require barryvdh/laravel-debugbar --dev
```

デバッグ中はデバッグバーが出るので本番公開するとやばい。  
.envファイルの切り替えをわすれない。  
APP_DEBUG=true → false

**データベースの設定**
.envの「DB_**」を変更する。
.envに設定したとおりにDBやユーザーを作成したら、以下コマンドを実行。  
```
php artisan migrate
```

### キャッシュの削除
コマンドからlaravelのキャッシュを削除できる。  
```
php artisan cache:clear
php artisan config:clear
```

### artisanコマンド
artisanコマンドで色々できる。  
キャッシュのクリアだったり、簡易サーバーを立ち上げたり。  
Modelを作成するコマンドとか
```
php artisan make:model Models/Test
```

### マイグレーション
DBテーブルの履歴管理。  
テーブルを作成したり、または削除したりなどの履歴を確認できる。  
**命名規則** モデルは単数形、マイグレーションは複数形で作成する。  
Testモデル testsマイグレーション  
Personモデル peopleマイグレーション  
作成したマイグレーションは database/migrations の中に入っている。  
マイグレーションからDBにテーブルを作成できる。  
```
php artisan migrate
```
マイグレーションのロールバック  
```
php artisan migrate:rollback
```
マイグレーションの状態確認
```
php artisan migrate:status
```

### ddコマンド
die + var_dump なlaravelのヘルパ関数。  
実行すると処理を止めて、コンソール的なものを表示させる。  

### コレクション型
Laravel独自の配列を拡張した型。  
データベースからデータ取得をしたときはコレクション型になっている。  
コレクション型専用の関数が多く、メソッドチェーンで記述が可能。  
https://readouble.com/laravel/6.x/ja/collections.html  

### クエリビルダ
データベースのクエリをスラスラかける。  
https://readouble.com/laravel/6.x/ja/queries.html  

### コントローラー
artisanから作成できる。  
--resource オプションをつけて作成するとRestFulなコントローラーになる。  
```
php artisan make:controller HogeController --resource
```

## React
### プロファイラ
アプリケーションのレンダー頻度やコストを計測できる。  
メモ化などの最適化が有効な可能性のある部位を特定する手助け。  
https://gist.github.com/bvaughn/60a883af01716a03a1b3285a1029be0c  
https://ja.reactjs.org/docs/profiler.html  

### 再レンダリングの検出
why-did-you-renderを使うと、再レンダリングを検出できる。  
https://github.com/welldone-software/why-did-you-render  
classベースのコンポーネントならwhy-did-you-updateの方が良さげ  
ちなみに、useStateやuseRefを使っている場合は最初の初期化処理でコンポーネントを再実行している。（再レンダリングではない

### useCallbackとReact.memo + useMemo
https://qiita.com/Climber22/items/2c6103b4e1ef7a1f2f7c  
https://qiita.com/k_7016/items/d1e6a5eb934aaf667739  
**userCallback**
第二引数が変わらない限り、再レンダリングで関数の再定義を防ぐ。  
子コンポーネントに関数を渡していて、再レンダリングで関数が再定義されると、内容に変化がないのにムダに子コンポーネントを再レンダリングしてしまうの防ぐために使う。  
その為、子コンポーネントに渡さない関数をuseCallbackでラップする必要はない。  

**React.memo**
第一引数にコンポーネントを返す関数を渡し、第二引数にPropsが同一であるかを判断する関数を渡します。  
第二引数がtrueを返すばあい再レンダリングをしない。  
第二引数に何も渡さなかった場合はPureComponent(shallowEqual)と同等の比較  

**useMemo**
重い処理などをメモ化するために使う。  
コストの高い処理を再レンダリング時に毎回行うと重くなってしまうので、何度実行しても結果が変わらない場合はメモ化する。  
第二引数に渡した値が変更されない限り実行されない。  
