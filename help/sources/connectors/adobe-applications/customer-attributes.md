---
keywords: Experience Platform；ホーム；人気のトピック；Customer Attributes connector
solution: Experience Platform
title: 顧客属性Source コネクタの概要
description: API またはユーザーインターフェイスを使用して顧客属性をAdobe Experience Platformに接続する方法について説明します
exl-id: 63765ecd-ddb5-4992-a3de-d53f054bfb28
source-git-commit: 5b37b51308dc2097c05b0e763293467eb12a2f21
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 19%

---

# 顧客属性コネクタ

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Cloud内の [[!DNL Customer Attributes]](https://experienceleague.adobe.com/docs/core-services/interface/services/customer-attributes/attributes.html) を使用すると、顧客関係管理（CRM）データベースから取り込んだ大規模法人データをアップロードできます。 データを Experience Cloud の顧客属性データソースにアップロードすると、そのデータを Adobe Analytics および Adobe Target で使用できます。

Experience Platformは、プロファイルデータをAdobe Experience Platformに取り込む [!DNL Customer Attributes] めのサポートを提供しています。

## データセットとスキーマ

[!DNL Customer Attributes] ソースは、データを取得するデータセットを自動的に作成します。 この自動作成されたデータセットは固定されており、手動で選択することはできません。 また、ソースは、入力データソースに基づいて、データセットのスキーマを自動作成します。 このプロセスには、スキーマとソースデータの間の必要なマッピングの自動作成も含まれます。

## ID

データセットのプライマリ ID は、ソースデータの CSV ファイルの最初の列に含まれています。 [!DNL Customer Attributes] ソースは、ID が常に [`CORE` 名前空間（[[!DNL Identity Service]](../../../identity-service/home.md) でサポートされるシステム生成の名前空間 ](../../../identity-service/features/namespaces.md) にマッピングされていることを前提としています。

ソースを使用する場合、ID の既存の名前空間を選択 [!DNL Customer Attributes] ることはできません。[!DNL Customer Attributes] れは、スキーマのプライマリ ID が常に ID マップにあることを前提としているからです。 次に [!DNL Customer Attributes] ソース ID と ID マップ UUID のマッピングを自動的に作成します。

[!DNL Customer Attributes] データを他の [!DNL Profile] データセットに結び付けるには、そのデータと ID をExperience Cloud ID と一致させる必要があります。

[Web SDK](/help/web-sdk/identity/overview.md)、{Mobile SDK](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/) または [5}Experience CloudID サービス API](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja) を使用して、訪問者のExperience CloudID を設定することで、`CORE` 名前空間を設定できます。[

[!DNL Customer Attributes] ファイルは、他の ID 関係をこれ以上入力しません。 例えば、[!DNL Customer Attributes] ソースデータセットに **メール** と **ロイヤルティ ID** フィールドが含まれる場合、[!DNL Identity Service] で処理するには、これらのフィールドにスキーマの ID フィールドというラベルを付ける必要があります。

詳しくは、[UI での  [!DNL Customer Attributes]  ソース接続の作成 ](../../tutorials/ui/create/adobe-applications/customer-attributes.md) に関するチュートリアルを参照してください。
