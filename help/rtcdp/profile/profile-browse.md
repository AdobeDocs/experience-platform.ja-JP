---
keywords: プロファイルの表示 rtcdp;rtcdp プロファイルビュー；rtcdp プロファイル
title: Real-Time Customer Data Platformでのプロファイルの参照
description: Adobe Real-Time Customer Data Platformでは、Adobe Experience Platform ユーザーインターフェイスを使用して、リアルタイム顧客プロファイルデータを参照できます。
feature: Get Started, Profiles
exl-id: 8481e286-2ff0-484f-85d2-a8db9b08d8d3
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 11%

---


# Real-Time Customer Data Platformでのプロファイルの参照

リアルタイム顧客プロファイルは、オンライン、オフライン、CRM、サードパーティデータなど複数のチャネルからのデータを組み合わせて、個々の顧客の全体像を作成します。 個々のプロファイルは様々なソースからシステムに取り込まれるデータに基づいて集計されるので、各プロファイルでは、顧客のブランドとのやり取りがすべて、タイムスタンプ付きで実用的に記述されます。

Adobe Experience Platformのユーザーインターフェイス内では、これらの読み取り専用プロファイルを表示し、環境設定、過去のイベント、インタラクション、個人が属するオーディエンスなど、個々の顧客に関する重要な情報を確認できます。

Adobe Real-Time Customer Data PlatformはAdobe Experience Platformをベースに構築されているので、Experience Platform UI のプロファイル表示機能を利用できます。 Experience Platform ユーザーインターフェイス内で顧客プロファイルを表示する詳細なガイドについては、[ リアルタイム顧客プロファイルユーザーガイド ](../../profile/ui/user-guide.md) を参照してください。

## Real-Time CDP B2B Edition のプロファイルの機能強化

Adobe Experience Platform、Real-Time CDPでサポートされているプロファイル参照機能に加えて、B2B edition ユーザーは、それぞれ「属性」タブと [!UICONTROL  イベント ] タブで、顧客プロファイル内の B2B 属性と [!UICONTROL  イベント ] にアクセスできます。 B2B データを使用してセグメント化を実行することもできます。これらのオーディエンスは、B2B 以外のオーディエンスと共に顧客の [!UICONTROL  オーディエンスメンバーシップ ] タブの下に表示されます。

Real-Time CDP、B2B editionを使用すると、個々の顧客に関連付けられた、全社のソースから [!UICONTROL Accounts]、[!UICONTROL Opportunity]、[!UICONTROL Source レコード ] を参照することもできます。

これらの機能強化を確認するには、まず [ リアルタイム顧客プロファイルユーザーガイド ](../../profile/ui/user-guide.md) で説明されている手順に従って、結合ポリシーまたは ID 名前空間でプロファイルを参照します。

![](images/b2b-browse-profile.png)

プロファイルの詳細には、顧客プロファイルに用意されている標準情報に加えて、[!UICONTROL  アカウント ]、[!UICONTROL  商談 ]、[!UICONTROL Source レコード ] の各タブへのアクセスが含まれています。これらの情報は、B2B のイベントと属性にも拡張されています。

![](images/b2b-profile-detail.png)

Experience Platform UI に表示されるプロファイルの詳細について詳しくは、[ プロファイルダッシュボードのドキュメントの詳細 ](../../dashboards/guides/profiles.md#browse-profiles) を参照してください。

### 「アカウント」タブ

**[!UICONTROL アカウント]** を選択して、プロファイルに関連するアカウントのリストを表示します。 このリストには、アカウントの名前、Web サイト、業界など、アカウントプロファイルの基本情報や、アカウントプロファイルへのリンクが含まれます。

アカウントプロファイルの表示と調査について詳しくは、まず [ アカウントプロファイルの概要 ](../accounts/account-profile-overview.md) をお読みください。

![](images/b2b-profile-accounts.png)

### 「機会」タブ

「**[!UICONTROL 機会]**」タブには、アカウントに関連するオープンな機会およびクローズされた機会に関する詳細が表示されます。 これらのオポチュニティは複数のソースからExperience Platformに取り込むことができますが、Real-Time CDPのB2B editionを使用すると、マーケターは、これらのオポチュニティをすべて 1 か所で簡単に確認できます。

各機会には、機会の名前、その金額、ステージ、機会がオープン、クローズ、成立、不成立のどれであるかなどの情報が含まれます。

![](images/b2b-profile-opportunities.png)

### 「Source レコード」タブ

「**[!UICONTROL Source レコード]**」タブを使用すると、1 つの顧客プロファイルに関与している複数のソースレコードを、エンタープライズソースから簡単に確認できます。 [!UICONTROL  人物ソースキー ] とメールアドレスに加えて、各ソースレコードは、レコードのタイプ（「連絡先」や「リード」レコードなど）とソースも提供します。

![](images/b2b-profile-source-records.png)
