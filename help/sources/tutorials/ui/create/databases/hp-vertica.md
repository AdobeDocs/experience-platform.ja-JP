---
keywords: Experience Platform；ホーム；人気の高いトピック；HP Vertica
solution: Experience Platform
title: UIでHP Vertica Source Connectionを作成する
topic-legacy: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してHP Verticaソース接続を作成する方法を説明します。
exl-id: d7315ad4-9250-4e66-be33-016efabb512e
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 9%

---

# UIでHP [!DNL Vertica]ソース接続を作成する

>[!NOTE]
>
> HP [!DNL Vertica]コネクタはベータ版です。 ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザインターフェイスを使用してHP [!DNL Vertica]ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なHP [!DNL Vertica]接続をお持ちの場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)のチュートリアルに進むことができます。

### 必要な資格情報の収集

次の節では、[!DNL Flow Service] APIを使用してHP [!DNL Vertica]に正しく接続するために知っておく必要がある追加情報を示します。

| Credential | 説明 |
| ---------- | ----------- |
| `connectionString` | HP [!DNL Vertica]インスタンスへの接続に使用する接続文字列。 HP [!DNL Vertica]の接続文字列パターンは`Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`です |

開始方法の詳細については、[このHP [!DNL Vertica] ドキュメント](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/ConnectingToVertica/ClientJDBC/CreatingAndConfiguringAConnection.htm)を参照してください。

## HP [!DNL Vertica]アカウントに接続

必要な資格情報を収集したら、次の手順に従ってHP [!DNL Vertica]アカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

**[!UICONTROL Databases]**&#x200B;カテゴリーの下で、**[!UICONTROL HP Vertica]**&#x200B;を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しいHP [!DNL Vertica]コネクタを作成します。

![カタログ](../../../../images/tutorials/create/hp-vertica/catalog.png)

「**[!UICONTROL Connect to HP Vertica]**」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームに、名前、オプションの説明、HP [!DNL Vertica]資格情報を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/hp-vertica/new.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するHP [!DNL Vertica]アカウントを選択し、右上隅の「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/hp-vertica/existing.png)

## 次の手順

このチュートリアルに従うと、HP [!DNL Vertica]アカウントへの接続が確立されます。 次のチュートリアルに進み、[データを [!DNL Platform]](../../dataflow/databases.md)に取り込むようにデータフローを設定できます。
