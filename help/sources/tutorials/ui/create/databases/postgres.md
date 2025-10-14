---
title: UI を使用した PostgreSQL のExperience Platformへの接続
description: Experience Platform ユーザーインターフェイスのソースワークスペースを使用して、PostgreSQL データベースをExperience Platformに接続する方法について説明します。
exl-id: e556d867-a1eb-4900-b8a9-189666a4f3f1
source-git-commit: f4200ca71479126e585ac76dd399af4092fdf683
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 18%

---

# UI を使用した [!DNL PostgreSQL] のExperience Platformへの接続

このガイドでは、Experience Platform ユーザーインターフェイスのソースワークスペースを使用して [!DNL PostgreSQL] データベースをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL PostgreSQL] 接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/databases.md)に関するチュートリアルに進むことができます。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL PostgreSQL]  概要 &#x200B;](../../../../connectors/databases/postgres.md) を参照してください。

### 接続文字列の SSL 暗号化を有効にする

次のプロパティを使用して接続文字列を追加することで、[!DNL PostgreSQL] 接続文字列の SSL 暗号化を有効にできます。

| プロパティ | 説明 | 例 |
| --- | --- | --- |
| `EncryptionMethod` | [!DNL PostgreSQL] データに対して SSL 暗号化を有効にできます。 | <uL><li>`EncryptionMethod=0` （無効）</li><li>`EncryptionMethod=1` （有効）</li><li>`EncryptionMethod=6` （RequestSSL）</li></ul> |
| `ValidateServerCertificate` | `EncryptionMethod` ータの適用時に [!DNL PostgreSQL] データベースから送信された証明書を検証します。 | <uL><li>`ValidationServerCertificate=0` （無効）</li><li>`ValidationServerCertificate=1` （有効）</li></ul> |

次に、SSL 暗号化が追加された [!DNL PostgreSQL] 接続文字列の例を示します。`Server={SERVER};Database={DATABASE};Port={PORT};UID={USERNAME};Password={PASSWORD};EncryptionMethod=1;ValidateServerCertificate=1`

## ソースカタログのナビゲート {#navigate}

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、*[!UICONTROL Sources]* ワークスペースにアクセスします。 *[!UICONTROL カテゴリ]* パネルで適切なカテゴリを選択するか、検索バーを使用して、使用する特定のソースに移動します。

[!DNL PostgreSQL] を使用するには、「*[!UICONTROL データベース&#x200B;**[!UICONTROL 」の下の「]**&#x200B;PostgreSQL DB]*」ソースカードを選択し、「**[!UICONTROL 設定]**」を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントを作成すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![PostgreSQL ソースカードが選択されたソースカタログ &#x200B;](../../../../images/tutorials/create/postgresql/catalog.png)


## 既存のアカウントを使用 {#existing}

既存のアカウントを使用するには、「**[!UICONTROL 既存のアカウント]**」を選択して、使用する [!DNL PostgreSQL] アカウントを選択します。

![&#x200B; ソースワークフローの既存のアカウントインターフェイス。](../../../../images/tutorials/create/postgresql/existing.png)

## 新しいアカウントを作成 {#create}

既存のアカウントがない場合は、ソースに対応する必要な認証資格情報を指定して、新しいアカウントを作成する必要があります。

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、アカウントの名前を入力して、オプションで説明を追加します。

![&#x200B; アカウント名とオプションの説明が表示された、ソースワークフローの新しいアカウントインターフェイス。](../../../../images/tutorials/create/postgresql/new.png)

### Azure 上のExperience Platformへの接続 {#azure}

アカウントキーまたは基本認証を使用して、[!DNL PostgreSQL] アカウントを Azure 上のExperience Platformに接続できます。

>[!BEGINTABS]

>[!TAB  アカウントキー認証 ]

アカウントキー認証を使用するには、「**[!UICONTROL アカウントキー認証]**」を選択し、「[&#x200B; 接続文字列 &#x200B;](../../../../connectors/databases/postgres.md#azure)」を入力して、「**[!UICONTROL ソースに接続]**」を選択します。

![&#x200B; 「アカウントキー認証」が選択されたソースワークフローの新しいアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/postgresql/account-key.png)

>[!TAB  基本認証 ]

基本認証を使用する場合は、「**[!UICONTROL 基本認証]**」を選択し、[&#x200B; 認証資格情報 &#x200B;](../../../../connectors/databases/postgres.md#azure) の値を入力して「**[!UICONTROL ソースに接続]**」を選択します。

![&#x200B; ソースワークフローで「基本認証」が選択された新しいアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/postgresql/basic-auth.png)

>[!ENDTABS]

### Amazon Web ServicesのExperience Platform（AWS）への接続 {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../../../landing/multi-cloud.md) を参照してください。

新しい [!DNL PostgreSQL] アカウントを作成し、AWSでExperience Platformに接続するには、VA6 サンドボックスに属していることを確認し、必要な [&#x200B; 認証用の資格情報 &#x200B;](../../../../connectors/databases/postgres.md#aws) を指定します。

![AWSに接続するための、ソースワークフローの新しいアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/postgresql/aws.png)

## [!DNL PostgreSQL] データのデータフローの作成

このチュートリアルでは、[!DNL MariaDB] アカウントとの接続を確立しました。次のチュートリアルに進み、[&#x200B; データをExperience Platformに取り込むためのデータフローの設定 &#x200B;](../../dataflow/databases.md) を行いましょう。
