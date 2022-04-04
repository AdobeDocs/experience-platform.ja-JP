---
title: Supporting Customer Consent Preferences Using the Adobe Experience Platform Web SDK
description: Adobe Experience Platform Web SDK で同意設定をサポートする方法について説明します。
keywords: 同意；defaultConsent；デフォルトの同意；setConsent；プロファイルのプライバシーフィールドグループ；Experience Event のプライバシーフィールドグループ；Privacy フィールドグループ；
exl-id: 647e4a84-4a66-45d6-8b05-d78786bca63a
source-git-commit: 16c8972333fa67fa2e308445f4ad6282510370d1
workflow-type: tm+mt
source-wordcount: '950'
ht-degree: 31%

---

# 顧客の同意設定のサポート

ユーザーのプライバシーを尊重するため、SDK に対して特定の目的でユーザー固有のデータを使用することを許可する前に、ユーザーの同意を求めることができます。現在、SDK はユーザーに対し、あらゆる目的に対するオプトインまたはオプトアウトのみを許可していますが、アドビでは、将来的に、特定の目的でより詳細な制御を提供したいと考えています。

ユーザーがすべての目的をオプトインした場合、SDK は次のタスクを実行できます。

* アドビのサーバーとの間でデータを送信する。
* Read and write cookies or web storage items.

ユーザーがすべての目的をオプトアウトした場合、SDK は次のタスクを実行しません。

## Configuring consent

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
>コマンドは、メモリ内のキューにのみ格納されます。 ページの読み込み時には保存されません。

If you do not want to collect events that occurred before the user&#39;s opt-in preferences are set, you can pass `"defaultConsent": "out"` during SDK configuration. ユーザーのオプトイン設定に依存するコマンドを実行しようとしても、ユーザーのオプトイン設定を SDK に伝えるまで、効果はありません。

>[!NOTE]
>
>Currently, the SDK supports only a single all or nothing purpose. アドビでは、様々な機能や製品に対応する、さらに堅牢な目的やカテゴリのセットを構築する予定ですが、現在の実装アプリーチでは、すべてをオプトインするか、すべてをオプトインしないかのいずれかです。This only applies to Adobe Experience Platform [!DNL Web SDK] and NOT other Adobe JavaScript libraries.

この時点で、ユーザーインターフェイス内のどこかでユーザーにオプトインを求めることをお勧めします。ユーザーの環境設定を収集した後、これらの環境設定を SDK に伝えます。

## 同意設定の連絡 Adobe Experience Platform標準を使用

The SDK supports versions 1.0 and 2.0 of the Adobe Experience Platform consent standard. Currently, the 1.0 and 2.0 standards only support automatic enforcement of an all or nothing consent preference. The 1.0 standard is being phased out in favor of the 2.0 standard. 2.0 標準を使用すると、同意設定を手動で適用するための同意設定を追加できます。

### Adobe標準バージョン 2.0 の使用

Adobe Experience Platformを使用している場合、プロファイルスキーマにプライバシースキーマフィールドグループを含める必要があります。 詳しくは、 [Adobe Experience Platformのガバナンス、プライバシー、セキュリティ](../../landing/governance-privacy-security/overview.md) Adobe標準バージョン 2.0 の詳細。データは、 `consents` フィールド [!UICONTROL 同意および環境設定] プロファイルフィールドグループを使用します。

If the user opts in, execute the `setConsent` command with the collect preference set to `y` as follows:

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

時間フィールドには、ユーザーが同意設定を最後に更新した日時を指定する必要があります。 ユーザーがオプトアウトを選択した場合は、 `setConsent` コマンドを使用して、collect preference をに設定します。 `n` 次のように指定します。

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

### Adobe標準バージョン 1.0 の使用

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

## IAB TCF 標準を使用した同意設定の伝達

The SDK supports recording a user&#39;s consent preferences provided through the Interactive Advertising Bureau Europe (IAB) Transparency and Consent Framework (TCF) standard. 同じ `setConsent` コマンドは次のようになります。

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

この方法で同意が設定されると、リアルタイム顧客プロファイルは同意情報で更新されます。 これを機能させるには、プロファイル XDM スキーマに、 [プロファイルプライバシースキーマフィールドグループ](https://github.com/adobe/xdm/blob/master/docs/reference/mixins/profile/profile-privacy.schema.md). When sending events, the IAB consent information needs to be added manually to the event XDM object. SDK では、イベントに同意情報が自動的に含まれるわけではありません。 イベントで同意情報を送信するには、 [エクスペリエンスイベントプライバシーフィールドグループ](https://github.com/adobe/xdm/blob/master/docs/reference/mixins/experience-event/experienceevent-privacy.schema.md) をエクスペリエンスイベントスキーマに追加する必要があります。

## 1 回のリクエストで複数の標準を送信

また、SDK は、1 回のリクエストでの複数の同意オブジェクトの送信もサポートしています。

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

`setConsent` コマンドを使用してユーザー設定を SDK に伝えた後、SDK はユーザー設定を Cookie に保持します。次回ユーザーがブラウザーに Web サイトを読み込む際に、SDK は、これらの永続的な環境設定を取得して使用し、Adobeにイベントを送信できるかどうかを決定します。

You will need to store the user preferences independently to be able to show the consent dialog with the current preferences. SDK からユーザーの環境設定を取得する方法はありません。 ユーザーの環境設定を SDK と同期させるには、 `setConsent` コマンドを使用します。 The SDK will only make a server call if the preferences have changed.

## Syncing identities while setting consent

When the default consent is pending or out, the `setConsent` may be the first request that goes out and establishes identity. このため、最初のリクエストで ID を同期することが重要な場合があります。 ID マップは、 `setConsent` ～と同様の命令 `sendEvent` コマンドを使用します。 詳しくは、 [Experience CloudID を取得中](../identity/overview.md)
