---
keywords: Experience Platform；ホーム；人気のあるトピック；顧客属性コネクタ
solution: Experience Platform
title: 顧客属性ソースコネクタの概要
topic-legacy: overview
description: APIまたはユーザーインターフェイスを使用して顧客属性をAdobe Experience Platformに接続する方法を説明します
exl-id: 63765ecd-ddb5-4992-a3de-d53f054bfb28
source-git-commit: 541cc87f218a6ef3dcca37573f6d0f9cf560edfb
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 11%

---

# 顧客属性コネクタ

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[[!DNL Customer Attributes]](https://experienceleague.adobe.com/docs/core-services/interface/services/customer-attributes/attributes.html?lang=en) 「 」Experience Cloudでは、顧客関係管理(CRM)データベースから取り込んだ企業データをアップロードできます。データをExperience Cloudの顧客属性データソースにアップロードし、Adobe AnalyticsとAdobe Targetで使用できます。

Experience Platformは、[!DNL Customer Attributes]プロファイルデータをAdobe Experience Platformに取り込む機能を提供しています。

## データセットとスキーマ

[!DNL Customer Attributes]ソースは、宛先となるデータのデータセットを自動的に作成します。 この自動作成されたデータセットは固定されており、手動で選択することはできません。 また、入力データソースに基づいて、データセットのスキーマも自動作成されます。 また、このプロセスでは、スキーマとソースデータの間の必要なマッピングの自動作成もおこないます。

## ID

データセットのプライマリIDは、ソースデータのCSVファイルの最初の列に含まれます。 [!DNL Customer Attributes]ソースは、IDが常に[`CORE`名前空間](../../../identity-service/namespaces.md)（[[!DNL Identity Service]](../../../identity-service/home.md)でサポートされるシステム生成名前空間）にマッピングされることを前提としています。

[!DNL Customer Attributes]ソースを使用する場合、IDの既存の名前空間を選択できません。[!DNL Customer Attributes]は、スキーマのプライマリIDが常にIDマップにあると仮定します。 [!DNL Customer Attributes] 次に、ソースIDとidマップUUIDのマッピングを自動的に作成します。

[!DNL Customer Attributes]データを他の[!DNL Profile]データセットと結び付けるには、そのデータとIDをExperience CloudIDと照合できる必要があります。

`CORE`名前空間を確立するには、[Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=en)、[Mobile SDK](https://aep-sdks.gitbook.io/docs/foundation-extensions/mobile-core/identity)または[Experience CloudIDサービスAPI](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=en)を使用して、訪問者のExperience CloudIDを設定します。

[!DNL Customer Attributes]ファイルは、その他のID関係をさらに設定しません。 例えば、[!DNL Customer Attributes]ソースデータセットに&#x200B;**Email**&#x200B;と&#x200B;**Loyalty ID**&#x200B;フィールドが含まれる場合、[!DNL Identity Service]に処理するには、それらのフィールドをスキーマ内のIDフィールドとしてラベル付けする必要があります。

詳しくは、[UIでの [!DNL Customer Attributes] ソース接続の作成](../../tutorials/ui/create/adobe-applications/customer-attributes.md)に関するチュートリアルを参照してください。
