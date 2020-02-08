---
title: 宛先ワークスペース
seo-title: 宛先ワークスペース
description: Adobe Real-time Customer Data Platformで、左側のナビゲーションバーから「Destinations」を選択して、宛先ワークスペースにアクセスします。
seo-description: Adobe Real-time Customer Data Platformで、左側のナビゲーションバーから「Destinations」を選択して、宛先ワークスペースにアクセスします。
translation-type: tm+mt
source-git-commit: 132bc9787a86045adba559c769b02927a6045b17

---


# 宛先ワークスペース {#destinations-workspace}

Adobe Real-time Customer Data Platformで、左側のナビゲーションバーから「 **Destinations** 」を選択して、Destinationsワークスペースにアクセスします。

「宛先」ワークスペースは、 **Catalog**、 **Browse**、 **Accounts**、 **Dataフローの4つのセクションで構成され、**&#x200B;以下の各セクションで説明します。

![宛先 — 概要](/help/rtcdp/destinations/assets/destinations-overview.png)

## Catalog {#catalog}

「カ **[!UICONTROL タログ]** 」タブには、アドビが提供するすべての宛先のリストが表示されます。このリストには、データを送信できます。 カタログ内のリンク先を選択して、右側のレールを開きます。 ここでは、宛先への接続を設定する(**Connect destination**)か、ドキュメント(**View documentation**)を参照して各宛先に関する詳細情報を確認できます。

![保存先カタログオプション](/help/rtcdp/destinations/assets/destination-ui-catalog-options.png)

宛先カテゴリと各宛先の情報について詳しくは、宛先カタログを参照 [してください](/help/rtcdp/destinations/destinations-catalog.md)。

## 参照 {#browse}

「参 **[!UICONTROL 照]** 」タブには、接続を確立した宛先が表示されます。 切り替えを有効にした宛先はアクティブに設定し、逆も同様に設定します。 また、セグメント/参照を選択し、検査するセグメントを選択すると、デ **ータのフロー先を表示する** こともできます。 「参照」タブで各宛先に対して提供されるすべての情報については、次の表を参照してください。

![「参照」タブ](/help/rtcdp/destinations/assets/browse-tab.png)

| 要素 | 説明 |
---------|----------
| 宛先名 | この宛先へのアクティベートフローに指定した名前。 |
| Destination | アクティベーションフローに対して選択した宛先プラットフォームです。 |
| Created | 宛先へのアクティブ化フローが作成された日時です。 |
| 接続のタイプ | *電子メールマーケティングの宛先のみ*。 ストレージバケットへの接続タイプを表します。 S3またはFTPを指定できます。 |
| Username | 宛先フローに対して選択したアカウント資格情報。 |
| セグメント | この宛先に対してアクティブ化されているセグメントの数。 |
| ステータス | `Active` または `Inactive`. データが現在この宛先に対してアクティブ化されているかどうかを示します。 ステータスを編集するには、アクティベーションを無効にするを [参照してくださ](/help/rtcdp/destinations/activate-destinations.md#disable-activation)い。 |

目的の行をクリックすると、目的の行に関する詳細情報が右側のレールに表示されます。

![リンク先行をクリック](/help/rtcdp/destinations/assets/click-destination-row.png)

宛先名を選択して、この宛先に対してアクティブ化されたセグメントに関する情報を表示します。 [アクテ **[!UICONTROL ィブ化の編集]** ]をクリックして、この宛先に送信されるセグメントを変更または追加します。

## アカウント {#accounts}

「アカウ **[!UICONTROL ント]** 」タブでは、様々な宛先との接続を確立した場合の詳細を確認できます。 各リンク先で取得できるすべての情報については、次の表を参照してください。

![「アカウント」タブ](/help/rtcdp/destinations/assets/accounts-tab.png)

| 要素 | 説明 |
---------|----------
| プラットフォーム | 接続を設定した宛先。 |
| Username | 接続先ウィザードで選択 [したユーザー名](/help/rtcdp/destinations/email-marketing-destinations.md#connect-destination)。 |
| 流れ | 宛先に対して作成された基本情報に接続された、一意に成功した宛先フローの数を表します。 |
| 認証済み | この宛先への接続が承認された日付。 |

## データフロー {#data-flows}

「デー **[!UICONTROL タフロー]** 」タブには、リアルタイム顧客データプラットフォームで設定したアクティベートフローの図が表示されます。

![Data-flows1](/help/rtcdp/destinations/assets/data-flows1.png)

ページに表示される任意の宛先を選択し、「 **[!UICONTROL View flows]** 」を押して、各宛先に設定したすべてのデータフローに関する情報を表示します。

![Data-flows2](/help/rtcdp/destinations/assets/data-flows2.png)