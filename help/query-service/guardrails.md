---
keywords: Experience Platform；クエリ；クエリサービス；トラブルシューティング；ガードレール；ガイドライン；制限；
title: クエリサービスのガードレール
description: このドキュメントでは、クエリの使用を最適化するのに役立つ、クエリサービスデータの使用制限に関する情報を提供します。
exl-id: 1ad5dcf4-d048-49ff-97e3-07040392b65b
source-git-commit: 23c7a4590b365a49edb066567b6ebe2ac08c67e8
workflow-type: tm+mt
source-wordcount: '1168'
ht-degree: 4%

---

# クエリサービスのガードレール

ガードレールとは、Adobe Experience Platformでのデータやシステムの使用状況、パフォーマンスの最適化、エラーや予期しない結果の回避などをガイドするしきい値のことです。
このドキュメントでは、クエリサービスデータのデフォルトの使用制限を説明し、ライセンスの使用権限に関連してデータをクエリする際のシステムパフォーマンスを最適化するのに役立ちます。

>[!IMPORTANT]
>
>このガードレール ページに加えて、販売注文と対応する [ 製品説明 ](https://helpx.adobe.com/jp/legal/product-descriptions.html) でライセンスの使用権限を確認してください。

## 前提条件

このドキュメントを進める前に、主なクエリサービスの定義と機能を十分に理解しておく必要があります。 次に説明します。

* **アドホッククエリ**：クエリの結果がデータレイク上に **保存されていない** データを調査、実験、検証する `SELECT` クエリを実行します。

* **バッチクエリ**：データのク `INSERT TABLE AS SELECT` ーン、シェイプ、操作およびエンリッチメントを行うデータクエリと `CREATE TABLE AS SELECT` クエリを実行します。 これらのクエリの結果は、データレイク上で **保存され** す。 この機能の消費量を測定する指標は計算時間です。

* **クエリサービスユーザー**:Customer Journey Analytics、Adobe Real-Time Customer Data Platform、Adobe Journey Optimizerの現在のライセンスで提供されているクエリサービスユーザーも、Data Distillerで使用できます。 クエリサービスのユーザーは、機能間で共有されます。

* **アドホックユーザー**：アドホックユーザーは、アドホッククエリを実行するユーザーです。

* **バッチユーザー**：バッチユーザーは、バッチクエリを実行するユーザーです。

* **レポート API**：データ取得の呼び出しを（内部または外部で）行うための API。 拡張レポートデータモデルは、Real-Time CDP ダッシュボードデータモデルなどの、Adobe Experience Platformのネイティブなレポートデータモデルから派生します。

## ガードレール タイプ

このドキュメントでは、次の 2 種類のデフォルトの上限について説明します。

| ガードレール タイプ | 説明 |
|----------|---------|
| **パフォーマンスガードレール（ソフトリミット）** | パフォーマンスガードレールは、ユースケースのスコーピングに関連する使用制限です。 パフォーマンスガードレールを超えると、パフォーマンスの低下や待ち時間が発生する場合があります。 Adobeは、このようなパフォーマンス低下に対する責任を負いません。 パフォーマンスのガードレールを常に超えるお客様は、パフォーマンスの低下を避けるために、追加容量のライセンスを選択できます。 |
| **システムで適用されるガードレール（ハードリミット）** | システムに適用されたガードレールは、Real-Time CDP UI または API によって適用されます。 これらは、UI と API によってこれをブロックされるか、エラーを返すので、超えることはできない制限です。 |

{style="table-layout:auto"}

>[!NOTE]
>
>このドキュメントで説明するデフォルトの上限は、常に改善されています。 定期的にアップデートを確認してください。

## プライマリエンティティのパフォーマンスガードレール

次の表に、特定のクエリパターンを使用する際のクエリ実行に対して推奨されるガードレールの制限と説明を示します。

**アドホッククエリ**

| ガードレール | 上限 | 制限タイプ | 説明 |
|---|---|---|---|
| 最大実行時間 | 10 分 | システムに適用されたガードレール | これは、アドホック SQL クエリの最大出力時間を定義します。 結果を返す制限時間を超えると、エラーコード 53400 がスローされる。 |
| 同時クエリサービスユーザー | <ul><li>アプリケーションの製品説明で指定されている通り。</li><li>+5 （アドホッククエリユーザーのアドオンパックを追加購入するたびに適用）</li></ul> | システムに適用されたガードレール | これにより、特定の組織で同時にセッションを作成できるユーザーの数を定義します。 同時実行制限を超えると、`Session Limit Reached` エラーが表示されます。 |
| クエリの同時実行 | <ul><li>アプリケーションの製品説明で指定されている通り。</li><li>+1 （アドホッククエリユーザーのアドオン SKU パックを追加購入するたびに適用）</li></ul> | システムに適用されたガードレール | これにより、特定の組織に対して同時に実行できるクエリの数が定義されます。 同時実行数の上限を超えると、クエリはキューに入れられます。 |
| クライアントコネクタと結果の出力制限 | クライアントコネクタ<ul><li>クエリ UI （100 行）</li><li>サードパーティクライアント（50,000）</li><li>[!DNL PostgresSQL] クライアント （50,000）</li></ul> | システムに適用されたガードレール | クエリの結果は、次の手段で受信できます。<ul><li>クエリサービス UI</li><li>サードパーティクライアント</li><li>[!DNL PostgresSQL] クライアント</li></ul>メモ：出力数に制限を追加すると、結果がより高速に返される場合があります。 例えば、`LIMIT 5`、`LIMIT 10` など。 |
| 次を使用して結果が返される： | クライアント UI | なし | これにより、結果をユーザーが使用できるようにする方法が定義されます。 |

{style="table-layout:auto"}

**バッチクエリ**

| **ガードレール** | **制限** | **制限タイプ** | **説明** |
|---|---|---|---|
| 最大実行時間 | 24 時間 | システムに適用されたガードレール | これは、バッチ SQL クエリの最大実行時間を定義します。<br> クエリの処理時間は、処理するデータの量とクエリの複雑さに依存します。 |
| スケジュールされていないバッチの同時クエリサービスユーザー | <ul><li>アプリケーションの製品説明で指定されている通り。</li><li>+5 （アドホッククエリユーザーのアドオンパックを追加購入するたびに適用）</li></ul> | システムに適用されたガードレール | スケジュールされていないバッチクエリ（インタラクティブモードの CTAS/ITAS クエリなど）の場合、特定の組織で同時にセッションを作成できるユーザーの数を定義します。 同時実行制限を超えると、`Session Limit Reached` エラーが表示されます。 |
| スケジュール済バッチの同時問合せサービス・ユーザー | ユーザー制限なし | なし | スケジュールされたバッチクエリは非同期ジョブなので、ユーザーの制限はありません。 |
| バッチデータ処理の計算時間 | お客様のAdobe Experience Platform Intelligence の「カスタム SKU 受注の問合せ」で指定されている内容 | パフォーマンスガードレール | これは、顧客がバッチクエリを実行してデータをスキャン、処理およびデータレイクに書き戻すために許可される、1 年あたりの計算時間の範囲を定義します。 |
| クエリの同時実行 | サポートあり | なし | スケジュールされたバッチクエリは非同期ジョブなので、同時クエリがサポートされます。 |
| クライアントコネクタと結果の出力制限 | クライアントコネクタ<ul><li>クエリ UI （行の上限なし）</li><li>サードパーティクライアント （行の上限なし）</li><li>[!DNL PostgresSQL] クライアント （行の上限なし）</li><li>REST API （行の上限なし）</li></ul> | システムに適用されたガードレール | クエリの結果は、次の方法を使用して利用できます。<ul><li>派生データセットとして保存可能</li><li>既存の派生データセットに挿入できます</li></ul>注意：問合せ結果からのレコード数に上限はありません。 |
| 次を使用して結果が返される： | データセット | なし | これにより、結果をユーザーが使用できるようにする方法が定義されます。 |

{style="table-layout:auto"}

## クエリ高速化ストア {#query-accelerated-store}

次の表に、クエリ高速化ストアに対して推奨されるガードレールの制限と説明を示します。

| ガードレール | 上限 | 制限タイプ | 説明 |
|---|---|---|---|
| クエリの同時実行 | 4 | システムに適用されたガードレール | レポート API を介して集計されたデータに対するクエリ（Real-Time CDP データモデルなどのデータモデルを強化するクエリを含む）が効率的に実行できるリソースを持っていることを確認するために、レポート API は、各クエリに同時実行スロットを割り当てて、リソース使用率を追跡します。 システムはクエリをキューに入れ、同時実行スロットが使用可能になるまで、またはクエリをキャッシュから提供できるようになるまで待ちます。 常に最大 4 つの同時クエリスロットを使用できます。<br>BI ツールからレポート API にアクセスし、さらに同時実行が必要な場合は、BI サーバーが必要です。 |

{style="table-layout:auto"}

## 次の手順

このドキュメントを読むことで、使用可能なクエリパターンによるクエリ実行のデフォルト制限について、理解を深めることができました。

クエリサービスについて詳しくは、次のドキュメントを参照してください。

* [クエリサービス API](./api/getting-started.md)
* [クエリサービス UI](./ui/overview.md)

他のExperience Platform サービスのガードレール、エンドツーエンドの待ち時間の情報およびReal-Time CDP Product Description のドキュメントからのライセンス情報について詳しくは、次のドキュメントを参照してください。

* [Real-Time CDP ガードレール](/help/rtcdp/guardrails/overview.md)
* 様々なExperience Platform サービス用の [ エンドツーエンドの待ち時間の図 ](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/deployment/guardrails.html?lang=ja#end-to-end-latency-diagrams)。
* [Real-Time Customer Data Platform（B2C Edition - PrimeおよびUltimate パッケージ） ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2P - PrimeおよびUltimate パッケージ） ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2p-edition-prime-and-ultimate-packages.html)
* [Real-Time Customer Data Platform（B2B - PrimeおよびUltimate パッケージ） ](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html)