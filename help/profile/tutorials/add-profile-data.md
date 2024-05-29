---
keywords: Experience Platform；プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API；プロファイルの有効化；プロファイルの有効化
title: リアルタイム顧客プロファイルへのデータの追加
type: Tutorial
description: このチュートリアルでは、リアルタイム顧客プロファイルにデータを追加するために必要な手順の概要を説明します。
exl-id: c2df224b-bf3d-4994-aa3a-9e9f4a6a726c
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 59%

---


# へのデータの追加 [!DNL Real-Time Customer Profile]

このチュートリアルでは、にデータを追加するために必要な手順の概要を説明します [!DNL Real-Time Customer Profile].

## のスキーマを有効にする [!DNL Real-Time Customer Profile]

に取り込まれるデータ [!DNL Experience Platform] で使用 [!DNL Real-Time Customer Profile] に従わなければならない [!DNL Experience Data Model] が有効になっている（XDM）スキーマ [!DNL Profile]. スキーマをプロファイルで有効にするには、次のいずれかを実装する必要があります [!DNL XDM Individual Profile] または [!DNL XDM ExperienceEvent] クラス。

で使用するためのスキーマを有効にすることができます。 [!DNL Real-Time Customer Profile] の使用 [!DNL Schema Registry] API または [!DNL Schema Editor] ユーザーインターフェイス。 作業を開始するには、[API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)または[スキーマエディターの UI を使用したスキーマの作成](../../xdm/tutorials/create-schema-ui.md)に関するチュートリアルに従います。

## バッチインジェストを使用してデータを追加する

にアップロードされたすべてのデータ [!DNL Platform] バッチ取り込みを使用すると、個々のデータセットにアップロードされます。 このデータを使用する前に [!DNL Real-Time Customer Profile]を指定する場合は、問題のデータセットを明示的に設定する必要があります。 手順について詳しくは、[プロファイルと ID サービスのデータセットの設定](dataset-configuration.md)に関するチュートリアルを参照してください。

データセットを設定したら、データの取り込みを開始できます。様々な形式のファイルをアップロードする方法について詳しくは、「[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)」を参照してください。

## ストリーミングインジェストを使用してデータを追加する

に準拠する、ストリームで取り込まれたデータ [!DNL Profile]を有効にした XDM スキーマは、適切なレコードを自動的に追加または上書きします。 [!DNL Real-Time Customer Profile]. レコードに複数の ID が指定されている場合、または時系列データが使用されている場合は、それらの ID が ID グラフにマッピングされます。追加の設定は必要ありません。詳しくは、[ストリーミングインジェストの開発者ガイドド](../../ingestion/tutorials/streaming-record-data.md)を参照してください。

## アップロードが正常に完了したことを確認する

新しいデータセットに初めてデータをアップロードする場合、または新しい ETL やデータソースに関するプロセスの一部としてデータをアップロードする場合は、データが正しくアップロードされているかどうかを慎重に確認することをお勧めします。

[!DNL Real-Time Customer Profile] Access API を使用すると、データセットに読み込まれるバッチデータを取得できます。目的のエンティティを取得できない場合は、[!DNL Profile] でデータセットが有効になっていない可能性があります。データセットが有効になっていることを確認した後、ソースデータの形式と識別子が要件を満たしていることを確認します。

を使用してエンティティにアクセスする方法の詳細な手順 [!DNL Real-Time Customer Profile] API については、 [エンティティエンドポイントガイド](../api/entities.md)（「」とも呼ばれます）[!DNL Profile Access] API の略です。

## プロファイルストアデータの更新

場合によっては、組織のプロファイルストアのデータを更新する必要があります。 例えば、レコードを修正したり、属性値を変更したりする必要がある場合があります。これは、バッチの取り込みを通じて行うことができ、upsert タグで構成されたプロファイル対応のデータセットが必要です。属性更新用にデータセットを構成する方法について詳しくは、[プロファイルとアップサートのデータセットの有効化](../../catalog/datasets/enable-upsert.md)に関するチュートリアルを参照してください。
