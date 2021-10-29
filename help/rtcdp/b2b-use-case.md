---
keywords: RTCDP;CDP;Real-time Customer Data Platform;リアルタイムの顧客データプラットフォーム;リアルタイム cdp;cdp;rtcdp
title: Real-time Customer Data Platform B2B Edition のユースケースの例 （ベータ版）
description: このサンプルシナリオは、Real-time Customer Data Platform B2B Edition の実装の構成例を示します。
exl-id: 15505980-ac33-44b2-8989-c08cbabd212b
source-git-commit: 6f421a8ae77318ca2598d640cf7e27ea485ec9db
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 100%

---

# Real-time Customer Data Platform B2B Edition のユースケースの例 （ベータ版）

>[!IMPORTANT]
>
>Real-time CDP B2B Edition は現在ベータ版です。ドキュメントと機能は変更される場合があります。

Real-time Customer Data Platform B2B Edition は、既存の Real-time CDP および Adobe Experience Platform 製品を拡張して、B2B データおよびワークフローをサポートします。このドキュメントでは、B2B Edition が提供するさらなるメリットを示すユースケースの例について説明します。次のようなものがあります。

- 様々なサイロ化されたデータソースからの個人データとアカウントデータを組み合わせて、顧客の理解を深め、より正確なセグメント化を可能にする包括的なビューを作成します。詳しくは、様々な B2Bソースで使用するための [XDM スキーマ関係の作成](./schemas/b2b.md)に関するドキュメントを参照してください。
- 関連するエンティティの属性に基づいてオーディエンスをセグメント化します。これには、アカウント、オポチュニティ、キャンペーンおよびマーケティングリストが含まれます。セグメントは、ユーザー属性とエクスペリエンスイベントだけに限定されなくなりました。B2B 固有のオーディエンスを作成するその他の例については、[B2B セグメント化のドキュメント](./segmentation/b2b.md)を参照してください。
- 複数のアカウントに関連する 1 ユーザーのユースケースをネイティブにサポートします。

## 使用例

テクノロジー企業の Bodea には新製品があり、メールと LinkedIn の広告キャンペーンで同時に顧客をターゲットにしたいと考えています。Bodea は、マーケティングキャンペーンの効率を最大化するために、以前に製品に 100 万ドル以上を費やし、先月新しい製品ページにアクセスした既存のアカウントに関連付けられた人物もターゲットにしたいと考えています。

ただし、Bodea には 2 つの異なるビジネスがあります。Bodea の最初のビジネス部門である「Line 1」は、自動車業界向けのソフトウェアを作成しています。2 番目のビジネス部門である「Line 2」は、自動車部品を製造する 3D プリンターを販売しています。Bodea の 2 つのビジネス部門の結果として、Bodea の顧客アカウントから生成された収益データは単一のビューに統合されていません。

各ビジネス部門には、「CRM 1」と「CRM 2」という独自の販売システムがあります。これらの CRM 販売システムは両方とも、独自のマーケティング自動処理プラットフォーム「Marketo 1」および「Marketo 2」に接続されています。CRM 1 からのデータは Marketo 1 にのみ同期され、CRM 2 からのデータは Marketo 2 にのみ同期されます。最終的に、それらのデータは様々な企業情報サイロに保持されます。

<!-- ![lines of business diagram](./assets/lines-of-business.png) -->

## 現在のデータ状況

Bodea の両方のビジネス部門が Townsend Company に売却されるため、Townsend のビジネスデータは各販売システムの 2 つの個別のアカウントとして記録されます。

Marketo 1 では、Townsend はアカウント 1 として記録されます。アカウント 1 には 2 人の関係者（p1@townsend.com と p2@townsend.com）と、CRM 1 での 200,000 ドル（「Opportunity 1」）の 1 件の受注案件があります。そのデータは CRM 1 から Marketo 1 に同期されます。

Marketo 2 では、Townsend はアカウント 2 として記録されます。アカウント 2 にも 2 人の関係者（p2@townsend.com と p3@townsend.com）と、CRM 2 で 900,000 ドル（「Opportunity 2」）の 1 件の受注案件があります。そのデータは CRM 2 から Marketo 2 に同期されます。

統合と追加の企業管理の目的で、Bodea にはマスターデータ管理（MDM）システムもあり、Marketo 1（および CRM 1）のアカウント 1 と Marketo 2（および CRM 2）のアカウント 2 が同じ会社であることを示すレコードを保持しています。

先月、`p2@townsend.com` が新製品ページにアクセスし、Marketo 1 によって web 訪問が記録されました。

![アカウント情報図](./assets/account-info.png)

## 問題

