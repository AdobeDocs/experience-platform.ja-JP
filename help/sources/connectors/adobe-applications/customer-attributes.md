---
keywords: Experience Platform；ホーム；人気のあるトピック；顧客属性コネクタ
solution: Experience Platform
title: 顧客属性ソースコネクタの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して顧客属性をAdobe Experience Platformに接続する方法を説明します
exl-id: 63765ecd-ddb5-4992-a3de-d53f054bfb28
source-git-commit: 541cc87f218a6ef3dcca37573f6d0f9cf560edfb
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 19%

---

# 顧客属性コネクタ

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[[!DNL Customer Attributes]](https://experienceleague.adobe.com/docs/core-services/interface/services/customer-attributes/attributes.html?lang=en) 「Experience Cloud」では、顧客関係管理 (CRM) データベースから取り込んだ企業データをアップロードできます。データを Experience Cloud の顧客属性データソースにアップロードすると、そのデータを Adobe Analytics および Adobe Target で使用できます。

Experience Platformは、[!DNL Customer Attributes] プロファイルデータをAdobe Experience Platformに取り込む機能を提供しています。

## データセットとスキーマ

[!DNL Customer Attributes] ソースは、宛先となるデータのデータセットを自動的に作成します。 この自動作成されたデータセットは修正されており、手動で選択することはできません。 また、入力データソースに基づいて、データセットのスキーマも自動作成されます。 このプロセスでは、スキーマとソースデータの間の必要なマッピングの自動作成も行われます。

## ID

データセットのプライマリ ID は、ソースデータの CSV ファイルの最初の列に含まれます。 [!DNL Customer Attributes] ソースは、ID が常に [`CORE` 名前空間 ](../../../identity-service/namespaces.md)（[[!DNL Identity Service]](../../../identity-service/home.md) でサポートされるシステム生成名前空間）にマッピングされていると想定します。

[!DNL Customer Attributes] ソースを使用する場合は、ID の既存の名前空間を選択できません。[!DNL Customer Attributes] は、スキーマのプライマリ ID が常に ID マップにあると仮定します。 [!DNL Customer Attributes] 次に、ソース ID と id マップ UUID のマッピングを自動的に作成します。

[!DNL Customer Attributes] データを他の [!DNL Profile] データセットと結び付けるには、そのデータと ID がExperience CloudID と一致する必要があります。

`CORE` 名前空間を設定するには、[Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en)、[Mobile SDK](https://aep-sdks.gitbook.io/docs/foundation-extensions/mobile-core/identity) または [Experience CloudID サービス API](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja) を使用して、訪問者のExperience CloudID を設定します。

[!DNL Customer Attributes] ファイルは、他の ID 関係をさらに設定しません。 例えば、[!DNL Customer Attributes] ソースデータセットに **Email** と **Loyalty ID** フィールドが含まれる場合、[!DNL Identity Service] に処理するには、それらのフィールドをスキーマ内の ID フィールドとしてラベル付けする必要があります。

詳しくは、[UI での  [!DNL Customer Attributes]  ソース接続の作成 ](../../tutorials/ui/create/adobe-applications/customer-attributes.md) に関するチュートリアルを参照してください。
