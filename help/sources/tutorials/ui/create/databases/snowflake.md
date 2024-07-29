---
title: UI でのSnowflakeのSource接続の作成
type: Tutorial
description: Adobe Experience Platform UI を使用してSnowflakeソース接続を作成する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: fb2038b9-7f27-4818-b5de-cc8072122127
source-git-commit: d89e0c81bd250e41a863b8b28d358cc6ddea1c37
workflow-type: tm+mt
source-wordcount: '792'
ht-degree: 18%

---

# UI での [!DNL Snowflake] ソース接続の作成

>[!IMPORTANT]
>
>Real-time Customer Data Platform Ultimate を購入したユーザーは、ソースカタログで [!DNL Snowflake] ソースを利用できます。

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL Snowflake] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必要な資格情報の収集

[!DNL Snowflake] ソースを認証するには、次の資格情報プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

| 資格情報 | 説明 |
| ---------- | ----------- |
| アカウント | アカウント名は、組織内のアカウントを一意に識別します。 この場合、アカウントを異なる [!DNL Snowflake] 組織で一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を追加する必要があります。 例：`orgname-account_name`。 詳しくは、[ アカウント識別子の取得 ](../../../../connectors/databases/snowflake.md#retrieve-your-account-identifier) に関するガイドを参  [!DNL Snowflake]  してください。 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| ウェアハウス | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に取り込む際は個別にアクセスする必要があります。 |
| データベース | [!DNL Snowflake] データベースには、Platform に取り込むデータが含まれています。 |
| ユーザー名 | [!DNL Snowflake] アカウントのユーザー名。 |
| パスワード | [!DNL Snowflake] ユーザーアカウントのパスワード。 |
| 役割 | [!DNL Snowflake] セッションで使用する既定のアクセス制御ロールです。 役割は、指定したユーザーに既に割り当てられている既存の役割である必要があります。 デフォルトの役割は `PUBLIC` です。 |
| 接続文字列 | [!DNL Snowflake] インスタンスへの接続に使用する接続文字列。 [!DNL Snowflake] の接続文字列パターンは `jdbc:snowflake://{ACCOUNT_NAME}.snowflakecomputing.com/?user={USERNAME}&password={PASSWORD}&db={DATABASE}&warehouse={WAREHOUSE}` です |

>[!TAB  キーペア認証 ]

キーペア認証を使用するには、[!DNL Snowflake] ソースのアカウントを作成する際に、2048 ビットの RSA キーペアを生成してから、次の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| アカウント | アカウント名は、組織内のアカウントを一意に識別します。 この場合、アカウントを異なる [!DNL Snowflake] 組織で一意に識別する必要があります。 これを行うには、アカウント名の前に組織名を追加する必要があります。 例：`orgname-account_name`。 詳しくは、[ アカウント識別子の取得 ](../../../../connectors/databases/snowflake.md#retrieve-your-account-identifier) に関するガイドを参  [!DNL Snowflake]  してください。 詳しくは、[[!DNL Snowflake] ドキュメント](https://docs.snowflake.com/en/user-guide/admin-account-identifier#format-1-preferred-account-name-in-your-organization)を参照してください。 |
| ユーザー名 | [!DNL Snowflake] アカウントのユーザー名。 |
| 秘密鍵 | [!DNL Snowflake] アカウントの [!DNL Base64-] エンコードされた秘密鍵。 暗号化された秘密鍵または暗号化されていない秘密鍵のいずれかを生成できます。 暗号化された秘密鍵を使用している場合は、Experience Platformに対する認証の際に、秘密鍵のパスフレーズも指定する必要があります。 詳しくは、[ 秘密鍵の取得 ](../../../../connectors/databases/snowflake.md) に関す  [!DNL Snowflake]  ガイドを参照してください。 |
| 秘密鍵のパスフレーズ | 秘密鍵のパスフレーズは、暗号化された秘密鍵を使用して認証を行う場合に使用する必要がある、追加のセキュリティレイヤーです。 暗号化されていない秘密鍵を使用している場合は、パスフレーズを指定する必要はありません。 |
| データベース | Experience Platformに取り込むデータを含む [!DNL Snowflake] データベース。 |
| ウェアハウス | [!DNL Snowflake] ウェアハウスは、アプリケーションのクエリ実行プロセスを管理します。 各 [!DNL Snowflake] ウェアハウスは互いに独立しており、データを Platform に取り込む際は個別にアクセスする必要があります。 |

これらの値について詳しくは、[ このSnowflakeドキュメント ](https://docs.snowflake.com/en/user-guide/key-pair-auth.html) を参照してください。

>[!ENDTABS]

>[!NOTE]
>
>[!DNL Snowflake] データベースからデータをExperience Platformにアンロードできるようにするには、`PREVENT_UNLOAD_TO_INLINE_URL` フラグを `FALSE` に設定する必要があります。

## Snowflakeアカウントを接続

Platform の UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

[!UICONTROL  データベース ] カテゴリで、「**[!UICONTROL Snowflake」を選択し]** 「**[!UICONTROL データを追加]**」を選択します。

![[!DNL Snowflake] がハイライト表示されているソースカタログ ](../../../../images/tutorials/create/snowflake/catalog.png)

**[!UICONTROL Snowflakeに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、接続する [!DNL Snowflake] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![ ソースワークフローの既存のアカウントインターフェイス。](../../../../images/tutorials/create/snowflake/existing.png)

### 新規アカウント

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、新しい [!DNL Snowflake] アカウントの名前と説明（オプション）を入力します。

![ ソースワークフローの新しいアカウントインターフェイス ](../../../../images/tutorials/create/snowflake/new.png)

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用するには、入力フォームに接続文字列を入力し、「**[!UICONTROL ソースに接続]**」を選択します。

![ アカウントキー認証インターフェイス ](../../../../images/tutorials/create/snowflake/connection-string.png)

>[!TAB  キーペア認証 ]

キーペア認証を使用するには、アカウント、ユーザー名、秘密鍵、秘密鍵のパスフレーズ、データベースおよびウェアハウスの値を指定し、「**[!UICONTROL ソースに接続]**」を選択します。

![ アカウントキーペア認証インターフェイス ](../../../../images/tutorials/create/snowflake/key-pair.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、Snowflakeアカウントとの接続を確立しました。 次のチュートリアルに進み、[ データをに取り込むためのデータフローの設定  [!DNL Platform]](../../dataflow/databases.md) を行いましょう。
