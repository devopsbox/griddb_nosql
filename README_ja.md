## 概要
  GridDBは、時系列で蓄積されるセンサーデータに適したKVS(Key-Value Store)型のデータモデルを持ち、センサの数に応じて容易にスケールアウトすることができるデータベースです。

  * 高信頼性  
    ノード間でキー・バリューデータの複製を互いのノードで持ち合う仕組みを備えており、万が一ノード障害が発生しても、他ノードの複製を使うことにより、数秒で自動フェイルオーバーが可能です。

  * 高速性(インメモリ)  
   RDBではメモリを大容量化しても、バッファ管理等に大きなオーバヘッドが発生するため本質的なデータ処理にCPUリソースの10%前後しか割当てられずCPUパワーを十分発揮できないことが知られています。GridDB/NoSQLは、大容量化されたメモリを前提に、バッファ処理の軽量化、リカバリ処理の軽量化、データ処理時のロックフリー化を行うことで、これまでのオーバヘッドを最小化します。

  * 高度なデータモデルと操作モデル  
    従来の分散KVSでは、Put/Get/Removeという操作によりデータを操作します。GridDB/NoSQLは、これを大幅に拡張し、構造化データの定義機能、SQLライクなクエリ機能、トランザクション機能、JavaのAPI(Application Programming Interface)をサポートしており、RDBユーザがスムーズに導入できるようになっています。キー・バリューをキー・コンテナと呼ぶレコードの集合体でデータを表現します。これはRDBのテーブル名とテーブルの関係に類似しています。また、センサーデータ管理向けの応用機能も備わっています。

##クイックスタート
### サーバのビルド
    CentOS6.7及びgccのバージョン4.4.7の環境での動作を確認しています。

    $ ./bootstrap.sh
    $ ./configure
    $ make 
    
### サーバの起動
    $ export GS_HOME=$PWD
    $ export GS_LOG=$PWD/log

    $ bin/gs_passwd admin
      #input your_password
    $ vi conf/gs_cluster.json
      #    "clusterName":"your_clustername" #<-- input your_clustername
    $ export no_proxy=127.0.0.1
    $ bin/gs_startnode
    $ bin/gs_joincluster -c your_clustername -u admin/your_password
    
### クライアント(Java)のビルド
    $ ant -f java_client/build.xml

### サンプルプログラムの実行
    $ export CLASSPATH=${CLASSPATH}:$GS_HOME/bin/gridstore.jar
    $ mkdir gsSample
    $ cp $GS_HOME/docs/sample/program/Sample1.java gsSample/.
    $ javac gsSample/Sample1.java
    $ java gsSample/Sample1 239.0.0.1 31999 your_clustername admin your_password
      --> Person:  name=name02 status=false count=2 lob=[65, 66, 67, 68, 69, 70, 71, 72, 73, 74]

## ドキュメント
  docsフォルダに以下のドキュメントがあります。

  * クラスタ設計書(GridDBTechnicalDesignDocument_ja.pdf)
  * スタートアップガイド(manual/GridDB_QuickStartGuide_ja.html)
  * APIリファレンス(manual/GridDB_API_Reference_ja.html)
  * RPMインストールガイド(manual/GridDB_RPM_InstallGuide.html)

## コミュニティ
  * Issues  
    質問、不具合報告はissue機能をご利用ください。
  * PullRequest  
    GridDB Contributor License Agreement(CLA_rev1.1.pdf)に同意して頂く必要があります。
    PullRequest機能をご利用の場合はGridDB Contributor License Agreementに同意したものとみなします。

## ライセンス
  サーバソースのライセンスはGNU Affero General Public License (AGPL)、
  クライアントライブラリと運用コマンドのライセンスはApache License, version 2.0です。
  サードパーティのソースとライセンスについては3rd_party.mdを参照ください。
