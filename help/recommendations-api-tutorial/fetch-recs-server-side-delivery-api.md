---
title: Delivery API でRecommendationsを取得する方法
description: チュートリアルのこの部分では、Adobe Target Delivery API を使用して Recommendations コンテンツを取得するために必要な手順を開発者にガイドします。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: 553d1208-647f-479d-acc7-d7760469d642
source-git-commit: cee2618bb92284da1f82d108a0aff0d39340a15b
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 1%

---

# 取得中 [!DNL Recommendations] と Delivery API の連携

Adobe TargetとAdobe Target [!DNL Recommendations] API を使用して Web ページにHTMLを配信できますが、アプリ、画面、コンソール、電子メール、キオスク、その他のディスプレイデバイスを含む、レスポンスベース以外のエクスペリエンスでも使用できます。 つまり、 [!DNL Target] ライブラリと JavaScript は使用できず、 **[!DNL Target]配信 API** 今でも、私たちは全範囲の [!DNL Target] 機能を使用してパーソナライズされたエクスペリエンスを提供します。

>[!NOTE]
>
> 実際のレコメンデーション（推奨商品または品目）を含むコンテンツをリクエストする場合は、 [!DNL Target] 配信 API。

レコメンデーションを取得するには、適切なコンテキスト情報を使用してAdobe Target Delivery API のPOST呼び出しを送信します。これには、ユーザー ID（ユーザーの最近表示された項目などのプロファイル固有のレコメンデーションで使用）、関連する mbox 名、mbox パラメーター、プロファイルパラメーター、その他の属性が含まれます。 応答には、推奨される entity.ids（および他のエンティティデータを含む場合もあります）が JSON 形式またはHTML形式で含まれ、デバイスに表示できます。

