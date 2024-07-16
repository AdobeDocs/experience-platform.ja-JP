---
title: UI での SugarCRM アカウントおよび連絡先ソース接続の作成
description: Adobe Experience Platform UI を使用して SugarCRM アカウントおよび連絡先ソース接続を作成する方法を説明します。
exl-id: 45840d7e-4c19-4720-8629-be446347862d
source-git-commit: 0de4b32ac2ddc90dabefd469b6658388a4532e0d
workflow-type: tm+mt
source-wordcount: '764'
ht-degree: 37%

---

# UI での [!DNL SugarCRM Accounts & Contacts] ソース接続の作成

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL SugarCRM Accounts & Contacts] ソース接続を作成する手順について説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

有効な [!DNL SugarCRM] アカウントを既にお持ちの場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/crm.md)に関するチュートリアルに進んでください。

### 必要な資格情報の収集

[!DNL SugarCRM Accounts & Contacts] を Platform に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| `Host` | ソースが接続する SugarCRM API エンドポイント。 | `developer.salesfusion.com` |
| `Username` | SugarCRM 開発者アカウントのユーザー名。 | `abc.def@example.com@sugarmarketdemo000.com` |
| `Password` | SugarCRM 開発者アカウントのパスワード。 | `123456789` |

### Platform スキーマの作成

また、[!DNL SugarCRM] ソース接続を作成する前に、まずソースに使用する Platform スキーマを作成する必要があります。 スキーマの作成方法に関する包括的な手順については、[Platform スキーマの作成 ](../../../../../xdm/schema/composition.md) に関するチュートリアルを参照してください。

[!DNL SugarCRM Accounts & Contacts] は複数の API をサポートしています。 つまり、活用しているオブジェクトタイプに応じて、個別のスキーマを作成する必要があります。 アカウントおよび連絡先スキーマの両方については、以下の例を参照してください。

>[!BEGINTABS]

>[!TAB アカウント]

![ アカウントのスキーマの例を示す Platform UI のスクリーンショット ](../../../../images/tutorials/create/sugarcrm-accounts-contacts/sugarcrm-schema-accounts.png)

>[!TAB  連絡先 ]

![ 連絡先のスキーマの例を示す Platform UI のスクリーンショット ](../../../../images/tutorials/create/sugarcrm-accounts-contacts/sugarcrm-schema-contacts.png)

>[!ENDTABS]

## [!DNL SugarCRM Accounts & Contacts] アカウントの接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*CRM* カテゴリ内で、「**[!UICONTROL SugarCRM アカウントと連絡先]**」、「**[!UICONTROL データを追加]**」の順に選択します。

![SugarCRM アカウントと連絡先カードを含んだカタログの Platform UI スクリーンショット ](../../../../images/tutorials/create/sugarcrm-accounts-contacts/catalog-sugarcrm-accounts-contacts.png)

**[!UICONTROL SugarCRM アカウントと連絡先アカウントを接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL SugarCRM Accounts & Contacts] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![SugarCRM アカウントと連絡先アカウントを既存のアカウントに接続するの Platform UI のスクリーンショット ](../../../../images/tutorials/create/sugarcrm-accounts-contacts/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、続けて名前、説明（オプション）、の認証情報を指定します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![SugarCRM アカウントと連絡先アカウントを新しいアカウントに接続するための Platform UI のスクリーンショット ](../../../../images/tutorials/create/sugarcrm-accounts-contacts/new.png)

### データの選択

最後に、Platform に取得するオブジェクトタイプを選択する必要があります。

| オブジェクトタイプ | 説明 |
| --- | --- |
| `Accounts` | 組織が関係を持つ会社。 |
| `Contacts` | 組織と確立された関係を持つ個人。 |

>[!BEGINTABS]

>[!TAB アカウント]

![ アカウントオプションが選択された設定を示す SugarCRM アカウントおよび連絡先の Platform UI スクリーンショット ](../../../../images/tutorials/create/sugarcrm-accounts-contacts/configuration-accounts.png)

>[!TAB  連絡先 ]

![ 「連絡先」オプションが選択された設定を示す、SugarCRM アカウントおよび連絡先の Platform UI スクリーンショット ](../../../../images/tutorials/create/sugarcrm-accounts-contacts/configuration-contacts.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL SugarCRM Accounts & Contacts] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを Platform に取り込むためのデータフローの設定](../../dataflow/crm.md)を行いましょう。

## その他のリソース

以下の節では、[!DNL SugarCRM] ソースを使用する際に参照できるその他のリソースを示します。

### ガードレール {#guardrails}

[!DNL SugarCRM] API のスロットル率は、1 分あたりの呼び出し 90 回か 1 日あたりの呼び出し 2000 回のいずれか早い方です。 ただし、この制限は、接続仕様にパラメーターを追加することで回避されました。このパラメーターはリクエスト時間を遅延させるので、レート制限に到達することはありません。

### 検証 {#validation}

ソースを正しく設定し、データが取り込まれてい [!DNL SugarCRM Accounts & Contacts] ことを検証するには、次の手順に従います。

* Platform UI で、ソースカタログの [!DNL SugarCRM Accounts & Contacts] ーザーカードメニューの横にある **[!UICONTROL データフローを表示]** を選択します。 次に、「**[!UICONTROL データセットをプレビュー]**」をクリックして、取り込まれたデータを確認します。

* 使用しているオブジェクト・タイプに応じて、集計データを次の [!DNL SugarMarket] の「アカウント」ページまたは「連絡先」ページに表示されるカウントと照合して検証できます。

>[!BEGINTABS]

>[!TAB アカウント]

![ アカウントのリストを表示する SugarMarket アカウントページからのスクリーンショット ](../../../../images/tutorials/create/sugarcrm-accounts-contacts/sugarmarket-accounts.png)

>[!TAB  連絡先 ]

![ 連絡先のリストを表示する SugarMarket 連絡先ページからのスクリーンショット ](../../../../images/tutorials/create/sugarcrm-accounts-contacts/sugarmarket-contacts.png)

>[!ENDTABS]

>[!NOTE]
>
>[!DNL SugarMarket] ページには、削除されたオブジェクトの数は含まれません。 ただし、このソースを通じて取得されたデータには、削除されたカウントも含まれ、これらは削除されたフラグでマークされます。
