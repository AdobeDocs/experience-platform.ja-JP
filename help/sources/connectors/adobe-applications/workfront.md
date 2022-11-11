---
keywords: Experience Platform;ホーム;人気のトピック;
title: （ベータ版）Adobe Workfront ソース
description: Adobe Workfront はマーケティング作業管理アプリケーションで、作業のライフサイクル全体を一元的に管理するのに役立ちます。Workfront には、組織での作業フローをより深く理解し、最適化するために使用できる、レポートおよび分析ツールが含まれています。
source-git-commit: 1af0863766e29c599e02f2a553d237bc62f455d2
workflow-type: ht
source-wordcount: '281'
ht-degree: 100%

---

# （ベータ版）Adobe Workfront ソース

>[!NOTE]
>
>Adobe Workfront ソースはベータ版です。ベータ版のコネクタの使用に関して詳しくは、[ソースの概要](../../home.md#terms-and-conditions)を参照してください。

Adobe Workfront はマーケティング作業管理アプリケーションで、作業のライフサイクル全体を一元的に管理するのに役立ちます。Workfront には、組織での作業フローをより深く理解し、最適化するために使用できる、レポートおよび分析ツールが含まれています。

Workfront と Adobe Experience Platform ソースカタログの統合により、Workfront データを Experience Platform に取り込んで、次のようなユースケースを実行できます。

* 業務レコードをサードパーティのデータと組み合わせます。
* 作業レコードに履歴分析と時系列分析を適用します。
* [!DNL PowerBI] のようなサードパーティのビジネスインテリジェンスツールを使用して、作業レコードにアクセスします。
* 標準の SQL を使用して作業データをクエリします。

次の作業項目とそれに対応する属性は、Workfront ソースを通じて Experience Platform に含めることができます。

* ポートフォリオ
* プログラム
* プロジェクト
* タスク
* オペレーショナルタスク（問題）
* ユーザー

Workfront ソースは、これらの属性に対するすべての新しい更新をストリーミングし、最大 1 年の履歴変更イベントをバックフィルします。 Workfront データを Platform データセットに含めたら、[クエリサービス](../../../query-service/home.md)やその他のツールを利用して、必要に応じて、作業に関連するデータをさらに分析したり、他のデータセットと結合したりできます。

## UI を使用した Workfront の Platform への接続

Workfront データを Platform に取り込む方法について詳しくは、[Workfront データを Platform に取り込むソース接続の作成](../../tutorials/ui/create/adobe-applications/workfront.md)に関するガイドを参照してください。