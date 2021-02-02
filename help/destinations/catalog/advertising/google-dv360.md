---
keywords: DoubleClick入札マネージャ；DoubleClick入札マネージャ；DoubleClick;Display & Video 360;Display 360;video 360;Video 360;Display 360;Display 360;display and video
title: Google Display と Video 360 の宛先
seo-title: Google Display と Video 360 の宛先
description: Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。
seo-description: 'Display & Video 360（旧称 DoubleClick Bid Manager）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。 '
translation-type: tm+mt
source-git-commit: bb2fc2658d32c59b476dd9d526eb8bc2f055a1af
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 46%

---


# [!DNL Google Display & Video 360] 宛先

## 概要

[!DNL Display & Video 360]（旧称 ）は、ディスプレイ広告、ビデオ、モバイルの在庫ソースをまたいで、再ターゲティングと、オーディエンスにターゲットを絞ったデジタルキャンペーンの実行に使用できるツールです。[!DNL DoubleClick Bid Manager]

## 宛先の仕様

[!DNL Google Display & Video 360]宛先に固有の次の詳細を確認します。

* 次の[ID](../../../identity-service/namespaces.md)を[!DNL Google Ads]宛先に送信できます。[AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=en)、Google cookie ID、IDFA、GAID、Roku ID、Microsoft ID、Amazon Fire TV ID。
   * Googleは、カリフォルニア州のユーザーをターゲットする場合は[AAM UUID](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/ids-in-aam.html?lang=en)を使用し、その他すべてのユーザーの場合はGoogle Cookie IDを使用します。
* アクティブ化されたオーディエンスは、Google プラットフォームでプログラムを使用して作成されます。
* 現在、プラットフォームには、アクティベーションの成功を検証するための測定指標は含まれていません。 統合を検証し、オーディエンスターゲティングの規模を理解するには、Google でのオーディエンス数を参照します。

>[!IMPORTANT]
>
>Google Display &amp; Video 360 を使用して最初の宛先を作成する場合で、以前に Experience Cloud ID サービスにおいて、Adobe Audience Manager や他のアプリケーションとの間で [ID 同期機能](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/idsync.html)を有効にしていないときには、アドビコンサルティングまたはカスタマーケアに連絡して、ID 同期を有効にしてください。以前にAudience ManagerでGoogle統合を設定していた場合、Platformへの繰り越しを設定したID同期。

### エクスポートの種類{#export-type}

**セグメントエクスポート**  — セグメント(オーディエンス)のすべてのメンバーをGoogleのエクスポート先にエクスポートします。

## 前提条件

### 許可リスト

>[!NOTE]
>
>この許可リストは、Platformで最初の[!DNL Google Display & Video 360]宛先を設定する前に必須です。 リンク先を作成する前に、Googleが以下に説明する許可リストプロセスを完了していることを確認してください。

Platformで[!DNL Google Display & Video 360]宛先を作成する前に、Googleに連絡して、許可されているデータプロバイダーのリストにAdobeし、許可リストにアカウントを追加するように求める必要があります。 Google に連絡し、次の情報を提供します。

* **アカウント ID**：これは、アドビの Google アカウント ID です。アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **顧客 ID**：これは、アドビの Google 顧客アカウント ID です。アドビカスタマーケアまたは Adobe 担当者に問い合わせて、この ID を取得してください。
* **アカウントの種類**：**[!DNL Invite advertiser]** を使用して、Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するか、**[!DNL Invite partner]** を使用して Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有します。

## 宛先の設定

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、[!DNL Google Display & Video 360]を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![Google Display &amp; Video 360 の宛先への接続](../../assets/catalog/advertising/google-dv360/catalog.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「[!UICONTROL アクティブ化]」と「[!UICONTROL 設定]」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

宛先を作成ワークフローの&#x200B;**セットアップ**&#x200B;手順で、宛先の[!UICONTROL 基本情報]と、この宛先に適用するマーケティングの使用例を入力します。

![Google Display &amp; Video 360 の基本情報](../../assets/catalog/advertising/google-dv360/setup.png)

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL アカウントの種類]**：Google のアカウントに応じて、次のオプションを選択します。
   * Display &amp; Video 360 アカウントにある特定のブランドにのみオーディエンスを共有するには、`Invite Advertiser` を使用します。
   * Display &amp; Video 360 アカウントのすべてのブランドにオーディエンスを共有するには、`Invite Partner` を使用します。
* **[!UICONTROL アカウント ID]**：**[!DNL Invite partner]** または **[!DNL Invite advertiser]** のアカウント ID に Google アカウントの ID を入力します。通常、これは 6 桁または 7 桁の ID です。
* **[!UICONTROL マーケティングの使用例]**:マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例について詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

>[!NOTE]
>
>[!DNL Google Display & Video 360]宛先を設定する際は、[!DNL Google Account Manager]またはAdobeの担当者に相談して、お持ちのアカウントのタイプをご確認ください。

## セグメントを[!DNL Google Display & Video 360]にアクティブ化

[!DNL Google Display & Video 360]にセグメントをアクティブ化する方法については、[宛先へのデータのアクティブ化](../../ui/activate-destinations.md)を参照してください。

## 書き出されたデータ

データが[!DNL Google Display & Video 360]宛先に正常にエクスポートされたかどうかを確認するには、[!DNL Google Display & Video 360]アカウントを確認してください。 アクティベーションに成功すると、オーディエンスがアカウントに入力されます。