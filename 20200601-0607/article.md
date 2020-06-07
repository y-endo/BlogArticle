## SPカメラを使ったクレジットカードスキャン
まずクレジットカードの番号スキャンはブラウザの機能として提供されている。  
HTMLのform,inputタグの属性の指定でクレジットカードの読み込み機能をONにする。  
またブラウザの更新対応や安定感を求めるため、JS等のサードパーティによるクレジットカードスキャンは無しにしたい。  
https://qiita.com/okumura_daiki/items/c5c28117b999252e22ca  
http://kato.space/wp/post-830/  

### 前提条件
1. SSL通信である
2. スマホのブラウザバージョン、カメラ性能に依存
	- iOSは8以降、Chromeは厳密にどのバージョンからなのか不明

### カードの読み込み
カメラのスキャンで読み取れる情報は  

- クレジットカード番号
- 有効期限: 月
- 有効期限: 年
- クレジットカードの種類
- クレジットカードの名義(iOS13で確認)

カード番号を入力するinputに以下の属性を設定した場合に、機能するかの検証結果  

- name="addCreditCardNumber" => 成功
- name="cardNumber" => 成功
- name="cardnumber" => 成功
- class="cardNumber" => 失敗
- type="cardNumber" => 失敗
- id="cardNumber", id="creditCardNumber", id="creditCardMonth", id="creditCardYear" => 成功

ちなみにtype属性はtelにするのが無難そう。Amazonのクレジットカード入力フォームがそうしているため。  

### autocomplete属性を使ったクレジットカード情報入力
どの種類の情報の入力を期待しているか指定することで、フォーム入力欄を埋めるための自動支援をする。autocompleteに指定がなくてもidやname属性で反応することもある。  
クレジットカードをスキャンしないPCをターゲットにした対応。  
クレジットカードの入力に関するautocomplete属性の値は以下の通り。  

- cc-name
	- クレジットカードなどの支払手段に表示、または関連付けられた氏名です。一般に、氏名の各部分に分割するよりも氏名のフィールドを使う方が推奨されます。
- cc-given-name
	- クレジットカードなどの支払手段に指定された下の名前 (ファーストネーム) です。
- cc-additional-name
	- クレジットカードなどの支払手段に指定された中間名 (ミドルネーム) です。
- cc-family-name
	- クレジットカードなどの支払手段に指定された姓です。
- cc-number
	- クレジットカードや番号や、口座番号などの支払手段を識別するその他の番号です。
- cc-exp
	- 支払手段の有効期限で、ふつうは MM/YY または MM/YYYY の形です。
- cc-exp-month
	- 支払手段の有効期限の月です。
- cc-exp-year
	- 支払手段の有効期限の年です。
- cc-csc
	- 支払手段のセキュリティコードです。クレジットカードでは、カードの裏に書かれた3桁の認証番号です。
- cc-type
	- 支払手段の種類です。 (例: Visa や Master Card).

IEは非対応。

### 実装例
ここまでの情報を踏まえてクレジットカード番号を入力させるformの実装例を書く。  
```
<form action="./">
<p><label for="cardNumber">カード番号</label>: <input type="tel" name="cardNumber" size="20" maxlength="20" autocomplete="cc-number"></p>
</form>
```
size,maxlengthの20はAmazonを参考。  

### 検証結果
#### iOS13.4.1 iPhoneXs Safari実機  
番号、有効期限(月/年)、クレジットカードの種類を問題無く読み込めた。  
一度読み込むとブラウザ更新をしない限り、もう一度スキャンする事ができなかった。  
また、情報が全部読み込めていない状況(番号だけスキャン完了)でも読み込みが終了してしまう。  
なので番号だけをスキャン対象にした方が安全そう、有効期限やカードの種類は一度に読み込むことができればラッキー程度。  

#### Android9 Chrome実機
クレジットカード番号の読み込みは問題無し。  
有効期限(月/年)は読み込みが出来なかった。  
暗い場所やカードに光が当たっている状況で読み込むと、カード番号が間違ってスキャンする事が何度かあった。  

---

## WebAuthn
Webで使える生体認証。  

参考サイト  
https://techblog.yahoo.co.jp/advent-calendar-2018/webauthn/  
https://qiita.com/okamoai/items/bee403e3311cda20fe03  

対応ブラウザ  
https://caniuse.com/#feat=webauthn

---

## Angular/Akita
### 参考サイト
https://datorama.github.io/akita/  
https://netbasal.gitbook.io/akita/  
https://qiita.com/m-hamada/items/89ee8268f5205cf77e19  
https://medium.com/angular-in-depth/state-management-in-angular-using-akita-82f117d282dd  
https://hackernoon.com/lets-build-a-pizza-store-with-akita-state-management-for-angular-applications-de6ced65024d  

---

## React/Redux
### typescript-fsa-redux-thunk
typescript-fsaでredux-thunkを使うには。  
https://github.com/xdave/typescript-fsa-redux-thunk  
https://gist.github.com/yuki-ycino/28a109f6dff332d40b420d10ab741060  