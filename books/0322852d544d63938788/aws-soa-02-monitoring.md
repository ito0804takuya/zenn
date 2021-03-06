---
title: "AWS SOA モニタリングとレポート / その他"
emoji: "👀"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["AWS","SOA"]
published: false
---

# 分野 1: モニタリングとレポート 22%

## Cloudwatch (重要)
  - データポイント
    デフォルトは、5分間隔。**詳細モニタリング**を有効にした場合、1分間隔。
  - グラフ
    メトリクスの最大2週間分の履歴データから、想定値のモデルを計算でき、この想定値の範囲をバンドとしてグラフに表示します。これらのグラフのURL は保存または共有でき、モニタリングによって発見した異常などは上司に共有することが可能。
  - Cloudwatchに送信するときは、名前空間の指定が必須。
### サービスヘルスダッシュボード
  各AWSのサービスの全体的なステータスを確認するのに適しています。
  （EC2インスタンスのリタイアメント通知の自動設定には利用されません。）
### クロスアカウントクロスリージョンダッシュボード
  複数アカウントおよび複数リージョン***全体の**パフォーマンスおよび運用データを可視化、集計*する。
  ##### ＜注意＞
  リージョン間のデータを収集して(ばらばらに)表示させることは可能だが、**リージョン間のデータの集計はできない**。
### 標準メトリクス
以下は標準メトリクスで監視できる。
- EC2インスタンスのディスクオペレーション数
- ELBとEC2間の 正常に確立されなかった接続数
- S3バケットへのHTTPリクエストの数
### カスタムメトリクス
**OSレベル**の問題チェックなど、標準メトリクスでは収集できないメトリクス。
**CloudWatchエージェント**を使用してメトリクス取得を設定することが必要。
AWS CLIからメトリクス(=監視対象)のモニタリング設定のスクリプトを記述して、そのメトリクスをCloudWatchにプッシュしてモニタリングを開始できる。
  - デフォルトではEC2インスタンスの**メモリ使用率**メトリクスが取得できないため、ユーザーがカスタムメトリクスを設定する必要がある。
  - [EC2 インスタンス]の通常のメトリクスはCloudWatchに自動で取得されていますが、[EC2 Linux インスタンス]のOSのメトリクスとパフォーマンスをモニタリングするためにはCloudWatch上にカスタムメトリクスを取得。
### CloudWatch エージェント
インストールすると、EC2インスタンスなどのシステム内部メトリクスとログファイルの両方を収集できる。
手順1. インスタンスにエージェントをインストール
手順2. CloudWatchへのアクセス許可をもつIAMロールをインスタンスにアタッチ


## Cloudwatch Logs
logs特定のフレーズ、値、またはパターンを⾒つけるためにログをリアルタイムでモニタリング。
**VPCフローログ**や**CloudTrailのログファイル**は、Cloudwatch Logsに連携することで、CloudWatchでモニタリングできる。
### CloudWatch Logsを用いてログ収集を行う手段
1. CloudWatch Logs エージェントを利用する。
  EC2で稼働しているログをCloudWatchに送りたい場合、**CloudWatch LogsエージェントをEC2にインストールし、設定ファイルを変更/エージェントを起動することでCloudWatch Logsへログを送る**ことができます。
2. 各サービスでログを有効化する。
  **CloudWatch Logsをインストールできない、PaaSやSaaS（例えばLambdaやRDSなど）は、コンソール画面でロギングを有効にすると、自動的にロググループが作成され、ログの収集が開始**されます。


## Cloudwatch Events
### ユースケース
- SNS通知をトリガー(実行)できます。
- EC2 インスタンスのリタイアメント通知に応じて、自動でEC2インスタンスを停止する。
  EC2インスタンスのリタイア
  →AWS Healthがキャッチ
  →CloudWatch Eventsを起動し、通知
  →通知をトリガーとして、LambdaでEC2インスタンスを停止する関数を実行
  →EC2インスタンス停止


## CloudWatchアラーム
特定のメトリクスのしきい値に応じたアラーム通知や自動アクションを実行。
- 既存のアラームの名前は変更できない。
- 複数のメトリクスを監視する(1つの)アラームは作成できない。
- アラーム履歴の保持期間は2週間。
### 連携できるサービス
以下のサービスと直接連携することができます。
- **SNS**
- **AWS Chatbot** (slackへ通知 と言われたらコレ)
- **EC2**(再起動など)
- **AutoScaling**
### ユースケース
- サーバの自動的な増減 (**AutoScaling**)
  EC2のCPU等が高騰しているときに、CloudWatch AlarmとAutoScalingを開始することが可能。
  これによって、一時的に高負荷な状態になった場合、AutoScalingの機能によりサーバの台数を増やす。
