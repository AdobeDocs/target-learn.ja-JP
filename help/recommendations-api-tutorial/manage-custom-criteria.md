---
title: カスタム条件の管理方法
description: チュートリアルのこの部分では、Adobe Target APIを使用してAdobe Target Recommendationsの条件を管理、作成、リスト、編集、取得、削除するために必要な手順を開発者に説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: ee63bd3e-200a-4c08-b364-9f17a479033b
source-git-commit: d1517f0763290eb61a9e4eef4f2eb215a9cdd667
workflow-type: tm+mt
source-wordcount: '941'
ht-degree: 1%

---

# カスタム条件の管理

[!DNL Recommendations]が提供するアルゴリズムが、プロモーションしたい特定の項目を表示できない場合があります。 このような状況では、カスタム条件を使用すると、特定のキー品目またはカテゴリに対して、レコメンデーション品目の特定のセットを配信できます。 主要品目またはカテゴリと推奨品目の間のマッピングを定義し、そのマッピングをカスタム条件としてインポートします。 このプロセスは、[カスタム条件のドキュメント](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html?lang=en)で説明しています。 このドキュメントで述べたように、 [!DNL Target]ユーザーインターフェイス(UI)を使用して、カスタム条件の作成、編集、削除をおこなうことができます。 ただし、[!DNL Target]には、カスタム条件をより詳細に管理するためのカスタム条件APIのセットも用意されています。

>[!IMPORTANT]
>
>カスタム条件については、次の使用ガイドラインに従います。
>
> APIを使用して、特定のカスタム条件に対してすべての操作（作成、編集、削除）をおこなうか、またはUIを使用してすべての操作（作成、編集、削除）をおこないます。 UIとAPIを組み合わせてカスタム条件を管理すると、情報が競合したり、予期しない結果が生じる場合があります。 例えば、UIでカスタム条件を作成し、APIで編集した場合、APIで表示されるようにバックエンドで更新されても、UIでの更新は反映されません。

## カスタム条件の作成

