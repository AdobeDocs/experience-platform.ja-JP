---
title: Adobe Experience Platform Web SDKを使用した顧客の同意設定のサポート
description: Adobe Experience Platform Web SDKを使用して、同意設定をサポートする方法について説明します。
keywords: 同意；defaultConsent；デフォルトの同意；setConsent;Profile Privacyフィールドグループ；Experience Event Privacyフィールドグループ；Privacyフィールドグループ；
exl-id: 647e4a84-4a66-45d6-8b05-d78786bca63a
source-git-commit: bd312024a1a3fb6da840a38d6e9d19fcbd6eab5a
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 32%

---

# 顧客の同意設定のサポート

ユーザーのプライバシーを尊重するため、SDK に対して特定の目的でユーザー固有のデータを使用することを許可する前に、ユーザーの同意を求めることができます。現在、SDK はユーザーに対し、あらゆる目的に対するオプトインまたはオプトアウトのみを許可していますが、アドビでは、将来的に、特定の目的でより詳細な制御を提供したいと考えています。

ユーザーがすべての目的をオプトインした場合、SDK は次のタスクを実行できます。

* アドビのサーバーとの間でデータを送信する。
* CookieまたはWebストレージ項目の読み取りと書き込み。

ユーザーがすべての目的をオプトアウトした場合、SDK は次のタスクを実行しません。

## 同意の設定

デフォルトでは、ユーザーはすべての目的に対してオプトインします。ユーザーがオプトインするまで SDK が上記のタスクを実行しないようにするには、次のように、SDK の設定時に `"defaultConsent": "pending"` を渡します。

```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "imsOrgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "defaultConsent": "pending"
});
```

一般的な目的のデフォルトの同意が「保留」に設定されている場合、ユーザーのオプトイン設定に依存するコマンド（`sendEvent` コマンドなど）を実行しようとすると、そのコマンドは SDK 内のキューに追加されます。これらのコマンドは、ユーザーのオプトイン設定を SDK に通知するまで処理されません。

>[!NOTE]
>
>コマンドは、メモリ内にのみキューに入れられます。 ページの読み込み時には保存されません。

ユーザーのオプトインの環境設定がおこなわれる前に発生したイベントを収集したくない場合は、SDKの設定時に`"defaultConsent": "out"`を渡すことができます。 ユーザーのオプトイン設定に依存するコマンドを実行しようとしても、そのユーザーのオプトイン設定をSDKに伝えるまでは効果がありません。

>[!NOTE]
>
>現在、SDKは、すべての目的または何の目的も1つしかサポートしていません。 アドビでは、様々な機能や製品に対応する、さらに堅牢な目的やカテゴリのセットを構築する予定ですが、現在の実装アプリーチでは、すべてをオプトインするか、すべてをオプトインしないかのいずれかです。これは、Adobe Experience Platform [!DNL Web SDK]にのみ適用され、他のAdobeJavaScriptライブラリには適用されません。

この時点で、ユーザーインターフェイス内のどこかでユーザーにオプトインを求めることをお勧めします。ユーザーの環境設定を収集した後、これらの環境設定を SDK に伝えます。

## 同意設定の連絡 Adobe Experience Platform標準を使用

SDKは、Adobe Experience Platform Consent Standardのバージョン1.0および2.0をサポートしています。 現在、1.0および2.0標準は、すべての同意設定または何も同意設定の自動適用のみをサポートしています。 1.0標準は廃止され、2.0標準に置き換えられています。 2.0標準では、同意設定を手動で適用するために使用できる同意設定を追加できます。

### Adobe標準バージョン2.0の使用

Adobe Experience Platformを使用している場合、プロファイルスキーマにプライバシースキーマフィールドグループを含める必要があります。 Adobe標準バージョン2.0について詳しくは、[Adobe Experience Platformのガバナンス、プライバシー、セキュリティ](../../landing/governance-privacy-security/overview.md)を参照してください。[!UICONTROL 同意と環境設定]プロファイルフィールドグループの`consents`フィールドのスキーマに対応する下のvalueオブジェクト内にデータを追加できます。

ユーザーがオプトインした場合は、次のように、collectプリファレンスを`y`に設定して`setConsent`コマンドを実行します。

```javascript
alloy("setConsent", {
    consent: [{
      standard: "Adobe",
      version: "2.0",
      value: {
        collect: {
          val: "y"
        },
        metadata: {
          time: "2021-03-17T15:48:42-07:00"
        }
      }
    }]
});
```

時間フィールドには、ユーザーが同意設定を最後に更新した日時を指定する必要があります。 ユーザーがオプトアウトを選択した場合は、次のように、collect環境設定を`n`に設定して`setConsent`コマンドを実行します。

```javascript
alloy("setConsent", {
    consent: [{
      standard: "Adobe",
      version: "2.0",
      value: {
        collect: {
          val: "n"
        },
        metadata: {
          time: "2021-03-17T15:51:30-07:00"
        }
      }
    }]
});
```

>[!NOTE]
>
>ユーザーがオプトアウトした後、SDKでは、ユーザーが`y`に対する同意を収集するように設定できません。

### Adobe標準バージョン1.0の使用

