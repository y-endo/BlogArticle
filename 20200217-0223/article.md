## Socket.io
Node製 socket.io サーバーとの通信のやり取りでチャットとか作るのに使う。  
使い方はEventEmitterに似てる。  
```
// Server
const app = require('express')();
const server = require('http').Server(app);
const io = require('socket.io')(server);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  console.log('a user connected');
  socket.on('eventName', arguments);
  io.emit('eventName', arguments);
});

server.listen(3000, function(){
  console.log('listening on *:3000');
});

// HTML
<script src="/socket.io/socket.io.js"></script>
<script>
	const socket = io();
  
  socket.emit('eventName', arguments);
  socket.on('eventName', arguments);
</script>
```

## PHP
### 変数と定数
変数は$を最初につける。  
定数は2パターンあり、constで宣言するものとdefineで宣言するものがある。  
constはPHP5.3以降でしか使えない。5.2以下はdefineしか使えない  
```
$name = 'Yuki';
const NAME = 'Yuki';
define('NAME', 'Yuki')
```
### POSTデータの受取
受け取ったデータは「スーパーグローバル」と呼ばれる変数に代入される。  
$_POST, $_GET $_SESSION, $_FILES などがある。  
```
<input type="text" name="user">
↓
$_POST['user']
```

### 受け取ったデータの存在を確認する empty関数
empty関数で変数の存在を確認する。  
```
if (empty($_POST['user'])) {
	// $_POST['user']は空
}
```

### str_replaceとpreg_replaceの違い
str_replace() 正規表現のような技巧的な置換を必要としない場合はpreg_replace() の代わりにこの関数を常用するべき  
https://cpoint-lab.co.jp/article/201806/3400/  

### ファイルの書き込み
fopen関数でファイルを開く。  
fwrite関数で書き込む。  
fopenの第一引数にファイルのパス、第二引数にモードを指定。  
モード

- r: 読み込みのみ
- w: 書き込み(ファイルをリセットしてから書き込み)
- a: 書き込み(ファイルの末端から追記する)

ファイルが存在しない場合は新規で作成される。  

```
if ($file_handle = fopen('ファイルパス', '[モード]')) {
	// 書き込み
	fwrite($file_handle, 'データ');
	// ファイルを閉じる
	fclose($file_handle);
}
```

### ファイルの読み込み
fgets関数を使う。  
データを1行ずつ取得する関数で、実行するたびにファイルポインターリソースの位置が更新されるので、繰り返し実行することで最後までデータを見に行ける。  
```
if ($file_handle = fopen('ファイルパス', 'r')) {
	// 1行ずつ読み込み
	while($data = $fgets($file_handle)) {
		// データ
	}
	// ファイルを閉じる
	fclose($file_handle);
}
```

### 配列の末尾に要素を追加する書き方
```
$arr = array();
$arr[] = 'hoge'; // この書き方は配列の末尾に追加される
```

### お手軽サニタイズ
htmlspecialchars関数。  
特殊文字をHTMLエンティティへ変換を行う。  
https://www.php.net/manual/ja/function.htmlspecialchars.php  
```
$clean = htmlspecialchars($string, $flags, 'UTF-8');
```

### データベースの接続
PDO（PHP Data Objects）を使えば簡単にMySQLなどに接続できる。  
MySQL以外のデータベースと接続するときも同じメソッドが使えて便利。  
https://qiita.com/mpyw/items/d52351bd1a8068344cc2  
https://qiita.com/mpyw/items/b00b72c5c95aac573b71  
https://qiita.com/sunnyG/items/dd981a5ee4487cedf02f  

### Dockerで環境構築した時にPDOでMySQLに接続できなかったら
ドライバがない場合と、接続先が間違っている場合がある。  
https://qiita.com/tarch710/items/1236a23f7ffde4c512f2  
https://chariosan.com/2019/04/14/lamp_docker_pdoerror/  


## その他
- Androidアプリ開発の教科書10章
  - データベースの使い方
  - https://github.com/y-endo/learn-android-app