この [配信 API](https://developer.adobe.com/target/implement/delivery-api/)Adobe Targetの {target=&quot;_blank&quot;} では、標準の [!DNL Target] リクエストによって提供されます。

>[!NOTE]
>Delivery API は次の操作を実行します。
>* 場所とオーディエンスのエクスペリエンスまたはオファーを RESTful な方法で取得できます。
>* 認証が不要です。
>* POST のみ。
>* Cookie やリダイレクト呼び出しを処理しません。
>* 「ユーザーの役割」を必要とせず、認識もしません。 単にコンテンツを取得したり、イベントを [!DNL Target] エッジサーバー。


Delivery API を使用してを配信するには、以下を実行します。 [!DNL Target] エクスペリエンス — レコメンデーションを含め、次の手順に従います。

1. の作成 [!DNL Target] アクティビティ (A/B、XT、AP、または [!DNL Recommendations]) を Visual Experience Composer ではなく、フォームベースの Composer を使用して追加する必要があります。
2. Delivery API を使用して、 [!DNL Target] 作成したアクティビティ。

<!-- Q: Why are BOTH steps necessary for this? If you have a Form-based recommendation defined for an mbox, what's the point/benefit of ALSO having the Delivery API step in to retrieve results? Why can't you just have the Form-based Rec deliver the results in the destination device...?? A: See use case below... it's when you want to "intercept" the pending results in order to do more stuff prior to displaying the results. Things like real-time comparisons to inventory levels. -->

## フォームベースの Experience Composer を使用したレコメンデーションの作成

Delivery API で使用できるレコメンデーションを作成するには、 [フォームベースのコンポーザー](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en).

1. 最初に、レコメンデーションで使用する JSON ベースのデザインを作成して保存します。 サンプルの JSON と、フォームベースのアクティビティを設定する際に JSON 応答を返す方法に関する背景情報については、 [レコメンデーションデザインの作成](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html?lang=en). この例では、デザインの名前はです。 *単純な JSON です。*

   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

2. In [!DNL Target]に移動します。 **[!UICONTROL アクティビティ] > [!UICONTROL アクティビティを作成] > [!UICONTROL Recommendations]**&#x200B;を選択し、「 **[!UICONTROL フォーム]**.

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

3. プロパティを選択し、 **[!UICONTROL 次へ]**.
4. ユーザーがレコメンデーションの応答を受け取る場所を定義します。 次の例では、という名前の場所を使用しています。 *api_charter*. 以前に作成した、という名前の JSON ベースのデザインを選択します。 *単純な JSON です。*
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
5. レコメンデーションを保存し、アクティブ化します。 結果が生成されます。 [結果の準備が整ったら](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html?lang=en)に値を入力しない場合は、Delivery API を使用してそれらを取得できます。

## Delivery API の使用

の構文 [配信 API](https://developer.adobe.com/target/implement/delivery-api/#tag/Delivery-API){target=&quot;_blank&quot;} は次の値です。

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. クライアントコードは必須です。 クライアントコードは、 **[!UICONTROL Recommendations] > [!UICONTROL 設定]**. 次の点に注意してください。 **[!UICONTROL クライアントコード]** 値を **[!UICONTROL レコメンデーション API トークン]** 」セクションに入力します。
   ![client-code.png](assets/client-code.png)
1. クライアントコードを入手したら、Delivery API 呼び出しを作成します。 次の例は、 **[!UICONTROL Web のバッチ処理された mbox Delivery API 呼び出し]** 指定された [配信 API Postmanコレクション](https://developers.adobetarget.com/api/delivery-api/#section/Getting-Started/Postman-Collection)、関連する変更をおこないます。 次に例を示します。
   * の **ブラウザー** および **住所** オブジェクトが **本文**( 非HTMLの使用例には必要ないため )
   * *api_charter* は、この例ではロケーション名として表示されます。
   * entity.id が指定されているので、このレコメンデーションはコンテンツの類似性に基づいているので、現在の項目キーをに渡す必要があります。 [!DNL Target].
      ![server-side-delivery-API-call.png](assets/server-side-delivery-api-call2.png)
必ずクエリパラメーターを正しく設定してください。 例えば、必ず 
`{{CLIENT_CODE}}` 必要に応じて。 <!--Q: In the updated call syntax, entity.id is listed as a profileParameter instead of an mboxParameter as in older versions. --> <!--Q: Old image ![server-side-create-recs-post.png](assets/server-side-create-recs-post.png) Old accompanying text: "Note this recommendation is based on Content Similar products based on the entity.id sent via mboxParameters." -->
      ![client-code3](assets/client-code3.png)
1. リクエストを送信します。 これは、 *api_charter* 場所：アクティブなレコメンデーションが実行され、JSON デザインで定義されており、レコメンデーションされたエンティティのリストを出力します。
1. JSON デザインに基づいて応答を受け取ります。
   ![server-side-create-recs-json-response2.png](assets/server-side-create-recs-json-response2.png)
応答には、キー ID と、レコメンデーションエンティティのエンティティ ID が含まれます。

Delivery API を [!DNL Recommendations] この方法を使用すると、訪問者にレコメンデーションを表示する前に、HTML以外のデバイスで追加の手順を実行できます。 例えば、Delivery API からの応答を使用して、最終結果を表示する前に、別のシステム（CMS、PIM、e コマースプラットフォームなど）からエンティティ属性の詳細（在庫、価格、評価など）をリアルタイムで追加検索できます。

このチュートリアルで概要を説明しているアプローチを使用すると、次の応答を活用する任意のアプリケーションを取得できます： [!DNL Target] パーソナライズされたレコメンデーションを提供するには

## 実装例

以下のリソースは、HTMLに重点を置いていない様々な実装の例を示しています。 関連するシステムとデバイスにより、すべての実装が一意になることに注意してください。

| リソース | 詳細 |
| --- | --- |
| [Adobe Target Everywhere - Server 側または IoT での実装](https://expleague.azureedge.net/labs/L733/index.html) | Adobe Targetのサーバー側 API を利用した React アプリケーションの実践的な操作を提供するAdobe Summit2019 Lab。 |
| [AdobeSDK を使用しないモバイルアプリでのAdobe Target](https://community.tealiumiq.com/t5/Universal-Data-Hub/Adobe-Target-in-a-Mobile-App-Without-the-Adobe-SDK/ta-p/26753) | このガイドでは、AdobeSDK をインストールせずにモバイルアプリでAdobe Targetをセットアップする方法を説明します。 このソリューションでは、Tealium SDK Web ビューと Remote Commands モジュールを使用して、Adobe訪問者 API(Experience Cloud) およびAdobe Target API にリクエストを送受信します。 |
| [モバイルアプリにおけるAdobe Targetの仕組み](https://experienceleague.adobe.com/docs/target/using/implement-target/mobile-apps/mobile-how-target-works-mobile-apps.html?lang=en) | 方法 [!DNL Target] は Mobile SDK で動作します |
| [の設定 [!DNL Target] Experience Platform Launchと実装の拡張 [!DNL Target] API](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/adobe-target) | 設定の手順 [!DNL Target] Experience Platform Launchの拡張，追加 [!DNL Target] アプリの拡張機能と実装 [!DNL Target] アクティビティをリクエストする API、オファーをプリフェッチする API、ビジュアルプレビューモードに入ります。 |
| [Adobe Target Node Client](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | オープンソース [!DNL Target] Node.js SDK v1.0 |
| [サーバー側の概要](https://experienceleague.adobe.com/docs/target/using/implement-target/server-side/api-and-sdk-overview.html?lang=en) | Adobe Target Server Side Delivery API、Server Side Batch Delivery API、Node.js SDK、Adobe Targetに関する情報です [!DNL Recommendations] API |
| [Adobe Campaign Content Recommendations in Email](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | Adobe CampaignのAdobe TargetとAdobe I/O Runtimeを介して電子メールでコンテンツレコメンデーションを活用する方法を説明するブログ。 |

## 管理 [!DNL Recommendations] API を使用した設定

ほとんどの場合、レコメンデーションはAdobe Target UI で設定され、 [!DNL Target] API（上記の節で説明したものなど） この UI と API の調整は一般的です。 ただし、API を介してすべてのアクション（セットアップと結果の使用）を実行したい場合があります。 一般的ではありませんが、ユーザーは、 *および* は、レコメンデーションの結果を API を完全に使用して活用します。

私たちは、 [前の節](https://developer.adobe.com/target/before-administer/recs-api/manage-catalog/){target=&quot;_blank&quot;} Adobe Target Recommendationsエンティティを管理し、サーバー側で配信する方法。 同様に、Adobe I/Oを使用すると、Adobe Targetにログインしなくても、条件、プロモーション、コレクション、デザインテンプレートを管理できます。 すべての [!DNL Recommendations] API が見つかる場合があります [ここ](https://developers.adobetarget.com/api/recommendations/)ですが、参照用の要約を次に示します。

| リソース | 詳細 |
| --- | --- |
| [コレクション](https://developers.adobetarget.com/api/recommendations/#tag/Collections) | コレクションのリスト、作成、取得、編集、削除を行います。 |
| [条件](https://developers.adobetarget.com/api/recommendations/#tag/Criteria) | 条件のリストと取得。 |
| [デザイン](https://developers.adobetarget.com/api/recommendations/#tag/Designs) | デザインのリスト、作成、取得、編集、削除、検証を行います。 |
| [エンティティ](https://developers.adobetarget.com/api/recommendations/#tag/Entities) | エンティティを保存、削除、取得します。 |
| [プロモーション](https://developers.adobetarget.com/api/recommendations/#tag/Promotions) | プロモーションのリスト、作成、取得、編集、削除を行います。 |
| [カテゴリ条件](https://developers.adobetarget.com/api/recommendations/#tag/Category-Criteria) | カテゴリ条件のリスト、作成、取得、編集、削除を行います。 |
| [カスタム条件](https://developers.adobetarget.com/api/recommendations/#tag/Custom-Criteria) | カスタム条件のリスト、作成、取得、編集および削除を行います。 |
| [項目条件](https://developers.adobetarget.com/api/recommendations/#tag/Item-Criteria) | 項目基準のリスト、作成、取得、編集、削除を行います。 |
| [人気度の条件](https://developers.adobetarget.com/api/recommendations/#tag/Popularity-Criteria) | 人気度条件のリスト、作成、取得、編集および削除を行います。 |
| [プロファイル属性条件](https://developers.adobetarget.com/api/recommendations/#tag/Profile-Attribute-Criteria) | プロファイル属性条件のリスト、作成、取得、編集、削除を行います。 |
| [最近の条件](https://developers.adobetarget.com/api/recommendations/#tag/Recent-Criteria) | 最近の条件のリスト、作成、取得、編集、削除を行います。 |
| [シーケンス条件](https://developers.adobetarget.com/api/recommendations/#tag/Sequence-Criteria) | シーケンス条件をリスト、作成、取得、編集および削除します。 |

## リファレンスドキュメント

* [Adobe Target Admin API ドキュメント](https://developer.adobe.com/target/administer/admin-api/){target=&quot;_blank&quot;}
* [Adobe Target Delivery API](https://developer.adobe.com/target/implement/delivery-api/){target=&quot;_blank&quot;}
* [ [!DNL Recommendations]  メールとの統合](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/integrating-recs-email.html)

## 概要とレビュー

おめでとうございます。 このチュートリアルを終了して、次の方法を学びました。
* [Recommendations API を使用したカタログの管理](https://developer.adobe.com/target/before-administer/recs-api/manage-catalog/){target=&quot;_blank&quot;}
* [Recommendations API を使用したカスタム条件の管理](https://developer.adobe.com/target/before-administer/recs-api/manage-custom-criteria/){target=&quot;_blank&quot;}
* [Recommendationsでの Delivery API の使用](https://developer.adobe.com/target/before-administer/recs-api/fetch-recs-server-side-delivery-api/){target=&quot;_blank&quot;}
