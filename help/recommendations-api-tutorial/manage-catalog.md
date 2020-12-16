---
title: APIを使用したRecommendationsカタログの管理
keywords: recommendations;adobe recommendations;premium;api;apis
description: Adobe TargetRecommendationsには、レコメンデーション可能な商品やコンテンツのカタログを管理するためのAPIの専用セットが含まれています。レコメンデーションのアルゴリズムとキャンペーンを管理します。Web、モバイル、電子メール、IOTなどのチャネルに表示するJSON、HTMLまたはXMLオブジェクトでレコメンデーションを配信します。
kt: 3815
audience: developer
doc-type: tutorial
activity: use
feature: api
topics: recommendations;adobe recommendations;premium;api;apis
solution: Target
author: Judy Kim
translation-type: tm+mt
source-git-commit: c221f434ce9daec03dbb4d897343775b40b14462
workflow-type: tm+mt
source-wordcount: '931'
ht-degree: 1%

---


# APIを使用した[!DNL Recommendations]カタログの管理

この時点で、JWT認証フローを使用してアクセストークンを生成し、Adobe I/OでAdobe Target管理APIを使用する方法を学びました。

[RecommendationsAPI](https://developers.adobetarget.com/api/recommendations/)を使用して、レコメンデーションカタログの品目の追加、更新または削除を行うことができます。 他のAdobe Target管理APIと同様に、[!DNL Recommendations] APIは認証が必要です。

>[!TIP]
>
>**[!UICONTROL IMSを送信：認証用にアクセストークンを更新する必要が生じた場合は、24時間後に期限切れになるので、JWT Generate + Auth via User Token]**&#x200B;リクエストを使用します。 手順については、[AdobeAPI認証の設定](../apis/configure-io-target-integration.md)を参照してください。

![JWT3ff](assets/configure-io-target-jwt3ff.png)

>[!NOTE]
>
>先に進む前に、[Recommendationsポストマンコレクション](https://developers.adobetarget.com/api/recommendations/#section/Postman)を取得します。

## エンティティの保存APIを使用した項目の作成と更新

製品ページで発生するCSV製品フィードや[!DNL Target]リクエストの代わりにAPIを使用して[!DNL Recommendations]製品データベースにデータを埋め込むには、[エンティティを保存API](https://developers.adobetarget.com/api/recommendations/#operation/saveEntities)を使用します。 このリクエストは、1つの[!DNL Target]環境内の項目を追加または更新します。 構文：

```
POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities
```

例えば、「エンティティの保存」を使用すると、在庫や価格のしきい値など、特定のしきい値を満たした場合に品目を更新し、品目にフラグを付けてレコメンデーションされないようにできます。

1. **[!DNL Target]/[!UICONTROL セットアップ]/[!UICONTROL ホスト]/[!UICONTROL 環境]**&#x200B;に移動し、アイテムを追加または更新する[!DNL Target]環境IDを取得します。

   ![SaveEntities1](assets/SaveEntities01.png)

2. `TENANT_ID`と`API_KEY`が、前に確立したPostman環境変数を参照していることを確認します。 比較には、次の画像を使用してください。 必要に応じて、APIリクエストのヘッダーとパスを、以下の画像のヘッダーとパスに一致するように変更します。

   ![SaveEntities3](assets/SaveEntities03.png)

3. JSONを&#x200B;**本文**&#x200B;に&#x200B;**raw**&#x200B;コードとして入力します。 `environment`変数を使用して、環境IDを指定するのを忘れないでください。 (次の例では、環境IDは6781です)。

   ![SaveEntities4.png](assets/SaveEntities04.png)

   >!![NOTE]
   以下のサンプルJSONは、Toaster Oven製品のエンティティ値を関連付けたentity.id kit2001を環境6781に追加します。

   ```
      {
      "entities": [{
              "name": "Toaster Oven",
              "id": "kit2001",
              "environment": 6781,
              "categories": [
                  "housewares:appliances"
              ],
              "attributes": {
                  "inventory": 77,
                  "margin": 23,
                  "message": "crashing helicopter",
                  "pageUrl": "www.foobar.foo.com/helicopter.html",
                  "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                  "value": 19.2
              }
          }]
      }
   ```

4. 「**送信**」をクリックします。次の回答を受け取ります。

   ![SaveEntities5.png](assets/SaveEntities05.png)

JSONオブジェクトは、複数の製品を送信するように拡大・縮小できます。 例えば、このJSONは2つのエンティティを指定します。

```
    {
        "entities": [{
                "name": "Toaster Oven",
                "id": "kit2001",
                "environment": 6781,
                "categories": [
                    "housewares:appliances"
                ],
                "attributes": {
                    "inventory": 89,
                    "margin": 11,
                    "message": "Toaster Oven",
                    "pageUrl": "www.foobar.foo.com/helicopter.html",
                    "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                    "value": 102.5
                }
            },
            {
                "name": "Blender",
                "id": "kit2002",
                "environment": 6781,
                "categories": [
                    "housewares:appliances"
                ],
                "attributes": {
                    "inventory": 36,
                    "margin": 5,
                    "message": "Blender",
                    "pageUrl": "www.foobar.foo.com/helicopter.html",
                    "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                    "value": 54.5
                }
            }
        ]
    }
```

1. 今度は君の番だ！ **エンティティを保存** APIを使用して、次の項目をカタログに追加します。 上記のサンプルJSONを基にしてください。 （JSONを拡張して、追加のエンティティを含める必要があります）。

   ![SaveEntities6.png](assets/SaveEntities06.png)

おお、この2つのアイテムは所属していないようだ。 **Get Entity** APIを使用してそれらを調べ、必要に応じて&#x200B;**Delete Entities** APIを使用して削除します。

## Get Entity APIを使用した品目の詳細の取得

既存の項目の詳細を取得するには、[Get Entity API](https://developers.adobetarget.com/api/recommendations/#operation/getEntity)を使用します。 構文：

```
GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities/[entity.id]
```

エンティティの詳細は、一度に1つのエンティティに対してのみ取得できます。 「エンティティの取得」を使用すると、カタログ内で更新が行われたことを期待どおりに確認したり、カタログの内容を監査したりできます。

1. APIリクエストで、変数`entityId`を使用してエンティティIDを指定します。 次の例では、entityId=kit2004を持つエンティティの詳細が返されます。

   ![GetEntity1](assets/GetEntity1.png)

2. `TENANT_ID`と`API_KEY`が、前に確立したPostman環境変数を参照していることを確認します。 比較には、次の画像を使用してください。 必要に応じて、APIリクエストのヘッダーとパスを、以下の画像のヘッダーとパスに一致するように変更します。

   ![GetEntity2](assets/GetEntity2.png)

3. リクエストを送信します。

   ![GetEntity3](assets/GetEntity3.png)
上の例に示すように、エンティティが見つからなかったというエラーが発生した場合は、リクエストを正しい [!DNL Target] 環境に送信していることを確認してください。

   >[!NOTE]
   環境が明示的に指定されていない場合、Get Entityは、[デフォルトの環境](https://docs.adobe.com/content/help/en/target/using/administer/hosts.html#section_4F8539B07C0C45E886E8525C344D5FB0)からのみエンティティの取得を試みます。 デフォルトの環境以外の環境から取り込む場合は、環境IDを指定する必要があります。

4. 必要に応じて、`environmentId`パラメーターを追加し、リクエストを再送信します。

   ![GetEntity4](assets/GetEntity4.png)

5. 別の&#x200B;**Get Entity**&#x200B;リクエストを送信します。今回は、entityId=kit2005を持つエンティティを検査します。

   ![GetEntity5](assets/GetEntity5.png)

これらのエンティティをカタログから削除する必要があると判断したとします。 **エンティティの削除** APIを使用します。

## エンティティの削除APIを使用した項目の削除

カタログから品目を削除するには、[エンティティを削除API](https://developers.adobetarget.com/api/recommendations/#operation/deleteEntities)を使用します。 構文：

```
DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities?ids=[comma-delimited-entity-ids]&environment=[environmentId]
```

>[!WARNING]
このAPIは、指定したIDで参照されているエンティティを削除します。
エンティティIDが指定されない場合、指定された環境内のすべてのエンティティが削除されます。 環境IDを指定しない場合、エンティティはすべての環境から削除されます。 これは注意して使用してください。

1. **[!DNL Target]/[!UICONTROL セットアップ]/[!UICONTROL ホスト]/[!UICONTROL 環境]**&#x200B;に移動し、項目を削除する[!DNL Target]環境IDを取得します。

   ![DeleteEntities1](assets/SaveEntities01.png)

2. APIリクエストで、構文`&ids=[comma-delimited-entity-ids]`(クエリパラメーター)を使用して、削除するエンティティのエンティティIDを指定します。 複数のエンティティを削除する場合は、IDをコンマで区切ります。

   ![DeleteEntities2](assets/DeleteEntities2.png)

3. 構文`&environment=[environmentId]`を使用して環境IDを指定します。指定しない場合は、すべての環境のエンティティが削除されます。

   ![DeleteEntities3](assets/DeleteEntities3.png)

4. `TENANT_ID`と`API_KEY`が、前に確立したPostman環境変数を参照していることを確認します。 比較には、次の画像を使用してください。 必要に応じて、APIリクエストのヘッダーとパスを、以下の画像のヘッダーとパスに一致するように変更します。

   ![DeleteEntities4](assets/DeleteEntities4.png)

5. リクエストを送信します。

   ![DeleteEntities5](assets/DeleteEntities5.png)

6. **エンティティ**&#x200B;を取得を使用して結果を確認します。これは、削除されたエンティティが見つからないことを示すはずです。

   ![DeleteEntities6](assets/DeleteEntities6.png)

   ![DeleteEntities6](assets/DeleteEntities7.png)

おめでとう！ [!DNL Recommendations] APIを使用して、カタログ内のエンティティの作成、更新、削除、および詳細の取得ができるようになりました。 次の節では、カスタム条件の管理方法について学びます。

[次の「カスタム条件の管理」>](manage-custom-criteria.md)
