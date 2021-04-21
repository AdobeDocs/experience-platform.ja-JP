---
keywords: Experience Platform;プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API;プロファイルを有効にする；プロファイルを有効にする
title: リアルタ追加イム顧客プロファイルに対するデータ
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、リアルタイム顧客プロファイルにデータを追加するために必要な手順について説明します。
exl-id: c2df224b-bf3d-4994-aa3a-9e9f4a6a726c
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 50%

---

# &lt;a0/追加>へのデータ[!DNL Real-time Customer Profile]

このチュートリアルでは、[!DNL Real-time Customer Profile]にデータを追加するために必要な手順について説明します。

## [!DNL Real-time Customer Profile]のスキーマを有効にする

[!DNL Real-time Customer Profile]が使用するために[!DNL Experience Platform]に取り込むデータは、[!DNL Profile]に対して有効になる[!DNL Experience Data Model] (XDM)スキーマに準拠している必要があります。 スキーマがプロファイルを有効にするには、[!DNL XDM Individual Profile]または[!DNL XDM ExperienceEvent]クラスを実装する必要があります。

[!DNL Schema Registry] APIまたは[!DNL Schema Editor]ユーザーインターフェイスを使用して、[!DNL Real-time Customer Profile]でのスキーマの使用を有効にできます。 作業を開始するには、[API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)または[スキーマエディターの UI を使用したスキーマの作成](../../xdm/tutorials/create-schema-ui.md)に関するチュートリアルに従います。

## バッチインジェストを使用してデータを追加する

バッチインジェストを使用して[!DNL Platform]にアップロードされたすべてのデータは、個々のデータセットにアップロードされます。 [!DNL Real-time Customer Profile]がこのデータを使用する前に、対象のデータセットを具体的に設定する必要があります。 手順について詳しくは、[プロファイルと ID サービスのデータセットの設定](dataset-configuration.md)に関するチュートリアルを参照してください。

データセットを設定したら、データの取り込みを開始できます。様々な形式のファイルをアップロードする方法について詳しくは、「[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)」を参照してください。

## ストリーミングインジェストを使用してデータを追加する

[!DNL Profile]対応のXDMスキーマに準拠しているストリームで取り込まれたデータは、[!DNL Real-time Customer Profile]の適切なレコードを自動的に追加または上書きします。 レコードに複数の ID が指定されている場合、または時系列データが使用されている場合は、それらの ID が ID グラフにマッピングされます。追加の設定は必要ありません。詳しくは、[ストリーミングインジェストの開発者ガイドド](../../ingestion/tutorials/streaming-record-data.md)を参照してください。

## アップロードが正常に完了したことを確認する

新しいデータセットに初めてデータをアップロードする場合、または新しい ETL やデータソースに関するプロセスの一部としてデータをアップロードする場合は、データが正しくアップロードされているかどうかを慎重に確認することをお勧めします。

[!DNL Real-time Customer Profile]アクセスAPIを使用すると、バッチデータがデータセットに読み込まれるのと同時に取得できます。 期待するエンティティをいずれも取得できない場合は、[!DNL Profile]に対してデータセットが有効になっていない可能性があります。 データセットが有効になっていることを確認した後、ソースデータの形式と識別子が要件を満たしていることを確認します。

[!DNL Real-time Customer Profile] APIを使用してエンティティにアクセスする方法について詳しくは、[エンティティエンドポイントガイド](../api/entities.md)（「[!DNL Profile Access] API」とも呼ばれます）を参照してください。
