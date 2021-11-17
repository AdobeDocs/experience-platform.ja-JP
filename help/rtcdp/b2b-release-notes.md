---
product: adobe experience platform
solution: Experience Platform, Real-time Customer Data Platform
title: Adobe Experience Platform Real-time Customer Data Platform B2B Edition リリースノート
description: Adobe Experience Platform Real-time Customer Data Platform B2B Edition の最新のリリースノートです。
source-git-commit: b6bcd94130126d1d893621ae1ab4255c8a789a80
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 22%

---

# Adobe Experience Platform Real-time Customer Data Platform B2B Edition リリースノート

**リリース日：2021 年 11 月 12 日**

## Real-time Customer Data Platformの更新

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

Real-time CDP B2B Edition と対応する B2C を区別する様々な Adobe Experience Platform 機能が改善されています。B2B の使用例に対するエクスペリエンスデータモデル (XDM) の改善、ID 解決とプロファイルセグメント化のアップグレード、Marketo Engage用のカスタムビルドのコネクタと宛先が含まれます。 Marketoコネクタを使用すると、B2B ブランドは、業界最先端の B2B エンゲージメントデータと行動情報を結び付け、リードを育成し、アカウントベースのマーケティング操作を強化できます。

-[**新しい B2B エディションと B2P エディション**](#editions)
-[**新しいMarketoデータソースおよび宛先コネクタ**](#marketo)
-[**標準 B2B XDM**](#XDM)

## 新しい B2B エディションと B2P エディション {#editions}

**新しい B2B エディションと B2P エディション** B2B のデータと機能をリアルタイム CDP と Platform Activation の両方に提供する製品を購入できます。

Real-time CDP B2B Edition の詳細については、 [概要](./b2b-overview.md)

## 新しいMarketoデータソースおよび宛先コネクタ {#marketo}

**新しいMarketoデータソースおよび宛先コネクタ** Marketoデータを Platform および Platform オーディエンスにストリーミングして、Marketoに戻します。 すべての Platform ユーザーが利用できます。

| 機能 | 説明 |
|---|---|
| Marketo Engageソースコネクタ | Marketo Engageソースコネクタを使用すると、マーケターは 1 つ以上のMarketoインスタンスからAdobe Experience Platformインスタンスにデータをシームレスに取り込み、リード管理と B2B マーケター向けの完全なソリューションを提供します。 |
| Marketo Engage先 | この [Marketoの宛先](https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/adobe/marketo-engage.html?lang=ja) Adobe Experience Platformで作成したセグメントをMarketoにプッシュし、静的リストとして表示できます。 |

Marketo Source Connector について詳しくは、 [概要](../sources/connectors/adobe-applications/marketo/marketo.md)

## 標準 B2B XDM {#XDM}

**標準 B2B XDM** クラス、フィールドグループ、データタイプは、すべての Platform ユーザーが使用できます。

| 機能 | 説明 |
|---|---|
| 標準 B2B XDM クラス | Real-time Customer Data Platform B2B Edition は、アカウント、商談、キャンペーンなど、B2B の重要なデータエンティティに関する詳細をキャプチャする、いくつかの標準 XDM を提供します |

詳しくは、 [Real-time Customer Data Platform B2B Edition のスキーマ](./schemas/b2b.md) を参照してください。