- サーバの自動的な再起動 (**EC2**)
  EC2が突然応答しなくなった場合など、強制的に再起動やインスタンスの終了を行うことができます。
  これにより、EC2が動かないなどの障害が発生しても、自動的に復旧できるため、復旧作業が不要になります。
- SNSでrootアカウントでのログインを検知する。 (**CloudTrail, SNS**)
  CloudWatchと、**CloudTrail、SNSを連携**させることでログインのモニタリングが可能。
  しがたって、rootログインが行われた際にCloudWatch Alarmを利用してアカウントの持ち主にメールを送付する、といったセキュリティを強固にする使い方が可能。
- 例：モニタリング値を閾値にアラームを設定して、担当者に周知する。 (**SNS**)
（CloudWatchアラーム → SNS → メール通知）
- SNSへ通知した内容をSlackに通知する (**SNS, AWS Chatbot**)
  AWS Chatbotを利用して、CloudWatch Alarmの内容をSlackへ通知する事が可能となりました。

-----

## Cloudtrail
**ユーザの証跡**ログをとる。
**AWSマネジメントコンソールでの操作とAWS APIコール(データの追加、更新、削除など)を記録**。
(**API呼び出しの関する調査・記録**の問題はcloudtrailが関連する可能性大。)
IAMユーザーやロールからのアクセスやAPIアクティビティの監視 はできるが、外部ユーザーからのクロスアカウントアクセスなどの監視 はできない。(→IAM Access Analyzer を使う。)

- デフォルトでCloudTrailのログファイルは、S3サーバーサイド暗号化（SSE）を使用して暗号化されている。
### ログの保存先
  基本はS3。
  Cloudwatch Logsに保存もできる。（S3より割高）
### ログファイルの整合性の検証
  有効にすることで、CloudTrailによる**ログ作成後に、そのログファイルが変更、削除、または変更されなかったかどうか検証可能**になる。
### ログの調査
- S3に保存している場合
  基本的にはAWS Athena。（最も簡単）
  高度な検索をしたい場合は、ElasticSearchService。
- Cloudwatch Logsに保存している場合
  Cloudwatch Logs Insights
### 「組織の証跡」設定
メンバーアカウントに対するCloudTrailによるログ設定が実施される。

-----

## Config
**AWSリソースの構成情報、変更履歴**を記録。（例：未承認の AMI から起動された EC2インスタンスを検知する）
ConfigにSNSを連携すれば、SNSを利用した通知設定が可能。（例：リソースが更新されるとメール通知が実施されて、変更内容が表示できる。）
### Config Rules
ルールに沿った構成変更が行われているかを評価。
- マネージドルール
  AWSで定義・提供している汎用性の高いベーシックルール。
- カスタムルール
  Lambdaをベースとして自分で作成するルール。
#### トリガータイプ
ルール評価実行のタイミング
- 設定変更時
- 定期的

-----

## EC2Rescue for Windows Server
EC2 Windows Serverインスタンス上で動作し、潜在的な問題の診断とトラブルシューティングを行うツール。ログファイルを収集して問題を解決するだけでなく、問題がありそうな部分をプロアクティブに検索することができます。

### AWS Health
AWSのリソース、サービス、およびアカウントの状態をリアルタイムで可視化。

-----

# その他

## X-Ray
アプリケーションが処理する**リクエスト**に関するデータを収集するサービス。
本番環境や分散アプリケーション (マイクロサービスアーキテクチャを使用して構築されたアプリケーションなど) を分析およびデバッグできる。
- リクエスト実行状況の確認ができる
  アプリケーションの実行状況をエンドツーエンドで確認できる。 
- アプリケーションの問題検出ができる
  X-Ray のトレース機能を使ってリクエストのパスをたどると、パフォーマンスの問題の原因と、問題に関係するアプリケーション内の場所を特定できる。
