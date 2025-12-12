---
title: AWS拡張機能の概要
description: Adobe Experience Platformでのイベント転送用のAWS拡張機能について説明します。
exl-id: 826a96aa-2d64-4a8b-88cf-34a0b6c26df5
last-substantial-update: 2022-11-23T00:00:00Z
source-git-commit: 44e2b8241a8c348d155df3061d398c4fa43adcea
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 2%

---

# [!DNL AWS] 拡張機能の概要

[[!DNL Amazon Web Services]  （[!DNL AWS]） ](https://aws.amazon.com/jp/) は、分散コンピューティング、データベースストレージ、コンテンツ配信、顧客関係管理（CRM）やエンタープライズリソースプランニング（ERP）向けの SaaS （software-as-a-service）統合サービスなど、様々なサービスを提供するクラウドコンピューティングプラットフォームです。

[!DNL AWS] [ イベント転送 ](../../../ui/event-forwarding/overview.md) 拡張機能は、[[!DNL Amazon Kinesis Data Streams]](https://docs.aws.amazon.com/streams/latest/dev/introduction.html) を活用して、イベントをAdobe Experience Platform Edge Networkから [!DNL AWS] に送信して、さらに処理できるようにします。 このガイドでは、拡張機能をインストールし、イベント転送ルールでその機能を使用する方法について説明します。

## 前提条件

この拡張機能を使用するには、既存の [!DNL AWS] データストリームを持つ [!DNL Kinesis] アカウントが必要です。 既存のデータストリームがない場合は、[!DNL AWS] 管理コンソールを使用した新しいデータストリームの作成 [ に関する  [!DNL AWS]  のドキュメントを参照し ](https://docs.aws.amazon.com/streams/latest/dev/how-do-i-create-a-stream.html) ください。

## 拡張機能のインストール {#install}

[!DNL AWS] 拡張機能をインストールするには、データ収集 UI またはExperience Platform UI に移動し、左側のナビゲーションから「**[!UICONTROL Event Forwarding]**」を選択します。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、左側のナビゲーションで「**[!UICONTROL Extensions]**」を選択し、「**[!UICONTROL Catalog]**」タブを選択します。 [!UICONTROL AWS] カードを検索し、「**[!UICONTROL Install]**」を選択します。

![ データ収集 UI の [!UICONTROL Install] 拡張機能に対して選択されている「[!UICONTROL AWS]」ボタン ](../../../images/extensions/server/aws/install.png)

次の画面で、[!DNL AWS] アカウントの接続資格情報を指定する必要があります。 特に、[!DNL AWS] アクセスキー ID と秘密アクセスキーを指定する必要があります。 これらの値がわからない場合は、[!DNL AWS] のドキュメント [ アクセスキー ID と秘密アクセスキーを取得する方法 ](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-appendix-sign-up.html) を参照してください。

![ 拡張機能の設定ビューに追加されたアクセスキー ID と秘密アクセスキー。](../../../images/extensions/server/aws/credentials.png)

>[!IMPORTANT]
>
>アクセス資格情報の生成に使用する [!DNL AWS] アカウントにアクセスポリシーを添付する必要があります。 このポリシーは、[!DNL Kinesis] のデータストリームにデータを送信するためのアクセス権を付与するように設定する必要があります。 ポリシーの定義方法については、**のポリシーの例**[!DNL AWS] に関する [ ドキュメントの  [!DNL Kinesis Data Streams] 例 2](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html#kinesis-using-iam-examples) を参照してください。

終了したら、「**[!UICONTROL Save]**」を選択すると、拡張機能がインストールされます。

## イベント転送ルールの設定 {#rule}

拡張機能をインストールした後、新しいイベント転送 [ ルール ](../../../ui/managing-resources/rules.md) を作成し、必要に応じてその条件を設定します。 ルールのアクションを設定する場合は、**[!UICONTROL AWS]** 拡張機能を選択してから、アクションタイプとして「**[!UICONTROL Send Data to Kinesis Data Stream]**」を選択します。

![ データ収集 UI でルールに対して選択されている [!UICONTROL Send Data to Kinesis Data Stream] アクションタイプ ](../../../images/extensions/server/aws/select-action-type.png)

右側のパネルが更新され、データの送信方法に関する設定オプションが表示されます。 特に、[ 設定を表す様々なプロパティに ](../../../ui/managing-resources/data-elements.md) データ要素 [!DNL Event Hub] を割り当てる必要があります。

![UI に表示される [!UICONTROL Send Data to Kinesis Data Stream] アクションタイプの設定オプション ](../../../images/extensions/server/aws/data-stream-details.png)

**[!UICONTROL Kinesis Data Stream Details]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL Stream Name] | このイベント転送ルールがデータレコードを送信するストリームの名前。 |
| [!UICONTROL AWS Region] | [!DNL AWS] データストリームが作成される [!DNL Kinesis] 領域。 |
| [!UICONTROL Partition Key] | データ ストリームにデータを送信するときに拡張機能が使用する [ パーティション キー ](https://docs.aws.amazon.com/streams/latest/dev/key-concepts.html#partition-key)。<br><br>[!DNL Kinesis Data Streams] は、ストリームに属するデータレコードを複数のシャードに分離します。 各データレコードと共に送信されるパーティションキーを使用して、特定のデータレコードが属するシャードを判断します。<br><br> 顧客ごとに異なるので、顧客を配布するための良いパーティションキーは、顧客番号かもしれません。 パーティション キーが不適切な場合、近くの同じエリアに住んでいる可能性があるため、郵便番号が異なる可能性があります。 一般に、パーティションキーは異なる可能性のある値の範囲が最も大きいものを選択します。 パーティションキーの管理のベストプラクティスについては、[!DNL AWS] データストリームのスケーリング [ に関する  [!DNL Kinesis]  の記事を参照してください ](https://aws.amazon.com/blogs/big-data/under-the-hood-scaling-your-kinesis-data-streams/) |

{style="table-layout:auto"}

**[!UICONTROL Data]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL Payload] | このフィールドには、[!DNL Kinesis] データストリームに転送されるデータが JSON 形式で格納されます。<br><br> 「**[!UICONTROL Raw]**」オプションでは、JSON オブジェクトを指定されたテキストフィールドに直接貼り付けることができます。または、データ要素アイコン（![ データセットアイコン ](/help/images/icons/database.png)）を選択して、ペイロードを表す既存のデータ要素のリストから選択します。<br><br>**[!UICONTROL JSON Key-Value Pairs Editor]** オプションを使用して、UI エディターから各キーと値のペアを手動で追加することもできます。 各値は、生の入力で表すことも、代わりにデータ要素を選択することもできます。 |

{style="table-layout:auto"}

終了したら、「**[!UICONTROL Keep Changes]**」を選択して、アクションをルール設定に追加します。 ルールの設定が完了したら、「**[!UICONTROL Save to Library]**」を選択します。

最後に、新しいイベント転送 [ ビルド ](../../../ui/publishing/builds.md) を公開して、ライブラリに対する変更を有効にします。

## 次の手順

このガイドでは、[!DNL Kinesis Data Streams] イベント転送拡張機能を使用して [!DNL AWS] にデータを送信する方法について説明しました。 Experience Platformのイベント転送機能について詳しくは、[ イベント転送の概要 ](../../../ui/event-forwarding/overview.md) を参照してください。
