---
title: Facebookの表示先
seo-title: Facebookの表示先
description: ハッシュ化された電子メールに基づいて、Facebookキャンペーンのプロファイルをアクティブ化し、オーディエンスのターゲット設定、パーソナライゼーション、抑制を行います。
seo-description: ハッシュ化された電子メールに基づいて、Facebookキャンペーンのプロファイルをアクティブ化し、オーディエンスのターゲット設定、パーソナライゼーション、抑制を行います。
translation-type: tm+mt
source-git-commit: bfcbc56f05fa1c3b5fafd57b1166e50130b6007d

---


# （ベータ版）Facebookの宛先

>[!IMPORTANT]
>
>Adobe Real-time CDPのFacebookの宛先は現在ベータ版で、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

## 概要

ハッシュ化された電子メールに基づいて、Facebookキャンペーンのプロファイルをアクティブ化し、オーディエンスのターゲット設定、パーソナライゼーション、抑制を行います。

## 宛先の仕様

### アクティベーションタイプ

セグメントエクスポート — セグメント(オーディエンス)のすべてのメンバーを、識別子（名前、電話番号など）と共にエクスポートします。Facebookの行き先で使用される

## 前提条件

Before you can send your audience segments to [!DNL Facebook], make sure you meet the following requirements:

1. お使いの [!DNL Facebook] ユーザーアカウントで、使用するプランの広告アカウントに対する&#x200B;**キャンペーンの管理**&#x200B;権限が有効になっている必要があります。
2. **Adobe Experience Cloud** ビジネスアカウントを [!DNL Facebook Ad Account] の広告パートナーとして追加します。`business ID=206617933627973`.を使用します。詳しくは、[ビジネスマネージャにパートナーを追加する](https://www.facebook.com/business/help/1717412048538897)を参照してください。
   >[!IMPORTANT]
   > Adobe Experience Cloud の権限を設定する場合は、**キャンペーンの管理**&#x200B;権限を有効にする必要があります。これは、[!DNL Adobe Real-time CDP] 統合に必要です。
3. [!DNL Facebook Custom Audiences] 利用規約を読み、署名します。これをおこなうには、`https://business.facebook.com/ads/manage/customaudiences/tos/?act=[accountID]` に進みます（`accountID` は [!DNL Facebook Ad Account ID] です）。


## 宛先の接続

Facebookの宛先に接続する方法については、ソーシャルネットワ [ークの宛先認証ワークフローを参照してくださ](/help/rtcdp/destinations/social-network-destinations-workflow.md)い。


## Facebookへのセグメントのアクティブ化

For instructions on how to activate segments to Facebook, see [Activate Data to Destinations](/help/rtcdp/destinations/activate-destinations.md).