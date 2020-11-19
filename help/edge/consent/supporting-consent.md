---
title: 同意のサポート
seo-title: Adobe Experience Platform Web SDK：同意設定のサポート
description: Experience Platform Web SDK を使用して同意設定をサポートする方法について説明します
seo-description: Experience Platform Web SDK を使用して同意設定をサポートする方法について説明します
keywords: consent;defaultConsent;default consent;setConsent;Profile Privacy Mixin;Experience Event Privacy Mixin;Privacy Mixin;
translation-type: tm+mt
source-git-commit: 0928dd3eb2c034fac14d14d6e53ba07cdc49a6ea
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 63%

---


# 同意のサポート

ユーザーのプライバシーを尊重するため、SDK に対して特定の目的でユーザー固有のデータを使用することを許可する前に、ユーザーの同意を求めることができます。現在、SDK はユーザーに対し、あらゆる目的に対するオプトインまたはオプトアウトのみを許可していますが、アドビでは、将来的に、特定の目的でより詳細な制御を提供したいと考えています。

ユーザーがすべての目的をオプトインした場合、SDK は次のタスクを実行できます。

* アドビのサーバーとの間でデータを送信する。
* Cookie または Web ストレージ項目の読み取りと書き込み（ユーザーのオプトイン設定を保持する場合を除く）。

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

一般的な目的のデフォルトの同意が「保留」に設定されている場合、ユーザーのオプトイン設定に依存するコマンド（`event` コマンドなど）を実行しようとすると、そのコマンドは SDK 内のキューに追加されます。これらのコマンドは、ユーザーのオプトイン設定を SDK に通知するまで処理されません。

この時点で、ユーザーインターフェイス内のどこかでユーザーにオプトインを求めることをお勧めします。ユーザーの環境設定を収集した後、これらの環境設定を SDK に伝えます。

## 同意設定の連絡 adobe標準を通じて

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

ユーザーがオプトインしたので、SDK はその前にキューに追加されたすべてのコマンドを実行します。ユーザーのオプトインに依存する以降のコマンドは、キューに追加されず、即座に実行されます。

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

ユーザーがオプトアウトを選択したので、以前にキューに追加されたコマンドから返された promise は拒否されます。以降のコマンドで、ユーザーのオプトインに依存するものは、同様に拒否された prormise を返します。エラーの処理や抑制の詳細については、[コマンドの実行](../fundamentals/executing-commands.md)を参照してください。

>[!NOTE]
>
>現在、SDK は `general` 目的のみをサポートしています。アドビでは、様々な機能や製品に対応する、さらに堅牢な目的やカテゴリのセットを構築する予定ですが、現在の実装アプリーチでは、すべてをオプトインするか、すべてをオプトインしないかのいずれかです。This only applies to Adobe Experience Platform [!DNL Web SDK] and NOT other Adobe JavaScript libraries.

## IAB TCF標準を使用して同意の環境設定を伝える

SDKは、Interactive Advertising Bureau(IAB)Transparency and Consent Framework(TCF)標準を通じて提供されるユーザーの同意の環境設定の記録をサポートしています。 同意文字列は、上記と同じ `setConsent` コマンドを使用して次のように設定できます。

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

このように同意が設定されると、リアルタイム顧客プロファイルは同意情報で更新されます。 これを機能させるには、プロファイルXDMスキーマに [プロファイルプライバシーミックスインが含まれている必要があります](https://github.com/adobe/xdm/blob/master/docs/reference/mixins/profile/profile-privacy.schema.md)。 イベントを送信する場合、IABの同意情報をイベントXDMオブジェクトに手動で追加する必要があります。 SDKは、イベントに同意情報を自動的に含めません。 同意情報をイベントに送信するには、エクスペリエンスイベント [のプライバシーMixin](https://github.com/adobe/xdm/blob/master/docs/reference/mixins/experience-event/experienceevent-privacy.schema.md) をエクスペリエンスイベントスキーマに追加する必要があります。

## 両方の標準を1回の要求で送信する

また、SDKは、リクエスト内での複数の同意オブジェクトの送信もサポートしています。

```javascript
alloy("setConsent", {
    consent: [{
      standard: "Adobe",
      version: "1.0",
      value: {
        general: "in"
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

`setConsent` コマンドを使用してユーザー設定を SDK に伝えた後、SDK はユーザー設定を Cookie に保持します。次回ユーザーがWebサイトをブラウザーに読み込むと、SDKは、これらの永続的な環境設定を取得して使用し、イベントをAdobeに送信できるかどうかを決定します。 `setConsent` コマンドを再び実行する必要はありません。ただし、ユーザーの環境設定に変更を加えた場合は伝える必要があります（変更はいつでも加えることができます）。

## 同意の設定時にIDを同期する

デフォルトの同意が保留中の場合、最初に外に出てIDを確立する要求 `setConsent` が送信される場合があります。 このため、最初の要求時にIDを同期することが重要な場合があります。 IDマップは、コマンドと同様に `setConsent` コマンドに追加でき `sendEvent` ます。 Experience CloudIDの [取得を参照してください。](../identity/overview.md)

