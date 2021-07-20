# 情報システム設計 要求仕様書

## 卓球戦績管理システム

### 改版履歴
|日付|版数|変更内容|
|:---:|:---:|:---|
|2021/07/15|初版|初版作成|

### 用語の定義
|用語|定義|
|:---|:---|
|ユーザ|本システムを利用する利用者のこと。|


### 1. 目的
　ユーザが卓球の対戦結果を入力し、自身の卓球の戦績を蓄積することができる。また、蓄積された自身の戦績について、対戦相手を指定して検索することができる。  
 　ユーザがこれらの機能を利用することによって、自身の卓球の戦績を振り返ることが容易になり、日々の卓球の練習に生かすことができるようになることを期待する。  
　なお、本システムは個人での卓球の戦績管理を想定している。

### 2. 機能要件
#### 2-1. 新規登録機能
　ユーザが本システムを利用するために、本システムのデータベースにユーザ情報を登録する機能。  
　ユーザが新規登録ページにおいて必要事項を不足なく正しく入力し、「登録する」ボタンをクリックすることで、新規登録が完了する。入力内容に不足があった場合や正しい情報が入力されなかった場合は、エラーメッセージが表示され、再度入力が求められる。

|入力事項|型|制約|備考|
|:---|:---|:---|:---|
|メールアドレス|VARCHAR(100)|NOT NULL, UNIQUE||
|パスワード|VARCHAR(16)|NOT NULL, 半角英数字のみ, 4文字以上16文字以下|パスワード確認も行う|

#### 2-2. ログイン機能
　ユーザが新規登録時に登録した情報を用いて、本システムの2-3.以降の機能を利用するために、ログインする機能。  
　ユーザがログインページにおいて必要事項を不足なく正しく入力し、「ログイン」ボタンをクリックすることで、ログインできる。入力内容に不足があった場合や正しい情報が入力されなかった場合は、エラーメッセージが表示され、再度入力が求められる。

|入力事項|型|制約|備考|
|:---|:---|:---|:---|
|メールアドレス|VARCHAR(100)|NOT NULL, UNIQUE||
|パスワード|VARCHAR(16)|NOT NULL, 半角英数字のみ, 4文字以上16文字以下||

#### 2-3. 対戦結果入力機能
　ユーザが対戦相手との対戦結果を入力し、自身の戦績を本システムのデータベースに蓄積する機能。  
　ユーザは対戦相手との対戦結果として、「対戦の日付」「大会名」「対戦の段階」「対戦相手の名前」「自分の得点」「相手の得点」「コメント」を入力する。  
　対戦結果を不足なく正しく入力し、「送信」ボタンをクリックすることで、対戦結果をデータベースに蓄積することができる。入力内容に不足があった場合や正しい情報が入力されなかった場合は、エラーメッセージが表示され、再度入力が求められる。

|入力事項|型|制約|備考|
|:---|:---|:---|:---|
|対戦の日付|DATE|NOT NULL||
|大会名|VARCHAR(20)|NOT NULL||
|対戦の段階|VARCHAR(10)|NOT NULL|準決勝や第5試合など|
|対戦相手の名前|VARCHAR(20)|NOT NULL|本システムにおいては重複を考慮しない|
|自分の得点|INT|NOT NULL, 0以上の値||
|相手の得点|INT|NOT NULL, 0以上の値||
|コメント|VARCHAR(255)|NULL可|相手の特徴やコンディションなど|

#### 2-4. 対戦結果検索機能
　ユーザが対戦相手を指定することで、2-3.対戦結果入力機能によってデータベースに蓄積された当該の対戦相手との対戦結果を、一覧表示で確認することができる機能。  
　ユーザは対戦結果を表示したい対戦相手の名前を入力して検索する。

|入力事項|型|制約|備考|
|:---|:---|:---|:---|
|対戦相手の名前|VARCHAR(20)|NOT NULL||

#### 2-5. 対戦結果編集機能
　ユーザが2-4.対戦結果検索機能で検索した対戦結果を編集することができる機能。  
　ユーザは検索した対戦結果の中から編集したい対戦結果を指定し、2-3.対戦結果入力機能で入力した事項を書き換え、「上書き」ボタンをクリックすることで、指定した対戦結果を編集することができる。

#### 2-6. 対戦結果削除機能
　ユーザが2-4.対戦結果検索機能で検索した対戦結果を削除することができる機能。  
 　ユーザは検索した対戦結果の中から削除したい対戦結果を指定し、「削除」ボタンをクリックすることで指定した対戦結果をデータベースから削除することができる。
 
#### 2-7. ログアウト機能
　ユーザがシステムを利用後に、他者に不正に利用されないようにログアウトする機能。  
 　ログイン後の各ページ上部にある「ログアウト」ボタンをクリックすることで、ログアウトすることができる。  
 　ログアウト後は、ログインページに遷移する。

### 3. データベース設計
#### 3-1. ユーザテーブル（users）

|カラム名|説明|型|制約|備考|
|:---|:---|:---|:---|:---|
|id|ユーザテーブルの主キー|INT|PRIMARY KEY, AUTO INCREMENT||
|email|メールアドレス|VARCHAR(100)|NOT NULL, UNIQUE||
|password|パスワード|VARCHAR(16)|NOT NULL||
|created_at|作成日時|DATETIME|NOT NULL||
|updated_at|更新日時|DATETIME|NOT NULL||

#### 3-2. 対戦結果テーブル（results）

|カラム名|説明|型|制約|備考|
|:---|:---|:---|:---|:---|
|id|対戦結果テーブルの主キー|INT|PRIMARY KEY, AUTO INCREMENT||
|match_date|対戦の日付|DATE|NOT NULL||
|match_name|大会名|VARCHAR(20)|NOT NULL||
|round|対戦の段階|VARCHAR(10)|NOT NULL||
|opponent_name|対戦相手の名前|VARCHAR(20)|NOT NULL||
|my_score|自分の得点|INT|NOT NULL, 0以上の値||
|opponent_score|相手の得点|INT|NOT NULL, 0以上の値||
|comment|コメント|VARCHAR(255)|NULL可||
|created_at|作成日時|DATETIME|NOT NULL||
|updated_at|更新日時|DATETIME|NOT NULL||
|user_id|ユーザテーブルのid|INT|NOT NULL|外部キー|