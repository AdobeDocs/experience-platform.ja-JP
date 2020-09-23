---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API;enable profile;Enable profile
title: リアルタイム顧客プロファイルへのデータの追加
topic: tutorial
description: このチュートリアルでは、リアルタイム顧客プロファイルにデータを追加するために必要な手順について説明します。
translation-type: tm+mt
source-git-commit: 59cf089a8bf7ce44e7a08b0bb1d4562f5d5104db
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 53%

---


# ～へ追加のデータ [!DNL Real-time Customer Profile]

This tutorial outlines the steps necessary to add data to [!DNL Real-time Customer Profile].

## スキーマの有効化 [!DNL Real-time Customer Profile]

によって使用 [!DNL Experience Platform] するために取り込まれるデータは、に対して有効になっている [!DNL Real-time Customer Profile] (XDM)スキーマに準拠している必要があり [!DNL Experience Data Model][!DNL Profile]ます。 In order for a schema to be enabled for Profile, it must implement either the [!DNL XDM Individual Profile] or [!DNL XDM ExperienceEvent] class.

スキーマを有効にして、 [!DNL Real-time Customer Profile] APIまたはユー [!DNL Schema Registry][!DNL Schema Editor] ザーインターフェイスの使用に使用できます。 作業を開始するには、[API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)または[スキーマエディターの UI を使用したスキーマの作成](../../xdm/tutorials/create-schema-ui.md)に関するチュートリアルに従います。

## バッチインジェストを使用してデータを追加する

All data uploaded to [!DNL Platform] using batch ingestion is uploaded to individual datasets. Before this data can be used by [!DNL Real-time Customer Profile], the dataset in question has to be specifically configured. 手順について詳しくは、[プロファイルと ID サービスのデータセットの設定](dataset-configuration.md)に関するチュートリアルを参照してください。

データセットを設定したら、データの取り込みを開始できます。様々な形式のファイルをアップロードする方法について詳しくは、「[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)」を参照してください。

## ストリーミングインジェストを使用してデータを追加する

Any stream-ingested data that is compliant with a [!DNL Profile]-enabled XDM schema will automatically add or overwrite the appropriate record in [!DNL Real-time Customer Profile]. レコードに複数の ID が指定されている場合、または時系列データが使用されている場合は、それらの ID が ID グラフにマッピングされます。追加の設定は必要ありません。詳しくは、[ストリーミングインジェストの開発者ガイドド](../../ingestion/tutorials/streaming-record-data.md)を参照してください。

## アップロードが正常に完了したことを確認する

新しいデータセットに初めてデータをアップロードする場合、または新しい ETL やデータソースに関するプロセスの一部としてデータをアップロードする場合は、データが正しくアップロードされているかどうかを慎重に確認することをお勧めします。

Using the [!DNL Real-time Customer Profile] Access API, you can retrieve batch data as it gets loaded into a dataset. If you are unable to retrieve any of the entities you expect, your dataset may not be enabled for [!DNL Profile]. データセットが有効になっていることを確認した後、ソースデータの形式と識別子が要件を満たしていることを確認します。

APIを使用してエンティティにアクセスする方法について詳しくは、「 [!DNL Real-time Customer Profile] API」とも呼ばれる [エンティティエンドポイントガイド](../api/entities.md)[!DNL Profile Access] を参照してください。