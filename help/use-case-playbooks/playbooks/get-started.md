---
solution: Experience Platform
title: ユースケースプレイブックの概要
description: ユースケースプレイブック機能の基本を学ぶ方法を説明します。
role: Admin
exl-id: 1c39792e-49fe-4c5f-9796-fa29f60b7461
source-git-commit: dfb4549a09628f2fed947ddf76167db28354eb92
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 15%

---


# 基本を学ぶ

Real-time Customer Data PlatformおよびAdobe Journey Optimizer向けに設計されたユースケースプレイブック用のアカウントが自動的に設定されない場合は、その設定方法を説明します。 主な設定手順は次の 3 つです。

* サンドボックスの作成
* ユーザー権限の設定
* メール、プッシュ、SMS 通知用のJourney Optimizer チャネルサーフェスの設定（Journey Optimizer プレイブックを使用する予定の場合）

## ユースケースプレイブックの設定 – ビデオチュートリアル {#video}

このビデオでは、サンドボックスの作成、権限の設定、Journey Optimizerでのメール、プッシュおよび SMS 通知用のチャネルサーフェスの設定に必要な手順について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3426987?learn=on)

## 開発サンドボックスを作成 {#create-development-sandbox}

ユースケースプレイブックは、特別なタイプの開発用サンドボックスを使用します。 [[!UICONTROL ユースケースプレイブック]](/help/use-case-playbooks/playbooks/overview.md)機能の基本を学び、アクセスするには、下記に示すように、サフィックスに `-ucp` または `-UCP` を含む名前（タイトルではない）を付けて[新しい開発サンドボックスを作成](/help/sandboxes/ui/user-guide.md#create)します（実稼動用サンドボックスを選択しないようにしてください）。

>[!IMPORTANT]
>
>新しい開発用サンドボックスを作成する場合、名前にが含まれていることを確認します `-ucp` または `-UCP` サフィックス


![ユースケースプレイブック用の開発サンドボックスの作成](/help/use-case-playbooks/assets/playbooks/get-started/create-sandbox-ucp.png)

これで、次のように表示されます [!UICONTROL プレイブック] の下の左側のレールで [!UICONTROL ユースケースプレイブック].

![サンドボックス作成後の UI でのユースケースプレイブック。](/help/use-case-playbooks/assets/playbooks/get-started/ucp-sandbox-in-ui.png)

上記に示すように、左側のパネルに[!UICONTROL プレイブック]が表示されない場合は、このリンク `https://experience.adobe.com/#/@<YOUR_ORG>/sname:<YOUR_SANDBOX_NAME>/platform/mexp/templates` を使用して直接移動します。リンク内の `<YOUR_ORG>` は組織の名前、`<YOUR_SANDBOX_NAME>` は作成した開発サンドボックスの名前です。

## 必要なアクセス権限をチームに付与 {#grant-access-permissions}

を使用するには [!UICONTROL ユースケースプレイブック]マーケティングチームのメンバーは、作成したプレイブックのリストを表示したり、自分でプレイブックを作成したりできるように、適切な権限が必要です。

**必要な権限**

必要な権限を追加するには、権限 UI で、新しいユースケースプレイブックサンドボックスをに含めます [既に設定されている役割](/help/access-control/abac/ui/permissions.md#managing-sandboxes-for-role)（他の開発用サンドボックスに使用される場合を含む）

![役割のプレイブックサンドボックスは既に設定済み](/help/use-case-playbooks/assets/playbooks/get-started/permissions-to-existing-roles.png)

**プレイブックの役割を設定します。**

または、での新しい役割の追加を検討することもできます [必要な権限](/help/access-control/home.md#sandboxes-and-permissions).

[新しい役割の設定](/help/access-control/abac/ui/permissions.md) 必須のプレイブックタスクに必要な権限を持つ。 以下に示すように、役割を作成し、それに新しいサンドボックスを追加します。

![役割を作成してサンドボックスに追加します](/help/use-case-playbooks/assets/playbooks/get-started/create-new-role.png)

**プレイブックインスタンスの権限**

ユースケースプレイブックの一部として、スキーマ、オーディエンス、宛先、ジャーニーなどの様々なアセットを作成します。 自身および他のユーザーは、これらのオブジェクトを作成するための適切な権限が必要になります。

**スキーマの権限**

スキーマを作成および管理するには、データモデリング権限を使用します。 **[!UICONTROL スキーマの管理]**, **[!UICONTROL スキーマの表示]**, **[!UICONTROL 関係の管理]**, **[!UICONTROL ID メタデータの管理]**

**宛先の権限**

宛先を作成および管理するには、宛先権限を使用します。 **[!UICONTROL 管理]**, **[!UICONTROL 宛先]**, **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL マッピングを使用しないセグメントのアクティブ化]**, **[!UICONTROL データセット宛先の管理とアクティブ化]**, **[!UICONTROL 宛先のオーサリング]**.

**ジャーニーの権限**

ジャーニーを作成および管理するには、ジャーニー権限を使用します。 **[!UICONTROL ジャーニーの管理]**, **[!UICONTROL ジャーニーの表示]**, **[!UICONTROL ジャーニーレポートの表示]**, **[!UICONTROL ジャーニーの管理]**, **[!UICONTROL イベント]**, **[!UICONTROL データソースとアクション]**, **[!UICONTROL ジャーニーの表示]**, **[!UICONTROL イベント]**, **[!UICONTROL データソースとアクション、ジャーニーの公開]**.

次の画像は、プレイブックとプレイブックによって生成されたアセットの表示、作成、管理にユーザーに推奨される権限のスナップショットを示しています。

![プレイブックのすべてのインスタンスを作成するために必要なすべての権限項目のスナップショット](/help/use-case-playbooks/assets/playbooks/get-started/permission-snapshot.png)

**役割にユーザーを追加**

一度 [新しい役割を作成しました](/help/access-control/abac/ui/permissions.md#managing-users-for-role) 上記のように、自分自身をユーザーとして追加します。 表示のみのアクセス権を持つ別のユーザーセットに対して、制限付きアクセスの役割を作成する場合、これらの権限に関連付けられている必要なビュー項目のみを含めます。

## Journey Optimizerでのサンドボックスおよびチャネルサーフェスの設定 {#configure-channel-surfaces}

組織がにライセンスを取得している場合 [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja)また、Journey Optimizer用に設計されたプレイブックを使用する場合は、サンドボックスでチャネルプリセットを設定する必要があります。これは、メッセージに必要な技術的パラメーターを定義します。 [詳しくは、Adobe Journey Optimizer でのチャネルサーフェスの設定方法を参照してください](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/channel-surfaces.html?lang=ja)。

Journey Optimizerでプレイブックのインスタンスを作成するには、メール、プッシュ、SMS 通知用のチャネルサーフェスを設定する必要があります。

### メールチャネルサーフェス

に移動 `Channels` Journey Optimizerのインターフェイスで上書きできます。 まだ設定されていない場合は、マーケティングメールとトランザクションメッセージ用に個別のサブドメインと IP プールを設定します。 これらは、注文確認メールなどのトランザクションメッセージを顧客に確実に送信するためのベストプラクティスです。 名前、メールアドレスおよび追加設定を入力します。 を選択 **Submit** ページの右上で、マーケティングチャネルサーフェスを作成します。 のドキュメントを参照してください。 [メールチャネルサーフェスの設定方法](https://experienceleague.adobe.com/docs/journey-optimizer/using/email/configure-email/email-settings.html).

### SMS チャネルサーフェス

SMS チャネルサーフェスを作成するには、まず SMS API 認証情報を作成し、優先ベンダー（Sinch など）を選択します。 SMS チャネルサーフェスに名前を付け（例：SMS マーケティング）、設定を選択し、送信者番号を入力します。 を選択 **Submit** をクリックし、SMS チャネルサーフェスを保存します。 のドキュメントを参照してください。 [sms チャネルサーフェスの設定方法](https://experienceleague.adobe.com/docs/journey-optimizer/using/sms/sms-configuration.html?lang=ja#message-preset-sms).

また、注文確認などのトランザクションメッセージを含むプレイブックのチャネルを設定します。

### チャネルサーフェスをプッシュ

アプリサーフェスがExperience Platformまたはデータ収集インターフェイスのいずれかから設定されていることを確認します。 データ収集環境では、アプリのサーフェスはこのように表示されます。

<!-- ![App surfaces in Data collections](/help/use-case-playbooks/assets/playbooks/get-started/.png) -->

次に、アプリサーフェス設定で確認したチャネル、プラットフォーム、アプリを選択します。 を選択 **Submit** プッシュ チャネルサーフェスを作成します。

のドキュメントを参照してください。 [プッシュチャネルサーフェスの設定方法](https://experienceleague.adobe.com/docs/journey-optimizer/using/push/push-config/push-configuration.html).

## 次の手順 {#next-steps}

このドキュメントのすべての手順を完了したので、左側のナビゲーションにユースケースプレイブックが表示された開発用サンドボックスを作成しました。 また、プレイブックの表示と管理、アセットの生成に必要な権限をチームメンバーに付与する方法についても理解しました。 次の手順として、方法をお読みください [適切なプレイブックを見つける](/help/use-case-playbooks/playbooks/discover.md) その時のために [そこからインスタンスを作成](/help/use-case-playbooks/playbooks/create-share-reuse.md).
