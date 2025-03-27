---
solution: Experience Platform
title: 同意および環境設定スキーマフィールドグループ
description: 同意および環境設定スキーマフィールドグループについて説明します。
exl-id: ec592102-a9d3-4cac-8b94-58296a138573
source-git-commit: be35c5398cd96cdfe424c5088db288ba4061ac4a
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# [!UICONTROL  同意および環境設定 ] フィールドグループ

[!UICONTROL  同意および環境設定 ] は、[[!DNL XDM Individual Profile]  クラス ](../../classes/individual-profile.md) の標準フィールドグループで、個々の顧客の同意および環境設定に関する情報を収集します。

>[!NOTE]
>
>このフィールドグループは [!DNL XDM Individual Profile] とのみ互換性があるので、[!DNL XDM ExperienceEvent] スキーマには使用できません。 エクスペリエンスイベントスキーマに同意データと環境設定データを含める場合は、代わりに [[!UICONTROL  カスタムフィールドグループ ] を使用して、](../../data-types/consents.md) プライバシー、Personalization、マーケティングの環境設定に対する同意 [ データタイプ ](../../ui/resources/field-groups.md#create) をスキーマに追加します。

## フィールドグループ構造 {#structure}

[!UICONTROL  同意および環境設定 ] フィールドグループには、同意と環境設定の情報を取り込むために、単一のオブジェクトタイプのフィールド `consents` が用意されています。 このフィールドは、[[!UICONTROL  プライバシー、Personalizationおよびマーケティング環境設定に対する同意 ] データタイプ ](../../data-types/consents.md) を拡張し、`adID` フィールドを削除して、`idSpecific` マップフィールドを追加します。

![](../../images/field-groups/consent.png)

>[!TIP]
>
>XDM リソースを検索し、Platform UI でその構造を検査する手順については、への [XDM リソースの調査 ](../../ui/explore.md) に関するガイドを参照してください。

次の JSON は、[!UICONTROL  同意と環境設定 ] フィールドグループが処理できるデータのタイプの例を示しています。 フィールドグループが提供するほとんどのフィールドの使用方法について詳しくは、[ 同意および環境設定データタイプ ](../../data-types/consents.md) に関するガイドを参照してください。 以下のサブセクションでは、フィールドグループがデータタイプに追加する一意の属性に焦点を当てています。

```json
{
  "consents": {
    "collect": {
      "val": "VI"
    },
    "share": {
      "val": "y"
    },
    "personalize": {
      "content": {
        "val": "y"
      }
    },
    "marketing": {
      "preferred": "email",
      "any": {
        "val": "y"
      },
      "email": {
        "val": "y"
      }
    },
    "idSpecific": {
      "ECID": {
        "37784337855396895622558625508046772577": {
          "adID": {
            "val": "n",
          },
          "share": {
            "val": "n"
          },
          "marketing": {
            "push": {
              "val": "n",
              "time": "2020-09-30T01:02:33+00:00",
              "reason": "not relevant"
            }
          }
        }
      },
      "email": {
        "john@xyz.com": {
          "marketing": {
            "email": {
              "val": "y"
            }
          }
        }
      }
    },
    "metadata": {
      "time": "2019-01-01T15:52:25+00:00"
    }
  }
}
```

>[!TIP]
>
>顧客の同意データと環境設定データのマッピング方法を視覚化するために、Experience Platformで定義する XDM スキーマのサンプル JSON データを生成できます。 詳しくは、次のドキュメントを参照してください。
>
>* [UI でのサンプルデータの生成 ](../../ui/sample.md)
>* [API でのサンプルデータの生成 ](../../api/sample-data.md)

### `idSpecific`

`idSpecific` は、特定の同意または環境設定が顧客に普遍的に適用されるのではなく、1 つのデバイスまたは ID に制限されている場合に使用できます。 例えば、ある顧客が、あるアドレスへのメールの受信をオプトアウトしながら、別のアドレスへのメールを許可できる可能性があります。

>[!IMPORTANT]
>
>チャネルレベルの同意および環境設定（`idSpecific` 以外の `consents` で提供される同意など）は、そのチャネル内のすべての ID に適用されます。 したがって、すべてのチャネルレベルの同意と環境設定は、同等の ID 固有の設定またはデバイス固有の設定に従うかどうかに直接影響します。
>
>* 顧客がチャネルレベルでオプトアウトした場合、`idSpecific` の同等の同意または環境設定は無視されます。
>* チャネルレベルの同意または環境設定が設定されていない場合、または顧客がオプトインした場合、`idSpecific` の同等の同意または環境設定が適用されます。

`idSpecific` オブジェクト内の各キーは、Adobe Experience Platform ID サービスによって認識される特定の ID 名前空間を表します。 独自のカスタム名前空間を定義して様々な識別子を分類できますが、ID サービスが提供する標準の名前空間の 1 つを使用して、リアルタイム顧客プロファイルのストレージサイズを小さくすることをお勧めします。 ID 名前空間について詳しくは、ID サービスドキュメントの [ID 名前空間の概要 ](../../../identity-service/features/namespaces.md) を参照してください。

各名前空間オブジェクトのキーは、顧客が環境設定を指定した一意の ID 値を表します。 各 ID 値には、`consents` と同じ方法で書式設定された、同意および環境設定の完全なセットを含めることができます。

```json
"idSpecific": {
  "email": {
    "jdoe@example.com": {
      "marketing": {
        "email": {
          "val": "n"
        }
      }
    }
  },
  "ECID": {
    "37784337855396895622558625508046772577": {
      "collect": {
        "val": "y"
      },
      "adID": {
        "val": "n"
      },
      "marketing": {
        "push": {
          "val": "n"
        }
      }
    }
  }
}
```

`idSpecific` セクション `marketing` 提供されるオブジェクト内では、「`any`」フィールドと「`preferred`」フィールドはサポートされていません。 これらのフィールドは、ユーザーレベルでのみ設定できます。 また、`email`、`sms` および `push` の `idSpecific` マーケティング環境設定では、`subscriptions` のフィールドはサポートされていません。

`idSpecific` の節でのみ提供できる同意もあります。`adID`。 このフィールドについては、次のサブセクションで説明します。

#### `adID`

`adID` の同意は、広告主 ID （IDFA または GAID）をこのデバイス上のアプリ間で顧客をリンクするために使用できるかどうかに対する顧客の同意を表します。 この値は、`idSpecific` セクションの `ECID` ID 名前空間でのみ設定でき、他の名前空間またはこのフィールドグループのユーザーレベルでは設定できません。

```json
"idSpecific": {
  "ECID": {
    "37784337855396895622558625508046772577": {
      "collect": {
        "val": "y"
      },
      "adID": {
        "val": "n"
      },
      "marketing": {
        "push": {
          "val": "n"
        }
      }
    }
  }
}
```

>[!NOTE]
>
>この値は、必要に応じてAdobe Experience Platform Mobile SDKが自動的に設定するので、直接設定する必要はありません。

## フィールドグループを使用したデータの取り込み {#ingest}

[!UICONTROL  同意および環境設定 ] フィールドグループを使用して顧客から同意データを取り込むには、そのフィールドグループを含むスキーマに基づいてデータセットを作成する必要があります。

フィールドにフィールドグループを割り当てる手順については、[UI でのスキーマの作成 ](https://www.adobe.com/go/xdm-schema-editor-tutorial-en) に関するチュートリアルを参照してください。 [!UICONTROL  同意および環境設定 ] フィールドグループを含むフィールドを含むスキーマを作成したら、既存のスキーマを使用してデータセットを作成する手順に従って、データセットユーザーガイドの [ データセットの作成 ](../../../catalog/datasets/user-guide.md#create) の節を参照してください。

>[!IMPORTANT]
>
>同意データを [!DNL Real-Time Customer Profile] に送信する場合は、[!UICONTROL  同意および環境設定 ] フィールドグループを含む [!DNL XDM Individual Profile] クラスに基づいて [!DNL Profile] 対応スキーマを作成する必要があります。 そのスキーマに基づいて作成したデータセットも、[!DNL Profile] 用に有効にする必要があります。 スキーマとデータセットの要件に関連する特定の手順については [!DNL Real-Time Customer Profile] 上記にリンクされたチュートリアルを参照してください。
>
>また、顧客プロファイルを正しく更新するには、最新の同意データと環境設定データを含むデータセットに優先順位を付けるように結合ポリシーが設定されていることを確認する必要があります。 詳しくは、[ 結合ポリシー ](../../../rtcdp/profile/merge-policies.md) の概要を参照してください。

## 同意および環境設定の変更の処理

お客様が web サイトに対する同意や環境設定を変更した場合は、変更内容を収集し、[Adobe Experience Platform web SDK](../../../web-sdk/commands/setconsent.md) を使用して直ちに適用する必要があります。 お客様がデータ収集をオプトアウトした場合、すべてのデータ収集は直ちに停止する必要があります。 顧客がパーソナライゼーションをオプトアウトする場合、次に読み込まれるページにはパーソナライゼーションが存在しないはずです。

## 次の手順

このドキュメントでは、[!UICONTROL  同意および環境設定 ] フィールドグループの構造と使用について説明しました。 フィールドグループが提供するその他のフィールドについて詳しくは、[[!UICONTROL  プライバシー、Personalizationおよびマーケティング環境設定に関する同意 ] データタイプ ](../../data-types/consents.md) に関するドキュメントを参照してください。
