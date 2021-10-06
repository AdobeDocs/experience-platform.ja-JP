---
keywords: プロファイル rtcdp、rtcdp プロファイルビュー、rtcdp プロファイルの表示
title: Real-time Customer Data Platformでのプロファイルの参照
description: Real-time Customer Data Platformでは、Adobe Experience Platformユーザーインターフェイスを使用して、リアルタイム顧客プロファイルデータを参照できます。
exl-id: 8481e286-2ff0-484f-85d2-a8db9b08d8d3
source-git-commit: 6579e371a8729e926b7061418c786150a27d4876
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 12%

---


# Real-time Customer Data Platformでのプロファイルの参照

リアルタイム顧客プロファイルは、オンライン、オフライン、CRM、サードパーティデータなど複数のチャネルからのデータを組み合わせて、個々の顧客の全体像を作成します。個々のプロファイルは様々なソースからシステムに取り込まれるデータに基づいて集計されるので、各プロファイルでは、顧客のブランドとのやり取りがすべて、タイムスタンプ付きで実用的に記述されます。

Adobe Experience Platformユーザーインターフェイス内で、これらの読み取り専用プロファイルを表示し、環境設定、過去のイベント、インタラクション、個人が属するセグメントなど、個々の顧客に関する重要な情報を確認できます。

Real-time Customer Data PlatformはAdobe Experience Platform上に構築されており、Experience PlatformUI のプロファイル表示機能を利用できます。 Platform ユーザーインターフェイス内での顧客プロファイルの表示に関する詳細なガイドについては、『[ リアルタイム顧客プロファイルユーザーガイド ](../../profile/ui/user-guide.md)』を参照してください。

## リアルタイム CDP、B2B エディションのプロファイルの強化

>[!IMPORTANT]
>
>Real-time Customer Data Platform B2B Edition は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

Adobe Experience Platformでサポートされているプロファイル参照機能に加えて、Real-time CDP、B2B Edition ユーザーは、 [!UICONTROL  属性 ] タブと [!UICONTROL  イベント ] タブで、顧客プロファイル内の B2B 属性とイベントにそれぞれアクセスできます。 B2B データは、セグメント化の実行にも使用できます。これらのセグメントは、非 B2B セグメントの横にお客様の [!UICONTROL  セグメントメンバーシップ ] タブに表示されます。

リアルタイム CDP、B2B エディションでは、個々の顧客に関連付けられた企業ソース全体から [!UICONTROL  アカウント ]、[!UICONTROL  オポチュニティ ]、[!UICONTROL  ソースレコード ] を参照することもできます。

これらの機能強化を調べるには、『[ リアルタイム顧客プロファイルユーザーガイド ](../../profile/ui/user-guide.md)』で説明されている手順に従って、結合ポリシーまたは ID 名前空間でプロファイルを参照します。

![](images/b2b-browse-profile.png)

プロファイルの詳細には、顧客プロファイルで提供される標準情報に加え、[!UICONTROL  アカウント ]、[!UICONTROL  オポチュニティ ]、[!UICONTROL  ソースレコード ] の各タブへのアクセスが含まれます。

![](images/b2b-profile-detail.png)

### 「アカウント」タブ

「**[!UICONTROL アカウント]**」を選択して、プロファイルに関連するアカウントのリストを表示します。 このリストには、アカウントの名前、Web サイト、業種など、アカウントプロファイルからの基本情報と、アカウントプロファイルへのリンクが含まれます。

アカウントプロファイルの表示と調査の詳細については、まず [ アカウントプロファイルの概要 ](../accounts/account-profile-overview.md) を参照してください。

![](images/b2b-profile-accounts.png)

### 「オポチュニティ」タブ

「**[!UICONTROL Opportunity]**」タブには、アカウントに関連するオープン・オポチュニティとクローズ・オポチュニティに関する詳細が表示されます。 これらのオポチュニティは複数のソースからExperience Platformに取り込むことができますが、リアルタイム CDP、B2B エディションを使用すると、マーケティング担当者は、これらのオポチュニティをすべて 1 か所で簡単に確認できます。

各オポチュニティには、オポチュニティの名前、その金額、ステージ、オポチュニティがオープン、クローズ、ウォン、ロストのどれであるかなどの情報が含まれます。

![](images/b2b-profile-opportunities.png)

### 「ソースレコード」タブ

「**[!UICONTROL ソースレコード]**」タブを使用すると、単一の顧客プロファイルに貢献している企業ソースの複数のソースレコードを簡単に確認できます。 [!UICONTROL  ユーザーのソースキー ] と電子メールアドレスに加えて、各ソースレコードには、レコードのタイプ（「連絡先」や「リード」レコードなど）とソースも表示されます。

![](images/b2b-profile-source-records.png)
