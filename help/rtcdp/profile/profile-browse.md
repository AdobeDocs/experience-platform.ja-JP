---
keywords: プロファイルの表示 rtcdp、rtcdp プロファイルビュー、rtcdp プロファイル
title: Real-time Customer Data Platformでのプロファイルの参照
description: Adobe Real-time Customer Data Platformを使用すると、Adobe Experience Platformユーザーインターフェイスを使用してリアルタイム顧客プロファイルデータを参照できます。
exl-id: 8481e286-2ff0-484f-85d2-a8db9b08d8d3
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 15%

---


# Real-time Customer Data Platformでのプロファイルの参照

リアルタイム顧客プロファイルは、オンライン、オフライン、CRM、サードパーティデータなど複数のチャネルからのデータを組み合わせて、個々の顧客の全体像を作成します。個々のプロファイルは様々なソースからシステムに取り込まれるデータに基づいて集計されるので、各プロファイルでは、顧客のブランドとのやり取りがすべて、タイムスタンプ付きで実用的に記述されます。

Adobe Experience Platformユーザーインターフェイス内では、これらの読み取り専用プロファイルを表示し、環境設定、過去のイベント、インタラクション、個人が属するセグメントなど、個々の顧客に関する重要な情報を確認できます。

Adobe Real-time Customer Data PlatformはAdobe Experience Platformをベースに構築されているので、Experience PlatformUI のプロファイル表示機能を利用できます。 Platform ユーザーインターフェイス内での顧客プロファイルの表示に関する詳細なガイドについては、 [リアルタイム顧客プロファイルユーザーガイド](../../profile/ui/user-guide.md).

## Real-Time CDP, B2B Edition のプロファイルの強化

Adobe Experience Platform、Real-Time CDP、B2B Edition のユーザーは、B2B の属性とイベントに、 [!UICONTROL 属性] および [!UICONTROL イベント] タブに設定します。 B2B データは、セグメント化の実行にも使用でき、これらのセグメントは顧客の [!UICONTROL セグメントのメンバーシップ] タブを非 B2B セグメントと一緒に表示します。

Real-Time CDP, B2B Edition では、 [!UICONTROL アカウント], [!UICONTROL 商談]、および [!UICONTROL ソースレコード] 個々の顧客に関連付けられている企業ソース全体から

これらの機能強化を確認するには、まず [リアルタイム顧客プロファイルユーザーガイド](../../profile/ui/user-guide.md) をクリックし、結合ポリシーまたは id 名前空間でプロファイルを参照します。

![](images/b2b-browse-profile.png)

プロファイルの詳細には、 [!UICONTROL アカウント], [!UICONTROL 商談]、および [!UICONTROL ソースレコード] 顧客プロファイルで提供される標準情報に加えて、B2B のイベントと属性が強化されました。

![](images/b2b-profile-detail.png)

### 「アカウント」タブ

選択 **[!UICONTROL アカウント]** をクリックして、プロファイルに関連するアカウントのリストを表示します。 このリストには、アカウントの名前、Web サイト、業種など、アカウントプロファイルからの基本情報と、アカウントプロファイルへのリンクが含まれます。

アカウントプロファイルの表示と調査の詳細については、まず [アカウントプロファイルの概要](../accounts/account-profile-overview.md).

![](images/b2b-profile-accounts.png)

### 「機会」タブ

この **[!UICONTROL 商談]** 「 」タブには、アカウントに関連するオープン商談とクローズ済商談に関する詳細が表示されます。 これらのオポチュニティは複数のソースからExperience Platformに取り込むことができますが、Real-Time CDP, B2B Edition を使用すると、マーケターは、これらのオポチュニティを 1 か所でまとめて簡単に確認できます。

各機会には、機会の名前、その金額、ステージ、機会がオープン、クローズ、成立、不成立のどれであるかなどの情報が含まれます。

![](images/b2b-profile-opportunities.png)

### 「ソースレコード」タブ

この **[!UICONTROL ソースレコード]** 「 」タブを使用すると、単一の顧客プロファイルに貢献するエンタープライズソースからの複数のソースレコードを簡単に確認できます。 また、 [!UICONTROL 担当者ソースキー] また、各ソースレコードには、レコードのタイプ（「連絡先」や「リード」レコードなど）とソースも表示されます。

![](images/b2b-profile-source-records.png)
