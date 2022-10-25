---
keywords: Experience Platform;ホーム;人気のトピック;
title: （ベータ版）Adobe Workfront Source
description: Adobe Workfrontは、1 か所で作業のライフサイクル全体を管理するのに役立つ、マーケティング作業管理アプリケーションです。 Workfrontには、組織での作業フローをより深く理解し、最適化するために使用できる、レポートおよび分析ツールが含まれています。
source-git-commit: 1af0863766e29c599e02f2a553d237bc62f455d2
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 3%

---

# （ベータ版）Adobe Workfrontソース

>[!NOTE]
>
>Adobe Workfrontソースはベータ版です。 詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータ版のコネクタの使用に関する詳細

Adobe Workfrontは、1 か所で作業のライフサイクル全体を管理するのに役立つ、マーケティング作業管理アプリケーションです。 Workfrontには、組織での作業フローをより深く理解し、最適化するために使用できる、レポートおよび分析ツールが含まれています。

WorkfrontとAdobe Experience Platformソースカタログの統合により、WorkfrontデータをExperience Platformに取り込んで、次のような使用例を実行できます。

* 業務レコードをサードパーティのデータと組み合わせます。
* 作業レコードに履歴分析と時系列分析を適用します。
* 次のようなサードパーティのビジネスインテリジェンスツールを使用して、業務記録にアクセス [!DNL PowerBI].
* クエリは、標準の SQL を使用して作業データを作成します。

次の作業項目とそれに対応する属性は、Workfrontソースを通じてExperience Platformに含めることができます。

* ポートフォリオ
* プログラム
* プロジェクト
* タスク
* オペレーショナルタスク（問題）
* ユーザー

Workfrontソースは、これらの属性に対するすべての新しい更新をストリーミングし、最大 1 年の履歴変更イベントをバックフィルします。 Workfrontデータを Platform データセットに含めたら、 [クエリサービス](../../../query-service/home.md) や、必要に応じて、作業に関連するデータを他のデータセットと詳細に分析または結合する他のツールを使用できます。

## UI を使用したWorkfrontの Platform への接続

Workfrontデータを Platform に取り込む方法について詳しくは、 [Workfrontデータを Platform に取り込むソース接続の作成](../../tutorials/ui/create/adobe-applications/workfront.md).