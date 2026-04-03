---
keywords: Experience Platform；ホーム；人気のトピック；顧客属性コネクタ
solution: Experience Platform
title: 顧客属性Source コネクタの概要
description: APIまたはユーザーインターフェイスを使用して、顧客属性をAdobe Experience Platformに接続する方法について説明します
exl-id: 63765ecd-ddb5-4992-a3de-d53f054bfb28
source-git-commit: b292b9243816b1eed7fd3939096ddc30d6be0606
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 19%

---

# 顧客属性コネクタ

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Cloudの[[!DNL Customer Attributes]](https://experienceleague.adobe.com/docs/core-services/interface/services/customer-attributes/attributes.html?lang=ja)を使用すると、取り込んだエンタープライズ データをCRM （顧客関係管理） データベースからアップロードできます。 データを Experience Cloud の顧客属性データソースにアップロードすると、そのデータを Adobe Analytics および Adobe Target で使用できます。

Experience Platformでは、[!DNL Customer Attributes] プロファイルデータをAdobe Experience Platformに取り込む機能がサポートされています。

## データセットとスキーマ

[!DNL Customer Attributes] ソースは、データを格納するデータセットを自動的に作成します。 この自動作成データセットは修正されており、手動で選択することはできません。 また、ソースは、入力データソースに基づいて、データセットのスキーマを自動作成します。 このプロセスでは、スキーマとソースデータ間で必要なマッピングを自動作成することも含まれます。

## ID

データセットのプライマリ IDは、ソースデータのCSV ファイルの最初の列に含まれます。 [!DNL Customer Attributes] ソースは、IDが常に[`CORE`によってサポートされているシステム生成の名前空間である](../../../identity-service/features/namespaces.md)名前空間[[!DNL Identity Service]](../../../identity-service/home.md)にマッピングされていることを前提としています。

[!DNL Customer Attributes] ソースを使用する場合、[!DNL Customer Attributes]はスキーマのプライマリ IDが常にID マップ内にあると仮定するため、IDの既存の名前空間を選択できません。 次に、[!DNL Customer Attributes]は、ソース IDとID マップ UUIDのマッピングを自動化された方法で作成します。

[!DNL Customer Attributes] データを他の[!DNL Profile] データセットに関連付けるには、そのデータとIDをExperience Cloud IDと照合できる必要があります。

`CORE`名前空間を確立するには、[ データ収集](/help/collection/identity/overview.md)のID、[ モバイル SDK](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/)または[Experience Cloud ID サービス API](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja)を使用して、訪問者用のExperience Cloud IDを設定します。

[!DNL Customer Attributes] ファイルは、他のID関係にそれ以上入力されません。 例えば、[!DNL Customer Attributes]のソースデータセットに&#x200B;**電子メール**&#x200B;と&#x200B;**ロイヤルティ ID** フィールドが含まれている場合、これらのフィールドを[!DNL Identity Service]に処理するには、スキーマのID フィールドとしてラベル付けする必要があります。

詳しくは、[UIでの [!DNL Customer Attributes]  ソース接続の作成](../../tutorials/ui/create/adobe-applications/customer-attributes.md)に関するチュートリアルを参照してください。
