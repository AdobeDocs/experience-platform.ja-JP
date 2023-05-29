---
title: UI での SugarCRM アカウントおよび連絡先ソース接続の作成
description: Adobe Experience Platform UI を使用して SugarCRM アカウントと連絡先ソース接続を作成する方法を説明します。
exl-id: 45840d7e-4c19-4720-8629-be446347862d
source-git-commit: e37c00863249e677f1645266859bf40fe6451827
workflow-type: tm+mt
source-wordcount: '779'
ht-degree: 39%

---

# （ベータ版） [!DNL SugarCRM Accounts & Contacts] UI のソース接続

>[!NOTE]
>
>[!DNL SugarCRM Accounts & Contacts] ソースはベータ版です。詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

このチュートリアルでは、 [!DNL SugarCRM Accounts & Contacts] Adobe Experience Platformユーザーインターフェイスを使用したソース接続

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

有効な [!DNL SugarCRM] アカウントを既にお持ちの場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/crm.md)に関するチュートリアルに進んでください。

### 必要な認証情報の収集

[!DNL SugarCRM Accounts & Contacts] を Platform に接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 | 例 |
| --- | --- | --- |
| `Host` | ソースが接続する SugarCRM API エンドポイント。 | `developer.salesfusion.com` |
| `Username` | SugarCRM 開発者アカウントのユーザー名。 | `abc.def@example.com@sugarmarketdemo000.com` |
| `Password` | SugarCRM 開発者アカウントのパスワード。 | `123456789` |

### Platform スキーマの作成

を作成する前に [!DNL SugarCRM] ソース接続の場合は、まず、ソースに使用する Platform スキーマを作成する必要もあります。 に関するチュートリアルを参照してください。 [Platform スキーマの作成](../../../../../xdm/schema/composition.md) スキーマの作成方法に関する包括的な手順を参照してください。

この [!DNL SugarCRM Accounts & Contacts] は複数の API をサポートしています。 つまり、活用するオブジェクトタイプに応じて、別のスキーマを作成する必要があります。 アカウントスキーマと連絡先スキーマの両方について、以下の例を参照してください。

>[!BEGINTABS]

>[!TAB アカウント]

![アカウントのスキーマ例を示した Platform UI のスクリーンショット](../../../../images/tutorials/create/sugarcrm-accounts-contacts/sugarcrm-schema-accounts.png)

>[!TAB 連絡先]

![連絡先のサンプルスキーマを示す Platform UI のスクリーンショット](../../../../images/tutorials/create/sugarcrm-accounts-contacts/sugarcrm-schema-contacts.png)

>[!ENDTABS]

## [!DNL SugarCRM Accounts & Contacts] アカウントの接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 *CRM* カテゴリ、選択 **[!UICONTROL SugarCRM アカウントおよび連絡先]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![SugarCRM アカウントおよび連絡先カードを含むカタログの Platform UI スクリーンショット](../../../../images/tutorials/create/sugarcrm-accounts-contacts/catalog-sugarcrm-accounts-contacts.png)

この **[!UICONTROL SugarCRM アカウントと連絡先アカウントを接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL SugarCRM Accounts & Contacts] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![SugarCRM アカウントと連絡先アカウントを既存のアカウントに接続するための Platform UI スクリーンショット](../../../../images/tutorials/create/sugarcrm-accounts-contacts/existing.png)

### 新規アカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新規アカウント]**」を選択し、続けて名前、説明（オプション）、 の認証情報を指定します。終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![SugarCRM アカウントと連絡先アカウントを新しいアカウントに接続するための Platform UI スクリーンショット](../../../../images/tutorials/create/sugarcrm-accounts-contacts/new.png)

### データの選択

最後に、Platform に取り込むオブジェクトタイプを選択する必要があります。

| オブジェクトタイプ | 説明 |
| --- | --- |
| `Accounts` | 組織が関係を持つ会社。 |
| `Contacts` | 組織と既に関係が構築されている個人。 |

>[!BEGINTABS]

>[!TAB アカウント]

![SugarCRM アカウントおよび連絡先用のプラットフォーム UI スクリーンショットが、「アカウント」オプションが選択された状態で設定されていることを示しています](../../../../images/tutorials/create/sugarcrm-accounts-contacts/configuration-accounts.png)

>[!TAB 連絡先]

![「連絡先」オプションが選択された設定を示す SugarCRM アカウントおよび連絡先用のプラットフォーム UI スクリーンショット](../../../../images/tutorials/create/sugarcrm-accounts-contacts/configuration-contacts.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL SugarCRM Accounts & Contacts] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを Platform に取り込むためのデータフローの設定](../../dataflow/crm.md)を行いましょう。

## その他のリソース

以下の節では、 [!DNL SugarCRM] ソース。

### ガードレール {#guardrails}

この [!DNL SugarCRM] API スロットル率は、1 分あたり 90 呼び出し、または 1 日あたり 2,000 呼び出し（いずれか最初に実行される方）です。 ただし、この制限は、レート制限に達しないようにリクエスト時間を遅延させるパラメーターを接続仕様に追加することで回避されました。

### 検証 {#validation}

ソースとが正しく設定されていることを検証するには、以下を実行します。 [!DNL SugarCRM Accounts & Contacts] データを取り込むには、次の手順に従います。

* Platform UI で、「 **[!UICONTROL データフローを表示]** の横に [!DNL SugarCRM Accounts & Contacts] ソースカタログのカードメニュー 次に、 **[!UICONTROL データセットをプレビュー]** をクリックして、取り込まれたデータを確認します。

* 操作しているオブジェクトタイプに応じて、集計データを、 [!DNL SugarMarket] 以下のアカウントまたは連絡先ページ：

>[!BEGINTABS]

>[!TAB アカウント]

![SugarMarket アカウントページのスクリーンショットに、アカウントのリストが表示されています](../../../../images/tutorials/create/sugarcrm-accounts-contacts/sugarmarket-accounts.png)

>[!TAB 連絡先]

![連絡先のリストが表示された SugarMarket 連絡先ページのスクリーンショット](../../../../images/tutorials/create/sugarcrm-accounts-contacts/sugarmarket-contacts.png)

>[!ENDTABS]

>[!NOTE]
>
>この [!DNL SugarMarket] ページには、削除されたオブジェクトの数は含まれません。 ただし、このソースを通じて取得されたデータには削除された数も含まれるので、削除済みフラグが付けられます。
