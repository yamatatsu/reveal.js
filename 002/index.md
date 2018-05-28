## 目次

- やりたかったこと
- どうやってつくろうか
- どうなったのか
- 反省といろいろ

===

## やりたかったこと

===

### twitterで技術系のつぶやきだけフィルターして読みたい🤔
### 「きかいがくしゅう」ってやつでできるのでは？👀

===

始めるきっかけにさせて頂いた記事

<a href="https://qiita.com/lamrongol/items/8c670351ef2b96d4fbb6" target="_blank"><img src="https://i.gyazo.com/d40e3ff2b1fee586284d0a4b3f459865.png" alt="https://gyazo.com/d40e3ff2b1fee586284d0a4b3f459865" width="725"/></a>

===

## SVMとは
Support Vector Machine

以下[ウィキペディア](https://ja.wikipedia.org/wiki/%E3%82%B5%E3%83%9D%E3%83%BC%E3%83%88%E3%83%99%E3%82%AF%E3%82%BF%E3%83%BC%E3%83%9E%E3%82%B7%E3%83%B3)より

===

> サポートベクターマシンは、教師あり学習を用いるパターン認識モデルの一つである。分類や回帰へ適用できる。

===

> 現在知られている手法の中でも認識性能が優れた学習モデルの一つである。サポートベクターマシンが優れた認識性能を発揮することができる理由は、未学習データに対して高い識別性能を得るための工夫があるためである。

===

> サポートベクターマシンは、線形入力素子を利用して 2 クラスのパターン識別器を構成する手法である。訓練サンプルから、各データ点との距離が最大となるマージン最大化超平面を求めるという基準（超平面分離定理）で線形入力素子のパラメータを学習する。

===

## 😇

===

### 使ってみよう
[svm (npm)](https://www.npmjs.com/package/svm)

===

```js
const { SVM } = require('svm')
const svm = new SVM()

// 「座標[0,0]は-1点、座標[1,1]は1点」っていう教師データ
svm.train(
  [[0, 0], [1, 1]],
  [-1, 1])

svm.marginOne([0,0]) // => -1
svm.marginOne([1,1]) // => 1
svm.marginOne([0.1,0.1]) // => -0.8
svm.marginOne([0.9,0.9]) // => 0.8
```

===

つぶやきフィルターの実装イメージ

===

```js
// つぶやき
const tweets: string[] = [
  '昨日は良い日だったな',
  '今日子ちゃんは今日もカワイイ',
  '今日は雨。明日は晴れるかな？',
]
```

```js
// 分かち書き
const words: string[][] = tweets.map(t => awesomeWakachi(t))
// [
//   ['昨日', '良い', '日'],
//   ['今日子', 'ちゃん', '今日', 'カワイイ'],
//   ['今日','雨','明日','晴れる'],
// ]
```

```js
// 各次元への単語の割当表
const wordIndexes: string[] = uniqConcat(words)
// ['昨日','良い','日','今日子','ちゃん','今日','カワイイ','雨','明日','晴れる'],
```

===

分かち書きとは
```js
tokenize('今日子ちゃんは今日もカワイイ')

// kuromojiの結果です
[ { word_id: 1681440,
    word_type: 'KNOWN',
    word_position: 1,
    surface_form: '今日子',
    pos: '名詞',
    pos_detail_1: '固有名詞',
    pos_detail_2: '人名',
    pos_detail_3: '名',
    conjugated_type: '*',
    conjugated_form: '*',
    basic_form: '今日子',
    reading: 'キョウコ',
    pronunciation: 'キョーコ' },
  { word_id: 86700,
    word_type: 'KNOWN',
    word_position: 4,
    surface_form: 'ちゃん',
    pos: '名詞',
    pos_detail_1: '接尾',
    pos_detail_2: '人名',
    pos_detail_3: '*',
    conjugated_type: '*',
    conjugated_form: '*',
    basic_form: 'ちゃん',
    reading: 'チャン',
    pronunciation: 'チャン' },
  { word_id: 93010,
    word_type: 'KNOWN',
    word_position: 7,
    surface_form: 'は',
    pos: '助詞',
    pos_detail_1: '係助詞',
    pos_detail_2: '*',
    pos_detail_3: '*',
    conjugated_type: '*',
    conjugated_form: '*',
    basic_form: 'は',
    reading: 'ハ',
    pronunciation: 'ワ' },
  { word_id: 126270,
    word_type: 'KNOWN',
    word_position: 8,
    surface_form: '今日',
    pos: '名詞',
    pos_detail_1: '副詞可能',
    pos_detail_2: '*',
    pos_detail_3: '*',
    conjugated_type: '*',
    conjugated_form: '*',
    basic_form: '今日',
    reading: 'キョウ',
    pronunciation: 'キョー' },
  { word_id: 93220,
    word_type: 'KNOWN',
    word_position: 10,
    surface_form: 'も',
    pos: '助詞',
    pos_detail_1: '係助詞',
    pos_detail_2: '*',
    pos_detail_3: '*',
    conjugated_type: '*',
    conjugated_form: '*',
    basic_form: 'も',
    reading: 'モ',
    pronunciation: 'モ' },
  { word_id: 230,
    word_type: 'UNKNOWN',
    word_position: 11,
    surface_form: 'カワイイ',
    pos: '名詞',
    pos_detail_1: '一般',
    pos_detail_2: '*',
    pos_detail_3: '*',
    conjugated_type: '*',
    conjugated_form: '*',
    basic_form: '*' } ]
```

===

```js
// ベクトル(次元毎のワードの出現数)
const vectors: number[][] = [
  [1,1,1,0,0,0,0,0,0,0], // 昨日は良い日だったな
  [0,0,0,1,1,1,1,0,0,0], // 今日子ちゃんは今日もカワイイ
  [0,0,0,0,0,1,0,1,1,1], // 今日は雨。明日は晴れるかな？
]
```

```js
// ラベル(関心の有り無し)
const labels: (1 | -1)[] = [
  -1,
  1,
  -1,
]
```

```js
svm.train(vectors, labels)
svm.marginOne('今日子ちゃんは雨が好き') // => ０より高いか低いか
```

===

## どうやってつくろうか

===

- 教師データ作るの大変らしい
  - スマホアプリで興味有り無しをポチポチできればスキマ時間で作れるのでは？

- なんかAWS Cloud Formationを素振りたい気分だ
  - lambda, dynamodbで機械学習できないかな
  - npmにいろいろあるぞ。[kuromoji.js](https://www.npmjs.com/package/kuromoji), [svm](https://www.npmjs.com/package/svm)

===

<a href="https://gyazo.com/6989888af0d9cac67b53a9735a5aae57"><img src="https://i.gyazo.com/6989888af0d9cac67b53a9735a5aae57.png" alt="https://gyazo.com/6989888af0d9cac67b53a9735a5aae57" width="974"/></a>

===

## どうなったのか

===

- modelはできたけどその精度試せてない。。。
- 4,5件確認したけど当たったり外れたり。

===

## 反省といろいろ

===

### 振り分けアプリは良かった
- 気づいたら1,500件くらいにラベル付けれてた
- 「ぜんぜんサボってるヤバイ。。。」って思ってたのに

===

デモ

<a href="https://gyazo.com/a70fc37a05500dd172a525ecf4f73274"><img src="https://i.gyazo.com/a70fc37a05500dd172a525ecf4f73274.png" alt="https://gyazo.com/a70fc37a05500dd172a525ecf4f73274" width="280"/></a>

===

### でも振り分けムズい
- 自分の中での「興味がある」の定義が日ごとにブレる
- 文字読んで振り分けるのは脳が疲れる。。。
  - 画像の振り分けのほうが直感的なのかも仮説
- 自然言語の教師データは手作りより生成した方が良い？
- それか学習済みモデル使うか

===

### AWS Cloud Formation はいいぞ
- コードが残る
- AWSの(ほぼ)全てを定義できる
- パターンの再利用が容易
  - 定期的にlambda実行とか
  - dynamodbのストリームでlambda実行とか
- コマンド一発で全てを捨てれる

===

lambdaの定義
```yaml
LambdaCreateModel:
  Type: AWS::Lambda::Function
  Properties:
    Code: ./dist/lambda
    Handler: bundle.createModel
    Role: !GetAtt LambdaRole.Arn
    Runtime: nodejs8.10
    Environment:
      Variables:
        BUCKET_NAME: !Ref BucketName
    MemorySize: 512
    Timeout: 300
```

===

roleの定義
```yaml
LambdaRole:
  Type: AWS::IAM::Role
  Properties:
    AssumeRolePolicyDocument:
      Statement:
        - Effect: Allow
          Action: [ "sts:AssumeRole" ]
          Principal:
            Service: [ lambda.amazonaws.com ]
    Path: /
    ManagedPolicyArns:
      - "arn:aws:iam::aws:policy/CloudWatchLogsFullAccess"
      - "arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess"
      - "arn:aws:iam::aws:policy/AmazonS3FullAccess"
```

===

lambdaを定期実行する定義
```yaml
EventRuleForFetchTweets:
  Type: AWS::Events::Rule
  Properties:
    ScheduleExpression: 'cron(0/15 * * * ? *)'
    Targets:
      - Arn: !GetAtt 'LambdaFetchTweets.Arn'
        Id: !Ref 'LambdaFetchTweets'
LambdaPermissionForEventsToInvokeLambda:
  Type: AWS::Lambda::Permission
  Properties:
    FunctionName: !Ref 'LambdaFetchTweets'
    Action: lambda:InvokeFunction
    Principal: events.amazonaws.com
    SourceArn: !GetAtt 'EventRuleForFetchTweets.Arn'
```

===

Cognitoの定義
```yaml
IdentityPool:
  Type: AWS::Cognito::IdentityPool
  Properties:
    AllowUnauthenticatedIdentities: true

RoleAttachment:
  Type: AWS::Cognito::IdentityPoolRoleAttachment
  Properties:
    IdentityPoolId: !Ref IdentityPool
    Roles:
      unauthenticated: !GetAtt 'AppRole.Arn'
```

===

### dynamodb気難しい。。
- 検索がむずい
  - 結局 `x is null` の検索できんかった。。。
  - (教えて欲しい)
- secondary index付ける毎に無料枠無料枠超えないかビビる
  - (結果全然余裕だったけど)

===

### SVMについて
- 機械学習ムツカシイ
- trainメソッドにはいろんなオプションがある
- 根本理解が必要そう

===

### jsで機械学習
- nampyの対抗として[numjs](https://www.npmjs.com/package/numjs)というのを見つけた
- でもjsは数字弱い
  - 80.7 - 10.1 => 70.60000000000001
- まぁ学習はgoogle Colaboratoryでpython使ってやれば良さそう😇
- 学習済みモデルの利用はjsでもできる
  - svm, [tensorflow.js](https://js.tensorflow.org/)

===

### lambdaで機械学習
- 5分タイムアウトに引っかかるよ！
- (知ってた。)
- 無理してlambda使う意味はないね！
- google Colaboratory, pythonでおｋ

===

### AWS利用料金
<a href="https://gyazo.com/abaaf2f7f55d3569dbb3d850a93f2087"><img src="https://i.gyazo.com/abaaf2f7f55d3569dbb3d850a93f2087.png" alt="https://gyazo.com/abaaf2f7f55d3569dbb3d850a93f2087" width="588"/></a>

===

### AWS利用料金
- dynamodb streamでlambdaがどかどか実行されたり
- lambdaのメモリサイズ結構上げたり
- lambdaの実行時間が結構伸びたり
- dynamodbのストレージサイズでドキドキしたり

いろいろあったけど結果0円！！

===

# まとめ

===

- expoらくちん
- CFnよい
- dynamodbをメインDBで使うのムズそう
- kuromoji楽しい
- 「学習済みモデル使えればいいんじゃね？」

===

## 感想
今度は学習済みデータ使ってラクして遊びたい 😇

===

## ソースコード
https://github.com/yamatatsu/twitter-filter

- CFnなので簡単に構築できます
- expoもそんなに手間なく試せると思います
- エイヤッと書いたままで汚いとこも多いかもですが。とくにアプリ

===

## ご清聴ありがとう
## ございました 🙇
