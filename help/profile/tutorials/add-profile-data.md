---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API、プロファイルの有効化、プロファイルの有効化
title: リアルタイム顧客プロファイルへのデータの追加
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、リアルタイム顧客プロファイルにデータを追加するために必要な手順について説明します。
exl-id: c2df224b-bf3d-4994-aa3a-9e9f4a6a726c
source-git-commit: 3b34cf37182ae98545651a7b54f586df7d811f34
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 42%

---


# [!DNL Real-time Customer Profile] へのデータの追加

このチュートリアルでは、[!DNL Real-time Customer Profile] にデータを追加するために必要な手順について説明します。

## [!DNL Real-time Customer Profile] のスキーマを有効にする

[!DNL Real-time Customer Profile] で使用するために [!DNL Experience Platform] に取り込むデータは、[!DNL Profile] に対して有効になっている [!DNL Experience Data Model] (XDM) スキーマに準拠している必要があります。 スキーマをプロファイルに対して有効にするには、[!DNL XDM Individual Profile] クラスまたは [!DNL XDM ExperienceEvent] クラスを実装する必要があります。

[!DNL Schema Registry] API または [!DNL Schema Editor] ユーザーインターフェイスを使用して、[!DNL Real-time Customer Profile] で使用するスキーマを有効にできます。 作業を開始するには、[API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)または[スキーマエディターの UI を使用したスキーマの作成](../../xdm/tutorials/create-schema-ui.md)に関するチュートリアルに従います。

## バッチインジェストを使用してデータを追加する

バッチ取得を使用して [!DNL Platform] にアップロードされたすべてのデータは、個々のデータセットにアップロードされます。 [!DNL Real-time Customer Profile] でこのデータを使用する前に、対象のデータセットを具体的に設定する必要があります。 手順について詳しくは、[プロファイルと ID サービスのデータセットの設定](dataset-configuration.md)に関するチュートリアルを参照してください。

データセットを設定したら、データの取り込みを開始できます。様々な形式のファイルをアップロードする方法について詳しくは、「[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)」を参照してください。

## ストリーミングインジェストを使用してデータを追加する

[!DNL Profile] 対応の XDM スキーマに準拠しているストリームで取り込まれたデータは、[!DNL Real-time Customer Profile] の適切なレコードを自動的に追加または上書きします。 レコードに複数の ID が指定されている場合、または時系列データが使用されている場合は、それらの ID が ID グラフにマッピングされます。追加の設定は必要ありません。詳しくは、[ストリーミングインジェストの開発者ガイドド](../../ingestion/tutorials/streaming-record-data.md)を参照してください。

## アップロードが正常に完了したことを確認する

新しいデータセットに初めてデータをアップロードする場合、または新しい ETL やデータソースに関するプロセスの一部としてデータをアップロードする場合は、データが正しくアップロードされているかどうかを慎重に確認することをお勧めします。

[!DNL Real-time Customer Profile] Access API を使用すると、データセットに読み込まれるバッチデータを取得できます。 期待するエンティティを取得できない場合は、[!DNL Profile] に対してデータセットが有効になっていない可能性があります。 データセットが有効になっていることを確認した後、ソースデータの形式と識別子が要件を満たしていることを確認します。

[!DNL Real-time Customer Profile] API を使用してエンティティにアクセスする方法について詳しくは、『[ エンティティエンドポイントガイド ](../api/entities.md)』（「[!DNL Profile Access] API」とも呼ばれます）を参照してください。

## プロファイルストアデータの更新

組織のプロファイルストアのデータを更新する必要が生じる場合があります。 例えば、レコードの修正や属性値の変更が必要になる場合があります。 バッチまたはストリーミングの取り込みによって実行でき、upsert タグで設定されたプロファイル対応のデータセットが必要です。 属性の更新用にデータセットを設定する方法の詳細については、[ プロファイルのデータセットの有効化と ](../../catalog/datasets/enable-upsert.md) のアップサートに関するチュートリアルを参照してください。