- アプリケーションのパフォーマンスを向上できる
  アプリケーションのパフォーマンス上のボトルネックを特定できます。X-Ray のサービスマップにより、アプリケーション内のサービスやリソースの関係をリアルタイムで表示できる。高いレイテンシーが発生している場所を簡単に検出し、サービスのノードとエッジのレイテンシーのディストリビューションを視覚化し、アプリケーションのパフォーマンスに影響を与える特定のサービスやパスをドリルダウンすることができる。

-----

## Amazon CloudSearch
AWSで完全に管理された検索サービス。高速で非常に拡張性の高い検索機能をアプリケーションに容易に統合できる。

-----

## RDS
- 保管時のRDS DBインスタンスとスナップショットを暗号化するには、**DBインスタンス作成時に**RDS DBインスタンスの暗号化オプションを有効にする。
- インスタンス作成時、**RDS側でSSL証明書が作成され**、DBインスタンスにインストールされる。
- メンテナンスウィンドウ
  RDSのメンテナンスを定期的に実行。
  OSの更新、DBバージョンの更新、サーバーに適用されたパッチの更新が実施される。
- RDS OracleではRDSのリードレプリカ機能ではなく、**Oracle Data Guard**を使用する必要がある。
- マルチAZでは、冗長性が上がるが、パフォーマンスは向上しない。
- トランザクションログの設定はユーザー側で指定できません。基本的に5分ごとに取得。
### メモリ消費を確認する方法
1. FreeableMemory と SwapUsage の両方の Amazon CloudWatch メトリクスを調べて、DBインスタンスの全体的なメモリ使用パターンを把握。
  スワップメモリをモニタリングするには、拡張モニタリングを有効にして、1 秒間隔でメトリクスを確認できる。
2. Performance Insightsを有効にする
  データベースのパフォーマンスに関する問題のトラブルシューティングに役立つ。
  Performance Insights を有効にして SQL を識別し、DBインスタンスで過度のスワップやメモリを消費しているイベントを確認できる。
### RDSコンソールからモニタリングできる項目
DB インスタンスへの**接続の数**
DB インスタンスへの**読み書きオペレーションの量**
DB インスタンスが現在利用している**ストレージの量**
DB インスタンスのために利用される**メモリおよびCPUの量**
DB インスタンスとの間で送受信される**ネットワークトラフィックの量**

## Aurora
- **Aurora Auto Scaling**を使用できる。（リードレプリカを自動的にプロビジョニング）
- マルチAZ構成はない。

## DynamoDB
- Auto Scalingできる。（パフォーマンス向上）
- グローバルテーブル
  **別のリージョン**のユーザーがDynamoDBテーブルのデータに低レイテンシーでアクセスできる。

## Amazon Redshift
データウェアハウス・BI（ビジネスインテリジェンス）のサービス。
- 拡張VPC ルーティング
  クラスターとデータリポジトリ間のすべてのCOPYとUNLOADトラフィックがVPCを通るよう強制できる。
- 監査ログと STL テーブルは、どのユーザーがいつログインしたかなど、データベースレベルのアクティビティを記録。これらのテーブルには、ユーザーが実行した SQL アクティビティとその時期も記録されている。
  - 監査ログ
    S3バケットに保存される。
  - STLテーブル
    クラスターのすべてのノードに保存される。
- クロスリージョンスナップショット
  RedshiftはRDSとは異なり、シングルAZ構成しか実行できなく、マルチAZ構成によるフェールオーバー機能がないため冗長性が低い。**スナップショットを利用してローカルおよびリモートの両リージョンでデータをセキュアにバックアップすることが最良**のソリューション。
  （マルチモードにするとクラスタのノード数を設定できます。複数ノードが利用可能になることで、単一ノード障害への耐久性が向上しますが、複数AZや複数リージョンの構成ができません。）
- Redshiftの**スナップショット低減によるコスト削減**
  Redshiftは、自動スナップショットで定期的にスナップショットを取得しているため、スナップショット量がかさむことで保存コストが増加している可能性がある。

-----

## EC2
- EC2 スポットブロック
  スポットインスタンスを**1~6時間継続して利用できることが担保**された入札形式。
- メタデータを表示するURI
  http://169.254.169.254/latest/meta-data/ 
### AMIを別リージョンで共有
  マネジメントコンソールまたはAWS CLIで、AMIを指定のリージョンにコピーする必要がある。
### インスタンスタイプ
#### 種類
- EBS最適化インスタンス
  EBS I/Oとその他のインスタンスからのトラフィック間の競合を最小化することで、EBS ボリュームの高パフォーマンスを提供。
  (そうでないと、EBSとEC2間の接続がパフォーマンスのボトルネックになる可能性がある。) 
