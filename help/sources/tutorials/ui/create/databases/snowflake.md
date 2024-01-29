---
title: UI でのSnowflakeソース接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用してSnowflakeソース接続を作成する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: fb2038b9-7f27-4818-b5de-cc8072122127
source-git-commit: 4de2193a45fc2925af310b5e2475eabe26d13adc
workflow-type: tm+mt
source-wordcount: '786'
ht-degree: 17%

---

# UI での [!DNL Snowflake] ソース接続の作成

>[!IMPORTANT]
>
>The [!DNL Snowflake] ソースは、Real-time Customer Data Platform Ultimate を購入したユーザーがソースカタログで利用できます。

このチュートリアルでは、 [!DNL Snowflake] Adobe Experience Platformユーザーインターフェイスを使用したソースコネクタ

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必要な資格情報の収集

次の資格情報プロパティの値を指定して、 [!DNL Snowflake] ソース。

>[!BEGINTABS]

>[!TAB アカウントキー認証]

| 資格情報 | 説明 |
| ---------- | ----------- |
| アカウント | アカウント名は、組織内のアカウントを一意に識別します。 その場合は、異なる [!DNL Snowflake] 組織。 これをおこなうには、アカウント名の前に組織名を追加する必要があります。 例えば、`orgname-account_name` のようになります。アカウント名の詳細については、 [!DNL Snowflake] に関するドキュメント [アカウント識別子](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization). |
| Warehouse | The [!DNL Snowflake] warehouse は、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に引き渡す際には、個別にアクセスする必要があります。 |
| データベース | The [!DNL Snowflake] データベースには、Platform を取り込むデータが含まれます。 |
| ユーザー名 | のユーザー名 [!DNL Snowflake] アカウント。 |
| パスワード | のパスワード [!DNL Snowflake] ユーザーアカウント。 |
| 役割 | デフォルトのアクセス制御の役割で、 [!DNL Snowflake] セッション。 この役割は、指定したユーザーに既に割り当てられている既存の役割である必要があります。 デフォルトの役割は `PUBLIC`. |
| 接続文字列 | に接続するために使用される接続文字列 [!DNL Snowflake] インスタンス。 次の接続文字列パターン： [!DNL Snowflake] 次に該当 `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` |

>[!TAB キーペア認証]

キーペア認証を使用するには、2048 ビットの RSA キーペアを生成し、次の値を指定して、 [!DNL Snowflake] ソース。

| 資格情報 | 説明 |
| --- | --- |
| アカウント | アカウント名は、組織内のアカウントを一意に識別します。 その場合は、異なる [!DNL Snowflake] 組織。 これをおこなうには、アカウント名の前に組織名を追加する必要があります。 例えば、`orgname-account_name` のようになります。アカウント名の詳細については、 [!DNL Snowflake] に関するドキュメント [アカウント識別子](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization). |
| ユーザー名 | ユーザー名 [!DNL Snowflake] アカウント。 |
| 秘密鍵 | The [!DNL Base64-]エンコードされた秘密鍵 [!DNL Snowflake] アカウント。 暗号化または暗号化されていない秘密鍵を生成できます。 暗号化された秘密鍵を使用する場合は、Experience Platformに対する認証時に秘密鍵のパスフレーズも指定する必要があります。 |
| 秘密鍵のパスフレーズ | 秘密鍵のパスフレーズは、暗号化された秘密鍵で認証する際に使用する必要があるセキュリティの追加レイヤーです。 暗号化されていない秘密鍵を使用している場合は、パスフレーズを指定する必要はありません。 |
| データベース | The [!DNL Snowflake] データベースに取り込むデータを含むExperience Platform。 |
| Warehouse | The [!DNL Snowflake] warehouse は、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に引き渡す際には、個別にアクセスする必要があります。 |

これらの値について詳しくは、 [このSnowflake文書](https://docs.snowflake.com/en/user-guide/key-pair-auth.html).

>[!ENDTABS]

Experience PlatformでSnowflakeアカウントにアクセスするには、次の認証値を指定する必要があります。

>[!NOTE]
>
>次の項目を設定する必要があります。 `PREVENT_UNLOAD_TO_INLINE_URL` フラグを設定 `FALSE` データのアンロードを許可する [!DNL Snowflake] データベースからExperience Platformへ。

## Snowflakeアカウントに接続

Platform UI で、「 」を選択します。 **[!UICONTROL ソース]** 左側のナビゲーションから、 [!UICONTROL ソース] ワークスペース。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

の下 [!UICONTROL データベース] カテゴリ、選択 **[!UICONTROL Snowflake]** 次に、「 **[!UICONTROL データを追加]**.

![ソースカタログは [!DNL Snowflake] ハイライト表示されました。](../../../../images/tutorials/create/snowflake/catalog.png)

The **[!UICONTROL Snowflakeに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、 [!DNL Snowflake] 接続するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![ソースワークフローの既存のアカウントインターフェイス。](../../../../images/tutorials/create/snowflake/existing.png)

### 新規アカウント

新しいアカウントを作成するには、 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、新しい [!DNL Snowflake] アカウント。

![ソースワークフローの新しいアカウントインターフェイス。](../../../../images/tutorials/create/snowflake/new.png)

>[!BEGINTABS]

>[!TAB アカウントキー認証]

アカウントキー認証を使用するには、入力フォームに接続文字列を入力し、 **[!UICONTROL ソースに接続]**.

![アカウントキー認証インターフェイス。](../../../../images/tutorials/create/snowflake/connection-string.png)

>[!TAB キーペア認証]

キーペア認証を使用するには、アカウント、ユーザー名、秘密鍵、秘密鍵のパスフレーズ、データベースおよびウェアハウスの値を指定してから、 **[!UICONTROL ソースに接続]**.

![アカウントのキーペア認証インターフェイス。](../../../../images/tutorials/create/snowflake/key-pair.png)

>[!ENDTABS]

## 次の手順

このチュートリアルに従って、Snowflakeアカウントへの接続を確立しました。 次のチュートリアルに進み、 [データをに取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/databases.md).
