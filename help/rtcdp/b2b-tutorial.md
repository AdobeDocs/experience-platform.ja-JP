---
keywords: RTCDP;CDP;B2B エディション；Real-time Customer Data Platform；リアルタイム顧客データプラットフォーム；リアルタイム cdp;b2b;cdp
solution: Experience Platform
title: Real-time Customer Data Platform B2B Edition の概要
description: Real-time Customer Data Platform B2B Edition の実装を設定する際の例として、次のシナリオをサンプルとして使用します。
source-git-commit: e6f71954d52e0a998955c3420307417cc011c24d
workflow-type: tm+mt
source-wordcount: '1178'
ht-degree: 1%

---

# Real-time Customer Data Platform B2B Edition の概要

このドキュメントでは、Real-time Customer Data Platform(CDP)B2B Edition を使い始めるための、主要な概念を説明する使用例を使用した、エンドツーエンドの高レベルなワークフローを示します。

Bodea 社は、様々なサイロ化されたデータソースの人とアカウントのデータを組み合わせ、E メールとLinkedInの新製品の広告キャンペーンで顧客を効果的にターゲットにしたいと考えています。 Bodea は、Marketo Engageをマーケティングオートメーションプラットフォームとして使用し、顧客データを含む複数の CRM から B2B 固有のオーディエンスをセグメント化する必要があります。

## はじめに

このチュートリアルワークフローは、デモの一環として、複数のAdobe Experience Platformサービスに基づいています。 従う場合は、次のサービスに関する十分な理解を得ることをお勧めします。

- [エクスペリエンスデータモーダル (XDM)](../xdm/home.md)
- [ソース](../sources/home.md)
- [セグメント化](../segmentation/home.md)
- [宛先](../destinations/home.md)

## データのスキーマの作成

初期設定の一環として、Bodea の IT 部門は、Platform に取り込む際にデータが標準形式に従い、様々な Platform サービスやAdobe Experience Cloud製品 (Adobe AnalyticsやAdobe Targetなど ) で対応可能であることを確認する XDM スキーマを作成する必要があります。

Adobe Experience Platformを使用すると、B2B データソースに必要なスキーマと名前空間を自動的に生成できます。 このツールを使用すると、作成したスキーマで、構造化された再利用可能な方法でデータが記述されるようになります。 フォロー： [B2B 名前空間とスキーマ自動生成ユーティリティのドキュメント](../sources/connectors/adobe-applications/marketo/marketo-namespaces.md) を参照してください。

Adobe Experience Platform UI 内で、Bodea マーケターが **[!UICONTROL スキーマ]** 左側のレールで、その後に **[!UICONTROL 参照]** タブをクリックします。 Marketo Engage自動生成ユーティリティを使用したので、新しい空のスキーマがリストに表示され、すべてに「B2B」というプレフィックスが付きます。

![スキーマワークスペースの「参照」タブ](./assets/b2b-tutorial/empty-b2b-schemas.png)

