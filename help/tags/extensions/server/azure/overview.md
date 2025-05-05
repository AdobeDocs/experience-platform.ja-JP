---
title: Microsoft Azure 拡張機能の概要
description: Adobe Experience Platformでのイベント転送用のMicrosoft Azure 拡張機能について説明します。
exl-id: 2337d99d-861e-44e7-94ed-ba21ef28d815
last-substantial-update: 2022-11-23T00:00:00Z
source-git-commit: 1c417744518a7ac7cfb9c65d6af8219dcbc70d46
workflow-type: tm+mt
source-wordcount: '891'
ht-degree: 7%

---

# [!DNL Microsoft Azure] 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

ま [!DNL Microsoft Azure]、[[!DNL Event Hubs]](https://azure.microsoft.com/en-us/products/event-hubs/#overview) は、接続されたデバイスやアプリケーションによって生成された膨大な量のデータを処理および分析できる、拡張性の高いリアルタイムデータ入力サービスです。 データがイベントハブに収集されると、任意のリアルタイム分析プロバイダーまたはバッチ/ストレージアダプターを使用して変換および保存できます。

[!DNL Microsoft Azure] [ イベント転送 ](../../../ui/event-forwarding/overview.md) 拡張機能は、[!DNL Event Hubs] を活用して、イベントをAdobe Experience Platform Edge Networkから [!DNL Azure] に送信して、さらに処理できるようにします。 このガイドでは、拡張機能をインストールし、イベント転送ルールでその機能を使用する方法について説明します。

## 前提条件

この拡張機能を使用するには、[!DNL Event Hubs] へのアクセス権を持つ有効な [!DNL Azure] アカウントが必要です。 また、次の手順に従う前に [ ポータルを使用してイベントハブを作成する  [!DNL Azure]  必要があり ](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-create) す。

## 拡張機能のインストール

Microsoft [!DNL Azure] 拡張機能をインストールするには、データ収集 UI またはExperience PlatformUI に移動し、左側のナビゲーションから **[!UICONTROL イベント転送]** を選択します。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、左側のナビゲーションで **[!UICONTROL 拡張機能]** を選択し、「**[!UICONTROL カタログ]**」タブを選択します。 [!UICONTROL Microsoft Azure] カードを検索し、「**[!UICONTROL インストール]**」を選択します。

![ データ収集 UI で [!UICONTROL Microsoft Azure] 拡張機能に対して選択されている「[!UICONTROL &#x200B; インストール &#x200B;]」ボタン。](../../../images/extensions/server/azure/install.png)

拡張機能には設定プロパティがないので、インストールされた拡張機能のリストにすぐに追加されます。 イベント転送ルールを設定する際に、[!DNL Event Hub] アクションタイプの使用を開始できるようになりました。

## イベント転送ルールの設定 {#rule}

新しいイベント転送ルールの作成を開始し、必要に応じてその条件を設定します。 ルールのアクションを選択する場合、拡張機能で「**[!UICONTROL Microsoft Azure]**」を選択し、アクションタイプで「**[!UICONTROL Event Hubs にデータを送信]**」を選択します。

![ データ収集 UI のルールに対して選択されている [!UICONTROL Event Hubs にデータを送信 &#x200B;] アクションタイプ ](../../../images/extensions/server/azure/select-action-type.png)

右側のパネルが更新され、データの送信方法に関する設定オプションが表示されます。 特に、[!DNL Event Hub] 設定を表す様々なプロパティに [ データ要素 ](../../../ui/managing-resources/data-elements.md) を割り当てる必要があります。

![UI に表示される [!UICONTROL Event Hubs にデータを送信 &#x200B;] アクションタイプの設定オプション ](../../../images/extensions/server/azure/event-hub-details.png)

**[!UICONTROL イベントハブの詳細]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL 名前空間] | [ イベントハブの設定 ](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace) 時に作成した [!DNL Event Hubs] 名前空間の名前。 |
| [!UICONTROL 名前] | イベントハブの名前。 |
| [!UICONTROL SAS 承認規則名 &#x200B;] | [!DNL Event Hubs] 名前空間全体、またはデータの送信先の特定のイベントハブインスタンスの共有アクセス認証ルールの名前。 詳しくは、「[SAS 認証値の取得 ](#sas)」に関する付録の節を参照してください。 |
| [!UICONTROL SAS アクセスキー &#x200B;] | [!DNL Event Hubs] 名前空間全体、またはデータの送信先の特定のイベントハブインスタンスの共有アクセス許可ルールのプライマリキー。 詳しくは、「[SAS 認証値の取得 ](#sas)」に関する付録の節を参照してください。 |
| [!UICONTROL &#x200B; パーティション ID] | [!DNL Event Hubs] を使用すると、[ イベントを特定のパーティションに直接送信 ](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/event-hubs/partitioning-in-event-hubs-and-kafka) できます。 この機能を利用するには、イベントを受け取るパーティションの ID を指定します。 |

{style="table-layout:auto"}

**データ**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; ペイロード &#x200B;] | このフィールドには、[!DNL Event Hubs] に転送されるデータが含まれています。 データは、JSON オブジェクト、文字列、またはデータ要素です。 |

{style="table-layout:auto"}

完了したら、「**[!UICONTROL 変更を保持]**」を選択して、アクションをルール設定に追加します。 ルールの設定が完了したら、「**[!UICONTROL ライブラリに保存]**」を選択します。

最後に、新しいイベント転送 [ ビルド ](../../../ui/publishing/builds.md) を公開して、ライブラリに対する変更を有効にします。

## 次の手順

このガイドでは、[!DNL Microsoft Azure] イベント転送拡張機能を使用して [!DNL Event Hubs] にデータを送信する方法について説明しました。 Experience Platformのイベント転送機能について詳しくは、[ イベント転送の概要 ](../../../ui/event-forwarding/overview.md) を参照してください。

## 付録：SAS 認証値の取得 {#sas}

外部アプリケーションには、[ 共有アクセス署名（SAS） ](https://learn.microsoft.com/en-us/azure/event-hubs/authorize-access-shared-access-signature) を通じて [!DNL Event Hubs] へのアクセスが許可されます。 各 [!DNL Event Hubs] 名前空間およびイベントハブインスタンスには、作成時に自動的に割り当てられるデフォルトの SAS 承認規則がありますが、必要に応じて、各リソースに追加のポリシーを作成することもできます。

[!DNL Azure] 拡張機能を使用して [ イベント転送ルールの設定 ](#rule) を行う場合、データの送信先の名前空間または特定のイベントハブを管理する認証ルールの名前とプライマリキーを指定する必要があります。 これらの値を [!DNL Azure] portal から取得する方法について詳しくは、[!DNL Azure] ドキュメントの次の節を参照してください。

* [ 名前空間の SAS 値  [!DNL Event Hubs]  取得 ](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-get-connection-string#connection-string-for-a-namespace)
* [ 名前空間内の特定のイベントハブの SAS 値を取得する ](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-get-connection-string#connection-string-for-a-specific-event-hub-in-a-namespace)

必要な値が揃ったら、承認規則名を設定入力の文字列として直接指定するか、代わりに文字列型のデータ要素を作成して、それを参照できます。 ただし、プライマリキーは、データのセキュリティを保護するために、ルール設定で指定する前に、まずイベント転送の秘密鍵の内部に含める必要があります。

イベント転送 UI 内で [ 新しいシークレットを作成 ](../../../ui/event-forwarding/secrets.md)、シークレットタイプとして **[!UICONTROL トークン]** を選択します。 トークン値自体には、以前にコピーしたプライマリキーを指定します。 秘密鍵を作成したら、**[!UICONTROL 秘密鍵]** タイプのデータ要素を作成し、リストから [!DNL Event Hubs] 秘密鍵を選択します。 秘密鍵データ要素を設定したら、「**[!UICONTROL SAS アクセスキー]**」フィールドでそのデータ要素を参照できます。
