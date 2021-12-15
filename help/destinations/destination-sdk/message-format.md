---
description: このページでは、Adobe Experience Platformから宛先に書き出されたデータのメッセージ形式とプロファイル変換について説明します。
title: メッセージのフォーマット
exl-id: 1212c1d0-0ada-4ab8-be64-1c62a1158483
source-git-commit: 468b9309c5184684c0b25c2656a9eef37715af53
workflow-type: tm+mt
source-wordcount: '1981'
ht-degree: 2%

---

# メッセージのフォーマット

## 前提条件 — Adobe Experience Platformの概念 {#prerequisites}

Adobe側のメッセージ形式、プロファイル設定および変換プロセスを理解するには、次のExperience Platformの概念を把握してください。

* **エクスペリエンスデータモデル (XDM)**. [XDM の概要](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja) および  [Adobe Experience Platformで XDM スキーマを作成する方法](https://experienceleague.adobe.com/docs/experience-platform/xdm/tutorials/create-schema-ui.html?lang=en).
* **クラス**. [UI でのクラスの作成と編集](https://experienceleague.adobe.com/docs/experience-platform/xdm/ui/resources/classes.html?lang=en).
* **IdentityMap**. ID マップは、Adobe Experience Platformのすべてのエンドユーザー ID のマップを表します。 参照： `xdm:identityMap` 内 [XDM フィールドディクショナリ](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en).
* **SegmentMembership**. この [segmentMembership](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en) XDM 属性は、プロファイルがどのセグメントに属しているかを知らせます。 の `status` フィールドには、 [セグメントメンバーシップの詳細スキーマフィールドグループ](https://experienceleague.adobe.com/docs/experience-platform/xdm/field-groups/profile/segmentation.html).

## 概要 {#overview}

このページのコンテンツを他の [パートナー宛先の設定オプション](./configuration-options.md). このページでは、Adobe Experience Platformから宛先に書き出されたデータのメッセージ形式とプロファイル変換について説明します。 その他のページでは、宛先への接続と認証に関する詳細について説明します。

Adobe Experience Platformは、様々なデータ形式で、大量の宛先にデータを書き出します。 宛先のタイプの例としては、広告プラットフォーム (Google)、ソーシャルネットワーク (Facebook)、クラウドストレージの場所 (Amazon S3、Azure Event Hubs) があります。

Experience Platformは、書き出されたプロファイルのメッセージ形式を、想定される形式に合わせて調整できます。 このカスタマイズを理解するには、次の概念が重要です。
* Adobe Experience Platformのソース (1) とターゲット (2) の XDM スキーマ
* パートナー側 (3) で期待されるメッセージ形式
* XDM スキーマと期待されるメッセージ形式の間の変換レイヤー。これは、 [メッセージ変換テンプレート](./message-format.md#using-templating).

![スキーマから JSON への変換](./assets/transformations-3-steps.png)

Experience Platformは、XDM スキーマを使用して、一貫した再利用可能な方法でデータの構造を記述します。

<!--

Users who want to activate data to your destination need to map the fields in their Experience Platform datasets to a schema that translates to your destination's expected format. Adobe will create a custom field group for your company to add to the target schema. The fields in the field group depend on the profile attribute fields that you can receive.

-->

**ソース XDM スキーマ (1)**:この項目は、顧客がスキーマで使用するExperience Platformを指します。 Experience Platform内、 [マッピング手順](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en#mapping) 「宛先のアクティブ化」ワークフローで、ユーザーは、ソーススキーマのフィールドを宛先のターゲットスキーマにマッピングします (2)。

**Target XDM スキーマ (2)**:宛先で想定されている形式の JSON 標準スキーマ (3) に基づいて、ターゲット XDM スキーマでプロファイル属性と ID を定義できます。 これは、の宛先設定でおこなえます。 [schemaConfig](./destination-configuration.md#schema-configuration) および [identityNamespaces](./destination-configuration.md#identities-and-attributes) オブジェクト。

**宛先プロファイル属性の JSON 標準スキーマ (3)**:この項目は、 [JSON スキーマ](https://json-schema.org/learn/miscellaneous-examples.html) プラットフォームがサポートするすべてのプロファイル属性とそのタイプ ( 例：オブジェクト、文字列、配列 ) の形式を使用します。 例えば、宛先でサポートされるフィールドは次のようになります。 `firstName`, `lastName`, `gender`, `email`, `phone`, `productId`, `productName`など。 次が必要です： [メッセージ変換テンプレート](./message-format.md#using-templating) を使用して、Experience Platformから書き出されたデータを、目的の形式に合わせて調整します。

上記のスキーマ変換に基づいて、ソース XDM スキーマとパートナー側のサンプルスキーマの間でプロファイル設定がどのように変化するかを次に示します。

![変換メッセージの例](./assets/transformations-with-examples.png)

<br> 


## はじめに — 3 つの基本的な属性の変換 {#getting-started}

次の例では、Adobe Experience Platformの 3 つの一般的なプロファイル属性を使用して、プロファイル変換プロセスを示しています。 **名**, **姓**、および **電子メールアドレス**.

>[!NOTE]
>
>顧客は、ソース XDM スキーマの属性を、Adobe Experience Platform UI のパートナー XDM スキーマにマッピングします ( **マッピング** 手順 [宛先ワークフローを有効化](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping).

プラットフォームが次のようなメッセージ形式を受信できるとします。

```curl
POST https://YOUR_REST_API_URL/users/
Content-Type: application/json
Authorization: Bearer YOUR_REST_API_KEY

{
  "attributes":
    {
      "first_name": "Yours",
      "last_name": "Truly",
      "external_id": "yourstruly@adobe.com"
    }
}
```

メッセージ形式を考慮すると、対応する変換は次のようになります。

| Adobe側のパートナー XDM スキーマの属性 | 変換 | 側の HTTP メッセージの属性 |
|---------|----------|---------|
| `_your_custom_schema.firstName` | ` attributes.first_name` | `first_name` |
| `_your_custom_schema.lastName` | `attributes.last_name` | `last_name` |
| `personalEmail.address` | `attributes.external_id` | `external_id` |

## ID、属性、セグメントメンバーシップの変換にテンプレート言語を使用する {#using-templating}

Adobeでは、 [神社](https://jinja.palletsprojects.com/en/2.11.x/) を使用して、XDM スキーマのフィールドを、宛先でサポートされている形式に変換します。

この節では、これらの変換の方法の例をいくつか示します。入力 XDM スキーマから、テンプレートを介して、宛先で受け入れられるペイロード形式に出力する方法です。 以下の例は、次のように複雑さを増すことで示しています。

1. 単純な変換の例です。 テンプレートが、 [プロファイル属性](./message-format.md#attributes), [セグメントのメンバーシップ](./message-format.md#segment-membership)、および [ID](./message-format.md#identities) フィールド。
2. 上記のフィールドを組み合わせたテンプレートの複雑な例を増やしました。 [セグメントと ID を送信するテンプレートの作成](./message-format.md#segments-and-identities) および [セグメント、ID およびプロファイル属性を送信するテンプレートの作成](./message-format.md#segments-identities-attributes).
3. 集計キーを含むテンプレート。 を使用する場合、 [設定可能な集計](./destination-configuration.md#configurable-aggregation) 宛先設定で、Experience Platformは、セグメント ID、セグメントステータス、id 名前空間などの条件に基づいて、宛先に書き出されたプロファイルをグループ化します。

### プロファイル属性 {#attributes}

宛先に書き出したプロファイル属性を変換するには、以下の JSON とコードサンプルを参照してください。

>[!IMPORTANT]
>
>Adobe Experience Platformで使用可能なすべてのプロファイル属性の一覧については、 [XDM フィールドディクショナリ](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en).


**入力**

プロファイル 1:

```json
{
    "attributes": {
        "firstName": {
            "value": "Hermione"
    },
    "birthDate": {}
  }
}
```

プロファイル 2:

```json
{
  "attributes": {
    "firstName": {
      "value": "Harry"
    },
    "birthDate": {
        "value": "1980/07/31"
    }
  }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、二重引用符などの不正な文字をエスケープする必要があります `""` テンプレートを [宛先サーバーの設定](./server-and-template-configuration.md#template-specs). 二重引用符のエスケープについて詳しくは、 [JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/).

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            {% for attribute in profile.attributes %}
            "{{ attribute.key }}":
                {% if attribute.value is empty %}
                    null
                {% else %}
                    "{{ attribute.value.value }}"
                {% endif %}
            {% if not loop.last %},{% endif %}
            {% endfor %}
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**


```json
{
    "profiles": [
        {
            "firstName": "Hermione",
            "birthDate": null
        },
        {
            "firstName": "Harry",
            "birthDate": "1980/07/31"
        }
    ]
}
```

### セグメントのメンバーシップ {#segment-membership}

この [segmentMembership](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en) XDM 属性は、プロファイルがどのセグメントに属しているかを知らせます。
の `status` フィールドには、 [セグメントメンバーシップの詳細スキーマフィールドグループ](https://experienceleague.adobe.com/docs/experience-platform/xdm/field-groups/profile/segmentation.html).

**入力**

プロファイル 1:

```json
{
  "segmentMembership": {
    "ups": {
      "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "realized"
      },
      "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "existing"
      },
      "8f812592-3f06-416b-bd50-e7831848a31a": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "exited"
      }
    }
  }
}
```

プロファイル 2:

```json
{
  "segmentMembership": {
    "ups": {
      "32396e4b-16f6-4033-9702-fc69b5e24e7c": {
        "lastQualificationTime": "2021-08-20T17:23:04Z",
        "status": "realized"
      },
      "af854278-894a-4192-a96b-320fbf2623fd": {
        "lastQualificationTime": "2021-08-20T16:44:37Z",
        "status": "existing"
      },
      "66505bf9-bc08-4bac-afbc-8b6706650ea4": {
        "lastQualificationTime": "2019-08-20T17:23:04Z",
        "status": "realized"
      }
    }
  }
}
```

**テンプレート**


>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、二重引用符などの不正な文字をエスケープする必要があります `""` テンプレートを [宛先サーバーの設定](./server-and-template-configuration.md#template-specs). 二重引用符のエスケープについて詳しくは、 [JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/).

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                {% for segment in profile.segmentMembership.ups | added %}
                "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ],
                "remove": [
                {# Alternative syntax for filtering segments by status: #}
                {% for segment in removedSegments(profile.segmentMembership.ups) %}
                "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ]
            }
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

```json
{
    "profiles": [
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "32396e4b-16f6-4033-9702-fc69b5e24e7c",
                    "af854278-894a-4192-a96b-320fbf2623fd",
                    "66505bf9-bc08-4bac-afbc-8b6706650ea4"
                ],
                "remove": [
                ]
            }
        }
    ]
}
```

### ID {#identities}

Experience Platformの ID について詳しくは、 [ID 名前空間の概要](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja).

**入力**

プロファイル 1:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    }
}
```

プロファイル 2:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "jane.doe@example.com"
            }
        ]
    }
}
```

**テンプレート**


>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、二重引用符などの不正な文字をエスケープする必要があります `""` テンプレートを [宛先サーバーの設定](./server-and-template-configuration.md#template-specs). 二重引用符のエスケープについて詳しくは、 [JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/).

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}

                {# Add a comma only if you have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}

                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ]
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

```json
{
    "profiles": [
        {
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ]
        },
        {
            "identities": [
                {
                    "type": "email",
                    "id": "jane.doe@example.com"
                }
            ]
        }
    ]
}
```


### セグメントと ID を送信するテンプレートの作成 {#segments-and-identities}

この節では、AdobeXDM スキーマとパートナー宛先スキーマの間でよく使用される変換の例を示します。
次の例では、セグメントのメンバーシップと ID の形式を変換し、宛先に出力する方法を示します。

**入力**

プロファイル 1:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            },
            "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
              "lastQualificationTime": "2019-11-20T13:15:49Z",
              "status": "existing"
            },
            "8f812592-3f06-416b-bd50-e7831848a31a": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "exited"
            }
        }
    }
}
```

プロファイル 2:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "jane.doe@example.com"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2021-08-31T10:01:42Z",
                "status": "realized"
            }
        }
    }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、二重引用符などの不正な文字をエスケープする必要があります `""` テンプレートを [宛先サーバーの設定](./server-and-template-configuration.md#template-specs). 二重引用符のエスケープについて詳しくは、 [JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/).

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
                
                {# Add a comma only if you have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}
                
                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    {% for segment in profile.segmentMembership.ups | added %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ],
                "remove": [
                    {# Alternative syntax for filtering segments by status: #}
                    {% for segment in removedSegments(profile.segmentMembership.ups) %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ]
            }
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

この `json` 以下に、Adobe Experience Platformから書き出されたデータを示します。

```json
{
    "profiles": [
        {
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "identities": [
                {
                    "type": "email",
                    "id": "jane.doe@example.com"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075"
                ],
                "remove": []
            }
        }
    ]
}
```

### セグメント、ID およびプロファイル属性を送信するテンプレートの作成 {#segments-identities-attributes}

この節では、AdobeXDM スキーマとパートナー宛先スキーマの間でよく使用される変換の例を示します。

もう 1 つの一般的な使用例は、セグメントメンバーシップ ID（例： ）を含むデータを書き出す場合です。電子メールアドレス、電話番号、広告 ID) およびプロファイル属性。 この方法でデータをエクスポートするには、以下の例を参照してください。

**入力**

プロファイル 1:

```json
{
    "attributes": {
        "firstName": {
            "value": "Hermione"
        },
        "birthDate": {}
    },
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            },
            "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
              "lastQualificationTime": "2019-11-20T13:15:49Z",
              "status": "existing"
            },
            "8f812592-3f06-416b-bd50-e7831848a31a": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "exited"
            }
        }
    }
}
```

プロファイル 2:

```json
{
    "attributes": {
        "firstName": {
            "value": "Harry"
        },
        "birthDate": {
            "value": "1980/07/31"
        }
    },
    "identityMap": {
        "email": [
            {
                "id": "harry.p@example.com"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            }
        }
    }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、二重引用符などの不正な文字をエスケープする必要があります `""` テンプレートを [宛先サーバーの設定](./server-and-template-configuration.md#template-specs). 二重引用符のエスケープについて詳しくは、 [JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/).

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "attributes": {
            {% for attribute in profile.attributes %}
                "{{ attribute.key }}":
                    {% if attribute.value is empty %}
                        null
                    {% else %}
                        "{{ attribute.value.value }}"
                    {% endif %}
                {% if not loop.last %},{% endif %}
            {% endfor %}
            },
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}

                {# Add a comma only if we have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}

                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                {% for segment in profile.segmentMembership.ups | added %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ],
                "remove": [
                {# Alternative syntax for filtering segments by status: #}
                {% for segment in removedSegments(profile.segmentMembership.ups) %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ]
            }
        }
    ]
}
```

**結果**

この `json` 以下に、Adobe Experience Platformから書き出されたデータを示します。

```json
{
    "profiles": [
        {
            "attributes": {
                "firstName": "Hermione",
                "birthDate": null
            },
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "attributes": {
                "firstName": "Harry",
                "birthDate": "1980/07/21"
            },
            "identities": [
                {
                    "type": "email",
                    "id": "harry.p@example.com"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075"
                ],
                "remove": []
            }
        }
    ]
}
```

### テンプレートに集計キーを含めて、様々な条件でグループ化されてエクスポートされたプロファイルにアクセスします {#template-aggregation-key}

を使用する場合、 [設定可能な集計](./destination-configuration.md#configurable-aggregation) 宛先設定で、セグメント ID、セグメントエイリアス、セグメントメンバーシップ、id 名前空間などの条件に基づいて、宛先に書き出したプロファイルをグループ化できます。

メッセージ変換テンプレートでは、次の節の例に示すように、上記の集計キーにアクセスできます。 集計キーを使用して、宛先で想定される形式とレート制限に一致するように、Experience Platformから書き出された HTTP メッセージを構造化します。

#### テンプレートでセグメント ID 集計キーを使用 {#aggregation-key-segment-id}

次を使用する場合、 [設定可能な集計](./destination-configuration.md#configurable-aggregation) および設定 `includeSegmentId` を true に設定すると、宛先に書き出される HTTP メッセージ内のプロファイルは、セグメント ID でグループ化されます。 以下で、テンプレートのセグメント ID へのアクセス方法を参照してください。

**入力**

以下の 4 つのプロファイルについて考えてみましょう。
* 最初の 2 つは、セグメント ID を持つセグメントの一部です `788d8874-8007-4253-92b7-ee6b6c20c6f3`
* 3 番目のプロファイルは、セグメント ID を持つセグメントの一部です `8f812592-3f06-416b-bd50-e7831848a31a`
* 4 つ目のプロファイルは、上記の両方のセグメントの一部です。

プロファイル 1:

```json
{
   "attributes":{
      "firstName":{
         "value":"Hermione"
      }
   },
   "segmentMembership":{
      "ups":{
         "788d8874-8007-4253-92b7-ee6b6c20c6f3":{
            "lastQualificationTime":"2020-11-20T13:15:49Z",
            "status":"existing"
         }
      }
   }
}
```

プロファイル 2:

```json
{
   "attributes":{
      "firstName":{
         "value":"Harry"
      }
   },
   "segmentMembership":{
      "ups":{
         "788d8874-8007-4253-92b7-ee6b6c20c6f3":{
            "lastQualificationTime":"2020-11-20T13:15:49Z",
            "status":"existing"
         }
      }
   }
}
```

プロファイル 3:

```json
{
   "attributes":{
      "firstName":{
         "value":"Tom"
      }
   },
   "segmentMembership":{
      "ups":{
         "8f812592-3f06-416b-bd50-e7831848a31a":{
            "lastQualificationTime":"2021-02-20T12:00:00Z",
            "status":"existing"
         }
      }
   }
}
```

プロファイル 4:

```json
{
   "attributes":{
      "firstName":{
         "value":"Jerry"
      }
   },
   "segmentMembership":{
      "ups":{
         "8f812592-3f06-416b-bd50-e7831848a31a":{
            "lastQualificationTime":"2021-02-20T12:00:00Z",
            "status":"existing"
         },
         "788d8874-8007-4253-92b7-ee6b6c20c6f3":{
            "lastQualificationTime":"2020-11-20T13:15:49Z",
            "status":"existing"
         }
      }
   }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、二重引用符などの不正な文字をエスケープする必要があります `""` テンプレートを [宛先サーバーの設定](./server-and-template-configuration.md#template-specs). 二重引用符のエスケープについて詳しくは、 [JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/).

以下に、 `audienceId` は、セグメント ID にアクセスするためにテンプレートで使用されます。 この例では、 `audienceId` を使用します。 独自の分類に応じて、他のフィールド名を代わりに使用できます。

```python
{
    "audienceId": "{{ input.aggregationKey.segmentId }}",
    "profiles": [
        {% for profile in input.profiles %}
        {
            "first_name": "{{ profile.attributes.firstName.value }}"
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

宛先に書き出すと、プロファイルは、セグメント ID に基づいて 2 つのグループに分割されます。

```json
{
   "audienceId":"788d8874-8007-4253-92b7-ee6b6c20c6f3",
   "profiles":[
      {
         "firstName":"Hermione"
      },
      {
         "firstName":"Harry"
      },
      {
         "firstName":"Jerry"
      }
   ]
}
```

```json
{
   "audienceId":"8f812592-3f06-416b-bd50-e7831848a31a",
   "profiles":[
      {
         "firstName":"Tom"
      },
      {
         "firstName":"Jerry"
      }
   ]
}
```

#### テンプレートでセグメントエイリアス集計キーを使用 {#aggregation-key-segment-alias}

次を使用する場合、 [設定可能な集計](./destination-configuration.md#configurable-aggregation) および設定 `includeSegmentId` を true に設定すると、テンプレートのセグメントエイリアスにアクセスすることもできます。

テンプレートに以下の行を追加して、セグメントエイリアスでグループ化された書き出されたプロファイルにアクセスします。

```python
customerList={{input.aggregationKey.segmentAlias}}
```

#### テンプレートでセグメントステータス集計キーを使用 {#aggregation-key-segment-status}

次を使用する場合、 [設定可能な集計](./destination-configuration.md#configurable-aggregation) および設定 `includeSegmentId` および `includeSegmentStatus` を true に設定すると、テンプレートのセグメントステータスにアクセスできます。 これにより、プロファイルをセグメントに追加またはセグメントから削除するかに基づいて、宛先に書き出された HTTP メッセージ内のプロファイルをグループ化できます。

次のような値を選択できます。

* 実現
* 既存
* 終了

上記の値に基づいて、以下の行をテンプレートに追加し、セグメントに対してプロファイルを追加または削除します。

```python
action={% if input.aggregationKey.segmentStatus == "exited" %}REMOVE{% else %}ADD{% endif%}
```

#### テンプレートで ID 名前空間集計キーを使用 {#aggregation-key-identity}

以下は、 [設定可能な集計](./destination-configuration.md#configurable-aggregation) の宛先設定は、書き出されたプロファイルを id 名前空間別に集計するように、フォーム内で集計するように設定されます。 `"namespaces": ["email", "phone"]` および `"namespaces": ["GAID", "IDFA"]`. 詳しくは、 `groups` パラメーター [宛先設定 API リファレンス](./destination-configuration-api.md) を参照してください。

**入力**

プロファイル 1:

```json
{
   "identityMap":{
      "email":[
         {
            "id":"e1@example.com"
         },
         {
            "id":"e2@example.com"
         }
      ],
      "phone":[
         {
            "id":"+40744111222"
         }
      ],
      "IDFA":[
         {
            "id":"AEBE52E7-03EE-455A-B3C4-E57283966239"
         }
      ],
      "GAID":[
         {
            "id":"e4fe9bde-caa0-47b6-908d-ffba3fa184f2"
         }
      ]
   }
}
```

プロファイル 2:

```json
{
   "identityMap":{
      "email":[
         {
            "id":"e3@example.com"
         }
      ],
      "phone":[
         {
            "id":"+40744333444"
         },
         {
            "id":"+40744555666"
         }
      ],
      "IDFA":[
         {
            "id":"134GHU45-34HH-GHJ7-K0H8-LHN665998NN0"
         }
      ],
      "GAID":[
         {
            "id":"47bh00i9-8jv6-334n-lll8-nb7f24sghg76"
         }
      ]
   }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、二重引用符などの不正な文字をエスケープする必要があります `""` テンプレートを [宛先サーバーの設定](./server-and-template-configuration.md#template-specs). 二重引用符のエスケープについて詳しくは、 [JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/).

次の点に注意してください。 `input.aggregationKey.identityNamespaces` は、以下のテンプレートで使用されています

```python
{
            "profiles": [
            {% for profile in input.profiles %}
            {
                {% for ns in input.aggregationKey.identityNamespaces %}
                "{{ns}}": [
                    {% for id in profile.identityMap[ns] %}
                    "{{id.id}}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ]{% if not loop.last %},{% endif %}
                {% endfor %}
            }{% if not loop.last %},{% endif %}
            {% endfor %}
        ]
}
```

**結果**

宛先に書き出すと、プロファイルは、ID 名前空間に基づいて 2 つのグループに分割されます。 電子メールと電話は 1 つのグループに属し、GAID と IDFA は別のグループに属します。

```json
{
   "profiles":[
      {
         "email":[
            "e1@example.com",
            "e2@example.com"
         ],
         "phone":[
            "+40744111222"
         ]
      },
      {
         "email":[
            "e3@example.com"
         ],
         "phone":[
            "+40744333444",
            "+40744555666"
         ]
      }
   ]
}
```

```json
{
   "profiles":[
      {
         "IDFA":[
            "AEBE52E7-03EE-455A-B3C4-E57283966239"
         ],
         "GAID":[
            "e4fe9bde-caa0-47b6-908d-ffba3fa184f2"
         ]
      },
      {
         "IDFA":[
            "134GHU45-34HH-GHJ7-K0H8-LHN665998NN0"
         ],
         "GAID":[
            "47bh00i9-8jv6-334n-lll8-nb7f24sghg76"
         ]
      }
   ]
}
```

#### URL テンプレートでの集計キーの使用 {#aggregation-key-url-template}

使用事例に応じて、次に示すように、URL にここに記載されている集計キーも使用できます。

```python
https://api.example.com/audience/{{input.aggregationKey.segmentId}}
```

### 参照：変換テンプレートで使用されるコンテキストと関数 {#reference}

テンプレートに指定されたコンテキストにはが含まれます `input`  （この呼び出しで書き出されるプロファイル/データ）および `destination` (Adobeがデータを送信する宛先に関するデータ。すべてのプロファイルで有効 )。

以下の表では、上記の例に含まれる関数の説明を示します。

| 関数 | 説明 |
|---------|----------|
| `input.profile` | プロファイル。 [JsonNode](https://fasterxml.github.io/jackson-databind/javadoc/2.11/com/fasterxml/jackson/databind/node/JsonNodeType.html). このページで後述するパートナー XDM スキーマに従います。 |
| `destination.segmentAliases` | Adobe Experience Platform名前空間のセグメント ID からパートナーのシステムのセグメントエイリアスにマッピングします。 |
| `destination.segmentNames` | Adobe Experience Platform名前空間内のセグメント名からパートナーのシステム内のセグメント名にマッピングします。 |
| `addedSegments(listOfSegments)` | ステータスを持つセグメントのみを返します `realized` または `existing`. |
| `removedSegments(listOfSegments)` | ステータスを持つセグメントのみを返します `exited`. |

<!--

## What Adobe needs from you to set up your destination {#what-adobe-needs}

Based on the transformations outlined in the sections above, Adobe needs the following information to set up your destination:

* Considering *all* the fields that your platform can receive, Adobe needs the standard JSON schema that corresponds to your expected message format. Having the template allows Adobe to define transformations and to create a custom XDM schema for your company, which customers would use to export data to your destination.

-->