- メモリ最適化インスタンス（r5, r6, x1 等）
  メモリ内の大きいデータセットを処理するワークロードに対して高速。
- コンピューティング最適化インスタンス（c5, c6 等）
  高パフォーマンスプロセッサの恩恵を受けるコンピューティングバウンドなアプリケーションに最適。
- 高速コンピューティング（p3/p4, g3/g4, f1 等）
  機械学習。
- ストレージ最適化インスタンス（h1）
  ビックデータ解析。
#### インスタンスタイプの変更手順
  一旦インスタンスを停止した上で、異なるインスタンスタイプに変更して、インスタンスを再び起動。
### リザーブドインスタンス
#### スタンダード
下記のみ、変更可能。
・アベイラビリティーゾーンまたは適用範囲
・ネットワークプラットフォーム (EC2-Classic または VPC)
・インスタンスサイズ（Linuxのみ）
#### コンバーティブル
属性が変更できる(同等な価格のリザーブドインスタンスに交換できる)。
### 拡張ネットワーキング
高い帯域幅、1秒あたりのパケットの高いパフォーマンス、常に低いインスタンス間レイテンシーを実現。これを有効化することでネットワークパフォーマンスを向上できます。
### プレイスメントグループ
以下のような、インスタンス間における低レイテンシな通信実現するための機能
#### クラスタプレイスメントグループ
  単一AZ内のインスタンスを論理的にグループ化したもの
- **EC2間で低ネットワークレイテンシー、高ネットワークスループット**が発生する場合に最適
- 複数のAZにまたがることはできない
#### パーティションプレイスメントグループ
  論理的なパーティションに分散されているインスタンスのグループ
- 大規模なワークロードに最適（HDFS、HBase、Cassandra）
- 複数のAZに分散させることが可能
#### スプレッドプレイスメントグループ
  それぞれ異なるハードウェアに配置されるインスタンスのグループ
- 少数のEC2が互いに分離して保持される必要があるアプリケーションに最適
- 複数のAZにまたがることが可能
### トラブルシューティング
#### インスタンスを再起動後、すぐに保留状態から終了状態に移行してしまうトラブル
  ストレージが原因。
  1. EBSの容量制限超過
  2. EBSスナップショットの破損
  3. EBSボリュームが暗号化されているが、(復号化するための)KMSキーにアクセスする権限がない
  4. インスタンスを起動するために使用したインスタンスストアバックアップAMIに必要な部分（image.part.xxファイル）がない
#### InsufficientInstanceCapacity(不十分なインスタンス容量)エラー
  InsufficientInstanceCapacityエラーは、現在、リクエストを完了するための十分なオンデマンド容量がないことを示す。
  - 数分待つ。
  - インスタンスの数を減らす。
  - オンデマンド容量予約を作成。
  - 長期の容量予約であるリザーブドインスタンスを購入。
#### CPUクレジットがゼロになり、インスタンス処理が停止したとき
  - 無制限にバースト可能なインスタンスを使い、高いCPUパフォーマンスを維持する。（T3Unlimited や T2Unlimitedなど。t2やt3はバースト可能。）
  - インスタンスサイズを大きなものにスケールアップする。
#### ネットワークパフォーマンスが悪いとき（向上させる方法）
  - より大きなEC2インスタンスサイズを使用する。
  - 拡張ネットワークを実行。（拡張ネットワークをサポートしていないインスタンスタイプがある。m1.small 等）

-----

## AWS Server Migration Connector
## AWS Server Migration Service (SMS)
VMwareを利用した仮想サーバーを Amazon EC2に移行するために利用。
- VMware環境で AWS Server Migration Connector をセットアップ。(移行の準備)
- AWS SMS アクセス許可とロールの設定 を既に完了していることを前提として、SMSを利用した移行プロセスを実行。
  - **VM Importでなく、SMSを使用することがAWSでは推奨されています**。（置き換えできる要件のとき）

-----

## AWSサポート
人的なAWSのサポートサービス。
| プラン種類 | ベーシック | 開発者 | ビジネス | エンタープライズ |
| --- | --- | --- | --- | --- |
| 問合せ方法 | インスタンスのヘルスチェック不合格時のみ | メール | 電話 | 電話, テクニカルアカウントマネージャへの直接問合せ |
