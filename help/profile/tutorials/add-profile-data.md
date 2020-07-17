---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタ追加イム顧客プロファイルに対するデータ
topic: tutorial
translation-type: tm+mt
source-git-commit: f910351d49de9c4a18a444b99b7f102f4ce3ed5b
workflow-type: tm+mt
source-wordcount: '355'
ht-degree: 0%

---


# ～へ追加のデータ [!DNL Real-time Customer Profile]

このチュートリアルでは、にデータを追加するために必要な手順について説明 [!DNL Real-time Customer Profile]します。

## スキーマの有効化 [!DNL Real-time Customer Profile]

によって使用 [!DNL Experience Platform] するために取り込まれるデータは、に対して有効になっている [!DNL Real-time Customer Profile] (XDM)スキーマに準拠している必要があり [!DNL Experience Data Model][!DNL Profile]ます。 プロファイルを有効にするには、スキーマが [!DNL XDM Individual Profile] または [!DNL XDM ExperienceEvent] クラスを実装する必要があります。

スキーマを有効にして、 [!DNL Real-time Customer Profile] APIまたはユー [!DNL Schema Registry][!DNL Schema Editor] ザーインターフェイスの使用に使用できます。 作業を開始するには、APIを使用したスキーマの [作成](../../xdm/tutorials/create-schema-api.md) 、またはスキーマエディターUIを使用したスキーマの [作成に関するチュートリアルに従います](../../xdm/tutorials/create-schema-ui.md)。

## バッチ追加インジェストを使用したデータ

バッチ取り込みを [!DNL Platform] 使用してアップロードされたすべてのデータは、個々のデータセットにアップロードされます。 でこのデータを使用する前に、対象のデータセットを具体的に設定する必要があり [!DNL Real-time Customer Profile]ます。 詳しい手順については、プロファイルおよびIDサービス用のデータセットの [設定に関するチュートリアルを参照してください](dataset-configuration.md)。

データセットの設定が完了すると、データをデータセットに取り込む開始を設定できます。 様々な形式でファイルをアップロードする方法について詳しくは、 [バッチインジェスト開発者ガイド](../../ingestion/batch-ingestion/api-overview.md) を参照してください。

## ストリーミング取り込みを追加使用したデータ

有効なXDMスキーマに準拠しているストリームで取り込まれたデータは、 [!DNL Profile]にある適切なレコードを自動的に追加または上書きし [!DNL Real-time Customer Profile]ます。 レコードに複数のIDが指定されている場合、または時系列データが使用されている場合、追加の設定なしで、それらのIDがIDグラフにマッピングされます。 詳しくは、『 [ストリーミング取り込み開発者ガイド](../../ingestion/tutorials/streaming-record-data.md) 』を参照してください。

## アップロードが成功したことの確認

初めて新しいデータセットにデータをアップロードする場合、または新しいETLやデータソースに関連するプロセスの一部として、データが正しくアップロードされているかどうかを注意深く確認することをお勧めします。

Access APIを使用すると、バッチデータがデータセットに読み込まれるたびに取得できます。 [!DNL Real-time Customer Profile] 期待するエンティティをいずれも取得できない場合は、データセットが有効になっていない可能性があり [!DNL Profile]ます。 データセットが有効になっていることを確認したら、ソースデータの形式と識別子が期待どおりに動作することを確認します。

APIを使用してエンティティにアクセスする方法について詳しくは、「 [!DNL Real-time Customer Profile] API」とも呼ばれる [エンティティエンドポイントガイド](../api/entities.md)[!DNL Profile Access] を参照してください。