Line 1 は新しいソフトウェア製品をリリースしたばかりで、Bodea の既存の大手顧客にアップセルしたいと考えています。Bodea は、その特定のターゲットオーディエンスを念頭に置いたマーケティングキャンペーンを開始します。

関連する Townsend 情報は Marketo 1 のアカウント 1 および Marketo 2 のアカウント 2 として記録されているため、Bodea のマーケティングチームはサイロ化された情報を効率的に利用できません。

そのためBodea のマーケティングチームは、効率的にこれらの特定の企業を新しい製品のターゲットにすることができなくなります。

これまで、Townsend は、すべてのアカウントで Bodea 製品に累積で 100 万ドル以上を費やしてきました。ただし、古いシステムを使用して作成されたセグメントには、単一の販売システム内で費やされた合計が 100 万ドルを超えない限り、Townsend の誰も含まれません。これは、収益データが様々な販売システムのアカウントにサイロ化されているためです。

Townsend の支出は様々な販売システムに分割されており、個別に合計で 100 万ドルを超えることはないため、このセグメントでは Marketo 1 または Marketo 2 のいずれかに適格な人物は見つかりません。

### Real-time CDP B2B Edition が問題を解決する方法

Real-time CDP B2B エディションを使用すると、Bodea のマーケティングチームは次のことができるようになります。

- すべての異なるソース（複数の Marketo や CRM インスタンス、マスターデータ管理）からのデータを Real-time CDP B2B Edition に結合します。

RT-CDP B2B Edition を使用すると、Bodea は Marketo Engage Source Connector を使用して、Marketo 1 および Marketo 2 からの B2B データを Experience Platform に取り込み、プラットフォームに接続されたアプリケーションを使用してこのデータを最新の状態に保つことができます。詳しくは、[Marketo ソースコネクタ](../sources/connectors/adobe-applications/marketo/marketo.md)ドキュメントを参照してください。

CRM1 からの B2B データ（人物、アカウント、機会およびアクティビティ）が Marketo 1 に同期されます。同様に、CRM2 からのすべての B2B データは Marketo 2 に同期されます。それらのデータは、Marketo ソースコネクタを介して Adobe Experience Platform に同期されます。ただし、Bodea が CRM から Experience Platform に追加のデータを取り込む場合は、既存の CRM コネクタを使用できます。

簡単にするため、またこの例の目的のために、人物はメールで識別されています。この例の結合されたアカウントデータは次のようになります。

| People |
|---|
| p1@townsend.com |
| p2@townsend.com（先月新製品ページにアクセスした人物） |
| p3@townsend.com |

| 商談（受注案件） |
|---|
| 商談 1、20 万ドル |
| 商談 2、90 万ドル |

- 様々なマーケティングイニシアチブのために、この集計データを使用して一意のセグメントを作成します。この例では、セグメントは次のようなすべての人物を検索します。

   - すべてのアカウントで、関連する商談の価値が 100 万ドルを超える
   - おび
   - 先月製品ページにアクセスしました

- Bodea の新しいマーケティングキャンペーンの最も効率的な受け手となるオーディエンスを作成します。この例では、RT-CDP B2B Edition は、マーケティング担当者がマーケティングキャンペーンの適切なターゲットとして `p2@townsend.com` を特定するのに役立ちます。

Marketo Engage と LinkedIn の宛先を使用することにより、Bodea はマーケティングチーム向けのエンドツーエンドの顧客体験管理（CXM）ソリューションを提供します。Experience Platform で作成されたオーディエンスは、Marketo の宛先にプッシュされ、静的なリストとして表示されます。このオーディエンスは、Marketo マーケティングキャンペーンに自動的に追加されます。同時に、RT-CDP B2B Edition によってオーディエンスを LinkedIn マーケティングキャンペーンに送ることもできます。

## 次の手順

このドキュメントを読むことで、Real-time CDP B2B Edition を使用して解決できる目的と問題の種類が紹介されました。

B2B 固有の機能の理解を深めるために、次のドキュメントをお勧めします。

<!-- PLACEHOLDER Link to B2B tutorial required  -->
- [Real-time Customer Data Platform B2B エディションのソース](./sources/b2b.md)
- [Real-time Customer Data Platform B2B Edition のスキーマ](./schemas/b2b.md)
- [B2B セグメント化の例](./segmentation/b2b.md)
- [アカウントプロファイルの概要](./accounts/account-profile-overview.md)
- [Real-time Customer Data Platform B2B エディションの宛先](./destinations/b2b.md)
- [LinkedIn でマッチしたオーディエンスの宛先を設定](../destinations/catalog/social/linkedin.md)