ユーザーがオプトインした場合は、次のように、`general` オプションを `in` に設定して `setConsent` コマンドを実行します。

```javascript
alloy("setConsent", {
    consent: [{
      standard: "Adobe",
      version: "1.0",
      value: {
        general: "in"
      }
    }]
});
```

ユーザーがオプトアウトを選択した場合は、次のように、`general` オプションを `out` に設定して `setConsent` コマンドを実行します。

```javascript
alloy("setConsent", {
    consent: [{
      standard: "Adobe",
      version: "1.0",
      value: {
        general: "out"
      }
    }]
});
```

>[!NOTE]
>
>ユーザーがオプトアウトすると、SDK では `in` に対するユーザーの同意を設定できません。

## IAB TCF標準を使用した同意設定の伝達

SDKは、Interactive Advertising Bureau Europe(IAB)のTransparency and Consent Framework(TCF)標準を通じて提供される、ユーザーの同意設定の記録をサポートします。 次のように、同じ`setConsent`コマンドを使用して、同意文字列を設定できます。

```javascript
alloy("setConsent", {
    consent: [{
      standard: "IAB TCF",
      version: "2.0",
      value: "CO1Z4yuO1Z4yuAcABBENArCsAP_AAH_AACiQGCNX_T5eb2vj-3Zdt_tkaYwf55y3o-wzhhaIse8NwIeH7BoGP2MwvBX4JiQCGBAkkiKBAQdtHGhcCQABgIhRiTKMYk2MjzNKJLJAilsbe0NYCD9mnsHT3ZCY70--u__7P3fAwQgkwVLwCRIWwgJJs0ohTABCOICpBwCUEIQEClhoACAnYFAR6gAAAIDAACAAAAEEEBAIABAAAkIgAAAEBAKACIBAACAEaAhAARIEAsAJEgCAAVA0JACKIIQBCDgwCjlACAoAAAAA.YAAAAAAAAAAA",
      gdprApplies: true
    }]
});
```

この方法で同意が設定されると、リアルタイム顧客プロファイルが同意情報で更新されます。 これを機能させるには、プロファイルXDMスキーマに[プロファイルプライバシースキーマフィールドグループ](https://github.com/adobe/xdm/blob/master/docs/reference/mixins/profile/profile-privacy.schema.md)を含める必要があります。 イベントを送信する場合は、IAB同意情報をイベントXDMオブジェクトに手動で追加する必要があります。 SDKでは、イベントに同意情報が自動的に含まれるわけではありません。 同意情報をイベントで送信するには、エクスペリエンスイベントプライバシーフィールドグループ[をエクスペリエンスイベントスキーマに追加する必要があります。](https://github.com/adobe/xdm/blob/master/docs/reference/mixins/experience-event/experienceevent-privacy.schema.md)

## 1回のリクエストで複数の標準を送信

また、SDKは、1回のリクエストで複数の同意オブジェクトを送信することもできます。

```javascript
alloy("setConsent", {
    consent: [{
      standard: "Adobe",
      version: "2.0",
      value: {
        collect: {
          val: "y"
        },
        metadata: {
          time: "2021-03-17T15:48:42-07:00"
        }
      }
    },{
      standard: "IAB TCF",
      version: "2.0",
      value: "CO1Z4yuO1Z4yuAcABBENArCsAP_AAH_AACiQGCNX_T5eb2vj-3Zdt_tkaYwf55y3o-wzhhaIse8NwIeH7BoGP2MwvBX4JiQCGBAkkiKBAQdtHGhcCQABgIhRiTKMYk2MjzNKJLJAilsbe0NYCD9mnsHT3ZCY70--u__7P3fAwQgkwVLwCRIWwgJJs0ohTABCOICpBwCUEIQEClhoACAnYFAR6gAAAIDAACAAAAEEEBAIABAAAkIgAAAEBAKACIBAACAEaAhAARIEAsAJEgCAAVA0JACKIIQBCDgwCjlACAoAAAAA.YAAAAAAAAAAA",
      gdprApplies: true
    }]
});
```

## 同意設定の保持

`setConsent` コマンドを使用してユーザー設定を SDK に伝えた後、SDK はユーザー設定を Cookie に保持します。次回ユーザーがブラウザーにWebサイトを読み込む際に、SDKはこれらの永続的な環境設定を取得して使用し、イベントをAdobeに送信できるかどうかを判断します。

現在の環境設定で同意ダイアログを表示するには、ユーザーの環境設定を個別に保存する必要があります。 SDKからユーザーの環境設定を取得する方法はありません。 ユーザーの環境設定がSDKと同期するように、ページの読み込みごとに`setConsent`コマンドを呼び出すことができます。 SDKは、環境設定が変更された場合にのみサーバー呼び出しをおこないます。

## 同意の設定時のIDの同期

デフォルトの同意が保留または終了した場合、`setConsent`は、最初に送信され、IDを確立する要求になる場合があります。 このため、最初のリクエストでIDを同期することが重要な場合があります。 IDマップは、`sendEvent`コマンドと同様に`setConsent`コマンドに追加できます。 [Experience CloudIDの取得](../identity/overview.md)を参照
