---
keywords: Experience Platform；ホーム；人気の高いトピック；顧客属性コネクタ
solution: Experience Platform
title: 顧客属性ソースコネクタの概要
description: API またはユーザーインターフェイスを使用して顧客属性をAdobe Experience Platformに接続する方法を説明します
exl-id: 63765ecd-ddb5-4992-a3de-d53f054bfb28
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 23%

---

# 顧客属性コネクタ

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

[[!DNL Customer Attributes]](https://experienceleague.adobe.com/docs/core-services/interface/services/customer-attributes/attributes.html?lang=ja) 「Experience Cloud」では、顧客関係管理 (CRM) データベースから取り込んだ企業データをアップロードできます。 データを Experience Cloud の顧客属性データソースにアップロードすると、そのデータを Adobe Analytics および Adobe Target で使用できます。

Experience Platformは、取り込みのサポートを提供しています [!DNL Customer Attributes] プロファイルデータをAdobe Experience Platformに送信します。

## データセットとスキーマ

The [!DNL Customer Attributes] ソースは、宛先となるデータのデータセットを自動的に作成します。 この自動作成されたデータセットは修正されており、手動で選択することはできません。 また、入力データソースに基づいて、データセットのスキーマも自動的に作成されます。 また、このプロセスでは、スキーマとソースデータの間の必要なマッピングの自動作成もおこないます。

## ID

データセットのプライマリ ID は、ソースデータの CSV ファイルの最初の列に含まれます。 The [!DNL Customer Attributes] ソースは、id が常に [`CORE` 名前空間](../../../identity-service/namespaces.md)：でサポートされるシステム生成名前空間。 [[!DNL Identity Service]](../../../identity-service/home.md).

使用時に ID 用の既存の名前空間を選択することはできません [!DNL Customer Attributes] ソースの理由： [!DNL Customer Attributes] は、スキーマのプライマリ ID が常に id マップ内にあると仮定します。 [!DNL Customer Attributes] 次に、ソース ID と id マップ UUID のマッピングを自動的に作成します。

の場合 [!DNL Customer Attributes] 他のデータと結び付けるデータ [!DNL Profile] データセット、そのデータおよび id をExperience CloudID と照合する必要があります。

この `CORE` 名前空間を作成するには、次を使用して訪問者のExperience CloudID を設定します。 [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=ja), [モバイル SDK](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/)、または [Experience CloudID サービス API](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja).

The [!DNL Customer Attributes] ファイルは、その他の id 関係をさらに設定しません。 例えば、 [!DNL Customer Attributes] ソースデータセットに次が含まれている **電子メール** および **ロイヤリティ ID** フィールドに値を入力する場合、それらのフィールドを処理するには、スキーマ内の id フィールドとしてラベル付けする必要があります。 [!DNL Identity Service].

に関するチュートリアルを参照してください。 [作成 [!DNL Customer Attributes] UI のソース接続](../../tutorials/ui/create/adobe-applications/customer-attributes.md) を参照してください。
