---
title: Microsoft Azure 拡張機能の概要
description: Adobe Experience Platformでのイベント転送に関するMicrosoft Azure 拡張機能について説明します。
exl-id: 2337d99d-861e-44e7-94ed-ba21ef28d815
source-git-commit: f6c11fadc0d8019044fbdd2923af00ce18ce39e1
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 7%

---

# [!DNL Microsoft Azure] 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

In [!DNL Microsoft Azure], [[!DNL Event Hubs]](https://azure.microsoft.com/en-us/products/event-hubs/#overview) は、拡張性が高く、リアルタイムのデータ入力サービスで、接続されたデバイスやアプリケーションで生成される大量のデータを処理および分析できます。 データがイベントハブに収集されたら、任意のリアルタイム分析プロバイダーまたはバッチ/ストレージアダプターを使用して、変換および格納できます。

この [!DNL Microsoft Azure] [イベント転送](../../../ui/event-forwarding/overview.md) 拡張機能の活用 [!DNL Event Hubs] を追加して、Adobe Experience Platform Edge Network からにイベントを送信します。 [!DNL Azure] を参照してください。 このガイドでは、イベント転送ルールでの拡張機能のインストール方法とその機能の使用方法について説明します。

## 前提条件

この拡張機能を使用するには、有効な [!DNL Azure] ～を利用できる口座 [!DNL Event Hubs]. また、 [イベントハブの作成には、 [!DNL Azure] ポータル](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-create) 次の手順に従う前に、

## 拡張機能のインストール

Microsoft [!DNL Azure] 拡張機能に移動し、データ収集 UI またはExperience PlatformUI に移動して、「 」を選択します。 **[!UICONTROL イベント転送]** をクリックします。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、「 」を選択します。 **[!UICONTROL 拡張機能]** 左側のナビゲーションで、 **[!UICONTROL カタログ]** タブをクリックします。 を検索します。 [!UICONTROL Microsoft Azure] カードを選択し、 **[!UICONTROL インストール]**.

![この [!UICONTROL インストール] ボタンを選択しています [!UICONTROL Microsoft Azure] 拡張機能を使用して、データ収集 UI に追加できます。](../../../images/extensions/server/azure/install.png)

拡張機能には設定プロパティがないので、直ちにインストールされた拡張機能のリストに追加されます。 これで、 [!DNL Event Hub] イベント転送ルールを設定する際のアクションタイプ。

## イベント転送ルールの設定 {#rule}

新しいイベント転送ルールの作成を開始し、必要に応じて条件を設定します。 ルールのアクションを選択する場合は、「 **[!UICONTROL Microsoft Azure]** 拡張機能に対して、「 」を選択します。 **[!UICONTROL イベントハブへのデータ送信]** （アクションタイプ）。

![この [!UICONTROL イベントハブへのデータ送信] データ収集 UI のルールに対して選択されているアクションタイプ。](../../../images/extensions/server/azure/select-action-type.png)

右側のパネルが更新され、データの送信方法に関する設定オプションが表示されます。 特に、 [データ要素](../../../ui/managing-resources/data-elements.md) を [!DNL Event Hub] 設定。

![の設定オプション [!UICONTROL イベントハブへのデータ送信] UI に表示されるアクションタイプ。](../../../images/extensions/server/azure/event-hub-details.png)

**[!UICONTROL イベントハブの詳細]**

| 必要情報 | 説明 |
| --- | --- |
| [!UICONTROL 名前空間] | の名前 [!DNL Event Hubs] 作成した名前空間 [イベントハブのセットアップ](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hubs-namespace). |
| [!UICONTROL 名前] | イベントハブの名前。 |
| [!UICONTROL SAS 認証ルール名] | 全体の共有アクセス認証ルールの名前 [!DNL Event Hubs] 名前空間、またはデータの送信先の特定のイベントハブインスタンス。 付録の [取得， SAS 認証値](#sas) を参照してください。 |
| [!UICONTROL SAS アクセスキー] | 全体の共有アクセス認証ルールのプライマリキー [!DNL Event Hubs] 名前空間、またはデータの送信先の特定のイベントハブインスタンス。 付録の [取得， SAS 認証値](#sas) を参照してください。 |
| [!UICONTROL パーティション ID] | [!DNL Event Hubs] を使用すると、 [特定のパーティションに直接イベントを送信](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/event-hubs/partitioning-in-event-hubs-and-kafka). この機能を利用するには、イベントを受け取るパーティションの ID を指定します。 |

{style=&quot;table-layout:auto&quot;}

**データ**

| 必要情報 | 説明 |
| --- | --- |
| [!UICONTROL ペイロード] | このフィールドには、 [!DNL Event Hubs]. データは、JSON オブジェクト、文字列、データ要素のいずれかです。 |

{style=&quot;table-layout:auto&quot;}

終了したら、「 」を選択します。 **[!UICONTROL 変更を保持]** をクリックして、ルール設定にアクションを追加します。 ルールに問題がない場合は、「 」を選択します。 **[!UICONTROL ライブラリに保存]**.

最後に、新しいイベント転送を公開します [ビルド](../../../ui/publishing/builds.md) ライブラリへの変更を有効にします。

## 次の手順

このガイドでは、にデータを送信する方法について説明しました [!DNL Event Hubs] の使用 [!DNL Microsoft Azure] イベント転送拡張機能。 Experience Platformのイベント転送機能について詳しくは、 [イベント転送の概要](../../../ui/event-forwarding/overview.md).

## 付録：SAS 認証値の取得 {#sas}

外部アプリケーションには、 [!DNL Event Hubs] 経由 [共有アクセス署名 (SAS)](https://learn.microsoft.com/en-us/azure/event-hubs/authorize-access-shared-access-signature). 各 [!DNL Event Hubs] 名前空間とイベントハブのインスタンスには、作成時にデフォルトの SAS 承認ルールが自動的に割り当てられますが、必要に応じて、各リソースに対して追加のポリシーを作成することもできます。

条件 [イベント転送ルールの設定](#rule) の使用 [!DNL Azure] 拡張機能の場合は、データの送信先の名前空間または特定のイベントハブを管理する認証ルールの名前とプライマリキーを指定する必要があります。 これらの値を [!DNL Azure] ポータルでは、 [!DNL Azure] ドキュメント：

* [の SAS 値の取得 [!DNL Event Hubs] 名前空間](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-get-connection-string#connection-string-for-a-namespace)
* [名前空間の特定のイベントハブの SAS 値を取得する](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-get-connection-string#connection-string-for-a-specific-event-hub-in-a-namespace)

必要な値を取得したら、認証ルール名を設定入力内の文字列として直接指定できます。または、代わりに、文字列タイプのデータ要素を作成して参照することもできます。 ただし、データセキュリティを保護するために、プライマリキーは、ルール設定で指定する前に、イベント転送シークレットの内部に含まれている必要があります。

イベント転送 UI 内で、 [新しい秘密を作る](../../../ui/event-forwarding/secrets.md) を選択し、 **[!UICONTROL トークン]** を秘密の型として トークンの値自体に対して、前にコピーしたプライマリキーを指定します。 シークレットを作成したら、タイプのデータ要素を作成します **[!UICONTROL 秘密鍵]** をクリックし、 [!DNL Event Hubs] リストからの秘密。 シークレットデータ要素が設定されたら、そのデータ要素を **[!UICONTROL SAS アクセスキー]** フィールドに入力します。
