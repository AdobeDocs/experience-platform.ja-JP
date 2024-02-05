---
solution: Experience Platform
title: 基本を学ぶ
description: ユースケースプレイブック機能の基本を学ぶ方法を説明します。
role: Admin
exl-id: 1c39792e-49fe-4c5f-9796-fa29f60b7461
source-git-commit: 785e32b27372cef9d23761f648bcbaa431448ce7
workflow-type: tm+mt
source-wordcount: '943'
ht-degree: 15%

---


# 基本を学ぶ

Real-time Customer Data PlatformとAdobe Journey Optimizer向けに設計されたユースケースプレイブックのアカウントを設定する方法を説明します。 主な設定手順は次の 3 つです。

* サンドボックスの作成
* ユーザー権限の設定
* Journey Optimizerチャネルの表示を電子メール、プッシュ、SMS 通知用に設定する (Journey Optimizer Playbook を使用する予定の場合 )

## ユースケースプレイブックの設定 — ビデオチュートリアル {#video}

このビデオでは、Journey Optimizerでサンドボックスを作成し、権限を設定し、E メール、プッシュ、SMS 通知のチャネル面を設定するために必要な手順について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3426987?learn=on)

## 開発サンドボックスを作成 {#create-development-sandbox}

ユースケースプレイブックは、特別なタイプの開発サンドボックスを使用します。 [[!UICONTROL ユースケースプレイブック]](/help/use-case-playbooks/playbooks/overview.md)機能の基本を学び、アクセスするには、下記に示すように、サフィックスに `-ucp` または `-UCP` を含む名前（タイトルではない）を付けて[新しい開発サンドボックスを作成](/help/sandboxes/ui/user-guide.md#create)します（実稼動用サンドボックスを選択しないようにしてください）。

>[!IMPORTANT]
>
>新しい開発サンドボックスを作成する場合は、名前にが含まれていることを確認してください。 `-ucp` または `-UCP` 」と入力します。


![ユースケースプレイブック用の開発サンドボックスの作成](/help/use-case-playbooks/assets/playbooks/get-started/create-sandbox-ucp.png)

これで、 [!UICONTROL Playbooks] の下の左側のレールに [!UICONTROL ユースケースプレイブック].

![サンドボックス作成後の UI でのユースケースプレイブック。](/help/use-case-playbooks/assets/playbooks/get-started/ucp-sandbox-in-ui.png)

上記に示すように、左側のパネルに[!UICONTROL プレイブック]が表示されない場合は、このリンク `https://experience.adobe.com/#/@<YOUR_ORG>/sname:<YOUR_SANDBOX_NAME>/platform/mexp/templates` を使用して直接移動します。リンク内の `<YOUR_ORG>` は組織の名前、`<YOUR_SANDBOX_NAME>` は作成した開発サンドボックスの名前です。

## 必要なアクセス権限をチームに付与 {#grant-access-permissions}

を使い始めるには、以下を実行します。 [!UICONTROL ユースケースプレイブック]マーケティングチームのメンバーは、作成したプレイブックのリストを表示したり、プレイブック自体を作成したりできるように、適切な権限が必要です。

**必要な権限**

必要な権限を追加するには、権限 UI で、新しいユースケースプレイブックサンドボックスを [既に設定済みのロール](/help/access-control/abac/ui/permissions.md#managing-sandboxes-for-role)（他の開発用サンドボックスに使用するものを含む）

![役割用のプレイブックサンドボックスが既に設定されています](/help/use-case-playbooks/assets/playbooks/get-started/permissions-to-existing-roles.png)

**プレイブックの役割を設定します。**

また、を使用して新しいロールを追加することも検討できます。 [必要な権限](/help/access-control/home.md#sandboxes-and-permissions).

[新しい役割の設定](/help/access-control/abac/ui/permissions.md) 必要なプレイブックタスクに必要な権限を持っています。 以下に示すように、役割を作成し、新しいサンドボックスを追加します。

![役割を作成し、サンドボックスに追加する](/help/use-case-playbooks/assets/playbooks/get-started/create-new-role.png)

**プレイブックインスタンスの権限**

ユースケースプレイブックの一部として、スキーマ、オーディエンス、宛先、ジャーニーなど、様々なアセットを作成します。 お客様や他のユーザーは、これらのオブジェクトを作成するのに適切な権限が必要になります。

**スキーマの権限**

スキーマを作成および管理するには、データモデリング権限を利用します。 **[!UICONTROL スキーマを管理]**, **[!UICONTROL スキーマを表示]**, **[!UICONTROL 関係を管理]**, **[!UICONTROL ID メタデータの管理]**

**宛先の権限**

宛先を作成および管理するには、宛先権限を使用します。 **[!UICONTROL 管理]**, **[!UICONTROL 宛先]**, **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL マッピングなしでセグメントをアクティブ化]**, **[!UICONTROL データセットの宛先の管理とアクティブ化]**, **[!UICONTROL 宛先のオーサリング]**.

**ジャーニーの権限**

ジャーニーを作成および管理するには、ジャーニー権限を使用します。 **[!UICONTROL ジャーニー]**, **[!UICONTROL 表示ジャーニー]**, **[!UICONTROL ジャーニーレポートを表示]**, **[!UICONTROL ジャーニー]**, **[!UICONTROL イベント]**, **[!UICONTROL データソースとアクション]**, **[!UICONTROL 表示ジャーニー]**, **[!UICONTROL イベント]**, **[!UICONTROL データソースとアクション、公開ジャーニー]**.

次の図は、プレイブックおよびプレイブックによって生成されたアセットを表示、作成、管理するための推奨権限のスナップショットを示しています。

![プレイブックのすべてのインスタンスの作成に必要なすべての権限項目のスナップショット](/help/use-case-playbooks/assets/playbooks/get-started/permission-snapshot.png)

**ユーザーを役割に追加**

一度 [新しい役割を作成しました](/help/access-control/abac/ui/permissions.md#managing-users-for-role) 上で参照したように、自身をそのユーザーとして追加します。 表示のみのアクセス権を持つ別のユーザーのセットに対して、制限付きアクセス権を持つロールを作成する場合は、これらの権限に関連付けられている必要な表示項目のみを含めます。

## Journey Optimizerでのサンドボックスとチャネルサーフェスの設定 {#configure-channel-surfaces}

組織が [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja)Journey Optimizer用に設計されたプレイブックを使用する場合は、サンドボックスでチャネルプリセットを設定し、メッセージに必要な技術パラメーターを定義する必要があります。 [詳しくは、Adobe Journey Optimizer でのチャネルサーフェスの設定方法を参照してください](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/channel-surfaces.html?lang=ja)。

Journey Optimizerで再生ブックのインスタンスを作成するには、電子メール、プッシュ、SMS 通知のチャネルサーフェスを設定する必要があります。

### E メールチャネルの表面

に移動します。 `Channels` Journey Optimizerインターフェイスの マーケティング E メールとトランザクションメッセージ用に、別々のサブドメインと IP プールを設定します（まだ設定されていない場合）。 これらは、注文確認 E メールなどのトランザクションメッセージがお客様に届くようにするためのベストプラクティスです。 名前、電子メールアドレス、およびその他の設定を入力します。 選択 **送信** をクリックして、マーケティングチャネルの表面を作成します。 ドキュメントを読む [電子メールチャネルのサーフェスを設定する方法](https://experienceleague.adobe.com/docs/journey-optimizer/using/email/configure-email/email-settings.html).

### SMS チャネルサーフェス

SMS チャネルサーフェスを作成するには、まず SMS API の資格情報を作成し、目的のベンダー（例：Sinch）を選択します。 SMS チャネルサーフェスに名前を付け（「SMS マーケティング」など）、設定を選択し、送信者番号を入力します。 選択 **送信** をクリックして、SMS チャネルサーフェスを保存します。 ドキュメントを読む [SMS チャネルサーフェスの設定方法](https://experienceleague.adobe.com/docs/journey-optimizer/using/sms/sms-configuration.html?lang=ja#message-preset-sms).

また、注文の確認などのトランザクションメッセージを含む再生ブックのチャネルも設定します。

### チャンネルサーフェスをプッシュ

アプリのサーフェスがExperience Platformまたはデータ収集インターフェイスのどちらかで設定されていることを確認します。 データコレクション環境では、アプリケーションの表示はこのようになります。

<!-- ![App surfaces in Data collections](/help/use-case-playbooks/assets/playbooks/get-started/.png) -->

次に、アプリケーションサーフェス設定で確認したチャネル、プラットフォーム、アプリを選択します。 選択 **送信** をクリックして、プッシュチャネルサーフェスを作成します。

ドキュメントを読む [プッシュチャネルサーフェスの設定方法](https://experienceleague.adobe.com/docs/journey-optimizer/using/push/push-config/push-configuration.html).

## 次の手順 {#next-steps}

このドキュメントのすべての手順に従ったので、左側のナビゲーションで使用できるユースケースプレイブックを含む開発サンドボックスを作成しておく必要があります。 チームメンバーに、プレイブックの表示と管理、およびアセットの生成に必要な権限を付与する方法についても説明します。 次の手順として、 [適切なプレイブックを見つける](/help/use-case-playbooks/playbooks/discover.md) 君にとっては [そのインスタンスからインスタンスを作成します。](/help/use-case-playbooks/playbooks/create-share-reuse.md).