自動生成ユーティリティは、標準の XDM B2B クラス ( [XDM ビジネスアカウント](../xdm/classes/b2b/business-account.md) および [XDM ビジネスオポチュニティ](../xdm/classes/b2b/business-opportunity.md)) を使用して、基本的な B2B データエンティティを取り込みます。 さらに、これらのクラスで構築された自動生成 B2B スキーマには、高度なセグメント化の使用例を可能にする事前に確立された関係があります。 データ構造に必要な追加のフィールドグループは、UI を通じて簡単に作成できます。 詳しくは、 [XDM UI ガイド、スキーマセクションへのフィールドグループの追加](../xdm/ui/resources/schemas.md#add-field-groups) を参照してください。

>[!NOTE]
> 
>自動ジェネレータユーティリティを使用していない場合、または新しい関係を作成する必要がある場合は、 [B2B スキーマ間の関係の作成](../xdm/tutorials/relationship-b2b.md).

リアルタイム顧客プロファイルは、異なるソースのデータを結合して、主要な B2B エンティティの統合プロファイルを作成します。 プロファイルは 1 つのクラスに基づいて生成されるので、自動生成ユーティリティは、一般的なビジネスユースケースに基づいてスキーマ間の関係を設定します。 その結果、B2B スキーマに基づいてデータを取り込む準備が整いました。

>[!NOTE]
> 
>自動生成ユーティリティでスキーマ用に作成されたデフォルトの ID 名前空間、プライマリキー、関係は、スキーマワークスペース内で簡単に見つけることができます。
>
>![デフォルトのスキーマ ID と関係 UI の表示](./assets/b2b-tutorial/schema-identity-relationship.png)

## データの取り込みExperience Platform

次に、ボディアマーケティング担当者は [Marketo Engageコネクタ](../sources/connectors/adobe-applications/marketo/marketo.md) を使用して、ダウンストリームサービスで使用するデータを Platform に取り込みます。 また、Real-time CDP B2B Edition 用の承認済みソースの 1 つを使用して、データを取り込むこともできます。

>[!NOTE]
> 
>組織で使用可能なソースコネクタを確認するには、Platform UI でソースカタログを表示します。 カタログにアクセスするには、 **ソース** 左側のナビゲーションで、「 **カタログ**.

Marketoアカウントと Platform の間の接続を作成するには、認証資格情報を取得する必要があります。 詳しくは、 [Marketoソースコネクタ認証資格情報の取得に関するガイド](../sources/connectors/adobe-applications/marketo/marketo-auth.md) を参照してください。

認証資格情報を取得した後、Bodea Marketer はMarketoアカウントと Platform IMS 組織の間に接続を作成します。 手順については、ドキュメントを参照してください。 [Platform UI を使用したMarketoアカウントの接続方法](../sources/tutorials/ui/create/adobe-applications/marketo.md).

Marketo Engageソースコネクタには、すべてのデータフィールドを新しく作成されたスキーマに簡単にマッピングできるようにする自動マッピング機能が用意されています。

>[!NOTE]
> 
>XDM スキーマでカスタムフィールドグループを作成した場合、プロセスのこの段階でフィールドが未接続になっている可能性があります。 カスタムフィールドグループに入力されるすべての値を確認してください。

Bodea マーケターは、すべてのフィールドグループが適切にマッピングされていることを確認し、データフローを初期化してソース設定プロセスを続行します。 Marketoデータを取り込むデータフローを作成することで、受信データをダウンストリームの Platform サービスで使用できます。 初期の取り込みプロセス中、データはバッチとしてExperience Platformに取り込まれます。 その後、その後、取り込んだデータは、ほぼリアルタイムで更新され、プロファイルにストリーミングされます。

## データを評価するセグメントを作成する

次のタスクでは、ソースデータ内の関連エンティティの特定の属性に基づいて、Bodea の新しい電子メールマーケティングキャンペーンのオーディエンスを作成します。 Platform UI 内で、Bodea マーケターが最初に選択します **[!UICONTROL セグメント]** 左のナビゲーションで、 **[!UICONTROL セグメントを作成]**.

この例では、セールス部門で働くすべての人物が検索され、少なくとも 1 つのオープンなオポチュニティを持つ任意のアカウントに関連しています。 このセグメントでは、XDM Individual Profile クラス、XDM Business Account クラス、XDM Business Opportunity クラス間のリンクが必要です。

![使用例セグメント](./assets/b2b-tutorial/use-case-segment.png)

>[!NOTE]
> 
>データを評価するセグメントを作成する方法については、 [セグメントビルダー UI ガイド](../segmentation/ui/segment-builder.md). B2B セグメント化の具体的な使用例については、 [リアルタイム CDP B2B エディションのセグメント化の概要](./segmentation/b2b.md).

セグメントビルダーを使用すると、リアルタイム顧客プロファイルデータからマーケティング可能なオーディエンスを作成し、定義した属性、イベントおよび既存のオーディエンスの組み合わせに基づいて、見込みオーディエンスの推定を表示できます。

## 宛先に対して評価済みデータをアクティブ化する

セグメントが正常に作成されると、概要が [!UICONTROL 詳細] ワークスペースの「 」セクションに表示されます。 現在、セグメントに対してアクティブ化されている宛先がないので、Bodea マーケターは、オーディエンスにアクセスして操作できるデータセットにオーディエンスを書き出す必要があります。

内 [!UICONTROL セグメント] Platform UI のワークスペースで、Bodea マーケターが **[!UICONTROL 宛先に対して有効化]**.

![宛先へのセグメントのアクティブ化](./assets/b2b-tutorial/activate-to-destination.png)

>[!NOTE]
> 
>に関するチュートリアルを参照してください。 [宛先へのセグメントのアクティブ化](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-cloud-segment-to-a-marketo-static-list.html) を参照してください。

Bodea マーケターは、セグメントをMarketoの宛先に対してアクティブ化し、Platform からセグメントデータを静的リストの形式でMarketo Engageにプッシュできます。 詳しくは、 [Marketoの宛先](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/adobe/marketo-engage.html?lang=ja) を参照してください。

## 次の手順

このチュートリアルに従うことで、Real-time CDP B2B Edition で使用される様々なAdobe Experience Platformサービスを正常に活用できます。 その結果、B2B データを、様々なチャネルをまたいで関与できる実用的なオーディエンスとして取得、セグメント化、評価および書き出す方法を学習しました。