[カスタム条件を作成API](https://developers.adobetarget.com/api/recommendations/#operation/createCriteriaCustom)を使用してカスタム条件を作成するには、次の構文を使用します。

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

>[!WARNING]
>
>カスタム条件を作成APIを使用して作成したカスタム条件は、この演習で説明するように、UIに表示され、そこで保持されます。 UIから編集や削除を行うことはできません。 API **を使用して**&#x200B;を編集または削除できますが、どちらの方法でも、 [!DNL Target] UIに引き続き表示されます。 UIでの編集または削除のオプションを維持するには、カスタム条件を作成APIではなく、[ドキュメント](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html?lang=en)ごとにUIを使用してカスタム条件を作成します。

上記の警告を読んだ後で、このチュートリアルを続行し、UIから削除できない新しいカスタム条件を作成することをお勧めします。

1. **の`TENANT_ID`と`API_KEY`を確認し、前に確立したPostman環境変数を参照してカスタム条件**&#x200B;を作成します。 以下の画像を比較に使用してください。

   ![CreateCustomCriteria1](assets/CreateCustomCriteria1.png)

2. **Body**&#x200B;を、カスタム条件のCSVファイルの場所を定義する&#x200B;**raw** JSONとして追加します。 [カスタム条件の作成API](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom)のドキュメントに記載されている例をテンプレートとして使用し、必要に応じて`environmentId`とその他の値を指定します。 この例では、 LAST_PURCHASEDをキーとして使用します。

   ![CreateCustomCriteria2](assets/CreateCustomCriteria2.png)

3. リクエストを送信し、作成したカスタム条件の詳細を含む応答を観察します。

   ![CreateCustomCriteria3](assets/CreateCustomCriteria3.png)

4. カスタム条件が作成されたことを確認するには、Adobe Target内で&#x200B;**[!UICONTROL Recommendations] / [!UICONTROL 条件]**&#x200B;に移動し、名前で条件を検索するか、次の手順で&#x200B;**カスタム条件APIのリスト**&#x200B;を使用します。

   ![CreateCustomCriteria4](assets/CreateCustomCriteria4.png)

この場合、エラーが発生します。 **カスタム条件のリストAPI**&#x200B;を使用して、カスタム条件をより詳細に調べて、エラーを調べます。

## カスタム条件のリスト

すべてのカスタム条件のリストと各条件の詳細を取得するには、[カスタム条件のリストAPI](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom)を使用します。 構文は次のとおりです。

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

1. 前と同様に`TENANT_ID`と`API_KEY`を確認し、要求を送信します。 応答では、カスタム条件IDと、前述のエラーメッセージに関する詳細をメモします。
   ![ListCustomCriteria](assets/ListCustomCriteria.png)

この場合、サーバー情報が正しくない、つまり[!DNL Target]がカスタム条件の定義を含むCSVファイルにアクセスできないことが原因で、エラーが発生しました。 カスタム条件を編集して修正しましょう。

## カスタム条件の編集

カスタム条件定義の詳細を変更するには、[カスタム条件を編集API](https://developers.adobetarget.com/api/recommendations/#operation/updateCriteriaCustom)を使用します。 構文は次のとおりです。

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. `TENANT_ID`と`API_KEY`を前と同様に確認します。
   ![EditCustomCriteria1](assets/EditCustomCriteria1.png)

1. 編集する（単一の）カスタム条件の条件IDを指定します。
   ![EditCustomCriteria2](assets/EditCustomCriteria2.png)

1. 本文で、更新されたJSONに正しいサーバー情報を指定します。 （この手順では、アクセス可能なサーバーへのFTPアクセスを指定します）。
   ![EditCustomCriteria3](assets/EditCustomCriteria3.png)

1. リクエストを送信し、応答をメモします。
   ![EditCustomCriteria4](assets/EditCustomCriteria4.png)

**カスタム条件API**&#x200B;を取得を使用して、更新されたカスタム条件の成功を検証します。

## カスタム条件の取得

特定のカスタム条件のカスタム条件の詳細を表示するには、[カスタム条件APIを取得](https://developers.adobetarget.com/api/recommendations/#operation/getCriteriaCustom)します。 構文は次のとおりです。

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. 詳細を取得するカスタム条件の条件IDを指定します。 リクエストを送信し、応答を確認します。
   ![GetCustomCriteria.png](assets/GetCustomCriteria.png)
1. 成功を確認します。 （この場合は、それ以上FTPエラーがないことを確認します）。
   ![GetCustomCriteria1.png](assets/GetCustomCriteria1.png)
1. （オプション）UIに更新が正しく反映されていることを確認します。
   ![GetCustomCriteria2.png](assets/GetCustomCriteria2.png)

## カスタム条件の削除

前述の条件IDを使用して、[カスタム条件を削除API](https://developers.adobetarget.com/api/recommendations/#operation/deleteCriteriaCustom)を使用してカスタム条件を削除します。 構文は次のとおりです。

`DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. 削除する（単一の）カスタム条件の条件IDを指定します。 「**送信**」をクリックします。
   ![DeleteCustomCriteria1](assets/DeleteCustomCriteria1.png)

1. 「カスタム条件を取得」を使用して、条件が削除されたことを確認します。
   ![DeleteCustomCriteria2](assets/DeleteCustomCriteria2.png)
この場合、404エラーが発生すると、削除された条件が見つからないことが示されます。

>[!NOTE]
>なお、条件はカスタム条件を作成APIを使用して作成されたので、削除されても[!DNL Target] UIから削除されません。

おめでとう！ [!DNL Recommendations] APIを使用して、カスタム条件の作成、リスト、編集、削除、詳細の取得をおこなうことができるようになりました。 次の節では、[!DNL Target] Delivery APIを使用してレコメンデーションを取得します。

[次：「サーバー側配信APIを使用したRecommendationsの取得」>](fetch-recs-server-side-delivery-api.md)
