---
keywords: Experience Platform;profile;real-time customer profile;user interface;UI;customization;profile dashboard;dashboard
title: プロファイルダッシュボード
description: 'このガイドでは、Adobe Experience PlatformUIで利用できるリアルタイム顧客プロファイルデータダッシュボードについて説明します。 '
topic: guide
type: Documentation
translation-type: tm+mt
source-git-commit: 983b357f2f17aad273f0465dc9250240a062dcd2
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 1%

---


# アルファ [!DNL Real-time Customer Profile] ダッシュボード {#profile-dashboard}

>[!IMPORTANT]
>
>このドキュメントで説明されているダッシュボード機能は、現在アルファベットで表示されており、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

Adobe Experience Platformユーザーインターフェイス(UI)は、毎日のスナップショット時に取り込まれる、データに関する重要な情報を表示できるダッシュボードを提供します。 [!DNL Real-time Customer Profile] このガイドでは、UIの [!DNL Profile] ダッシュボードにアクセスして操作する方法と、ダッシュボードに表示される指標に関する詳細について説明します。

Experience Platformユーザーインターフェイスに含まれるすべてのプロファイル機能の概要については、 [リアルタイムプロファイルUIガイドを参照してください](user-guide.md)。

## プロファイルダッシュボードデータ

プロファイルダッシュボードには、Experience Platform内のプロファイルストア内に組織が持つ属性（レコード）データのスナップショットが表示されます。 スナップショットには、イベント（時系列）データは含まれません。

スナップショット内の属性データは、スナップショットが作成された特定の時点で表示されるデータとまったく同じ内容を示します。 つまり、スナップショットはデータの近似やサンプルではなく、プロファイルダッシュボードはリアルタイムで更新されません。

>[!NOTE]
>
>スナップショットが作成された後にデータに対して行われた変更や更新は、次のスナップショットが作成されるまでダッシュボードに反映されません。

プロファイルダッシュボードに表示される指標は、組織のデフォルトの結合ポリシーに基づいています。 マージポリシーの詳細、およびデフォルトのマージポリシーを選択または変更する方法については、 [マージポリシーのUIガイドを参照してください](merge-policies.md)。

## プロファイルダッシュボードの詳細

Platform UI内のプロファイルダッシュボードに移動するには、左のナビゲーションバーの「 **[!UICONTROL プロファイル]** 」を選択し、「 **[!UICONTROL 概要]** 」タブを選択してダッシュボードを表示します。

![](../images/profile-dashboard/dashboard-overview.png)

### ウィジェットと指標

ダッシュボードはプロファイルで構成され、ウィジェットは読み取り専用の指標で、ウィジェットデータに関する重要な情報を提供します。 ウィジェットの「最終更新日」の日時は、データの最後のスナップショットが作成された日時を示します。

![](../images/profile-dashboard/dashboard-timestamp.png)

## 利用可能なウィジェット

Experience Platformは、プロファイルデータに関連する様々な指標を視覚化するために使用できる複数のウィジェットを提供します。 ウィジェット名を以下から選択して、詳細を確認します。

* [[!UICONTROL オーディエンスサイズ]](#audience-size)
* [[!UICONTROL 名前空間別プロファイル数]](#profiles-by-namespace)

### [!UICONTROL オーディエンスサイズ] {#audience-size}

オーディエンス **[!UICONTROL サイズ]** Widgetは、スナップショットが作成された時点での、プロファイルデータストア内のマージされたプロファイルの合計数を表示します。 この数値は、組織の既定の結合ポリシーがプロファイルデータに適用され、プロファイルフラグメントを結合して個々のプロファイルを1つにするための結果です。

フラグメントと結合されたプロファイルの詳細については、 [プロファイルの概要の「](../home.md#profile-fragments-vs-merged-profiles) プロファイルフラグメントと結合されたプロファイル [」セクションを読み始めてください](../home.md)。

![](../images/profile-dashboard/audience-size.png)

### [!UICONTROL 名前空間別プロファイル数] {#profiles-by-namespace}

名前空間別 **[!UICONTROL プロファイル]** Widgetは、プロファイルストア内の結合されたすべてのプロファイルでの名前空間の内訳を表示します。 1つのプロファイルに複数のプロファイルが関連付けられている場合があるので、 [!UICONTROL ID名前空間別の名前空間の合計数] (つまり、各名前空間に表示される値を合計した数)は、常にマージプロファイルの合計数より多くなります。 例えば、ある顧客が複数のチャネルで自社のブランドとやり取りした場合、複数の名前空間がその個々の顧客に関連付けられます。

ID名前空間の詳細については、 [Adobe Experience PlatformIDサービスのドキュメントを参照してください](../../identity-service/home.md)。

![](../images/profile-dashboard/profiles-by-namespace.png)

## その他のダッシュボード

Platform UIには、Experience Platform内のデータのスナップショットを表示するための追加のダッシュボードが用意されています。 これらのダッシュボードには、セグメント化やライセンスの使用が含まれます。 これらの追加ダッシュボードの詳細については、次のリンクから選択してください。

* [セグメントダッシュボード](../../segmentation/ui/segment-dashboard.md)
* [ライセンス使用ダッシュボード](../../landing/license-usage-dashboard.md)

## 次の手順

このドキュメントに従うと、プロファイルダッシュボードを見つけて、利用可能なウィジェットに表示される指標を理解できるようになります。 Experience PlatformUIでの [!DNL Profile] データの操作について詳しくは、 [[!DNL Profile] UIガイドを参照してください](user-guide.md)。