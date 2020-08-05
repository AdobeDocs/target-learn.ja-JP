---
title: 配信APIを使用したRecommendationsの取得
keywords: recommendations;adobe recommendations;premium;api;apis
description: Adobe TargetRecommendationsには、レコメンデーション可能な商品やコンテンツのカタログを管理するためのAPIの専用セットが含まれています。 レコメンデーションのアルゴリズムとキャンペーンを管理します。 Web、モバイル、電子メール、IOTなどのチャネルに表示するJSON、HTMLまたはXMLオブジェクトでレコメンデーションを配信します。
kt: 3815
audience: developer
doc-type: tutorial
activity: use
feature: api
topics: recommendations;adobe recommendations;premium;api;apis
solution: Adobe Target
author: Judy Kim
translation-type: tm+mt
source-git-commit: 18a9b664fe935fd5c52682b2bd798cafd75b6591
workflow-type: tm+mt
source-wordcount: '1473'
ht-degree: 0%

---


# 配信API [!DNL Recommendations] を使用した取得

Adobe TargetAPIとAdobe TargetAPIはWebページへの応答の配信に使用できますが、アプリ、画面、コンソール、電子メール、キオスク、その他のディスプレイデバイスなど、HTML以外のベースのエクスペリエンスでも使用できます。 [!DNL Recommendations] 言い換えると、 [!DNL Target] ライブラリとJavaScriptを使用できない場合でも、 **[!DNL Target]配信API **(Library And JavaScript)を使用すれば、パーソナライズされたエクスペリエンスを提供するための[!DNL Target]全機能にアクセスできます。

> [!NOTE]
> 実際のレコメンデーション（推奨商品や商品）を含むコンテンツをリクエストする場合は、 [!DNL Target] 配信APIを使用します。

レコメンデーションを取得するには、Adobe Target配信APIPOST呼び出しを適切なコンテキスト情報と共に送信します。これには、ユーザーID(最近表示した品目など、プロファイル固有のレコメンデーションで使用)、関連するmbox名、mboxパラメーター、プロファイルパラメーター、その他の属性が含まれます。 この応答には、推奨されるentity.ids（および他のエンティティデータを含む場合もあります）がJSON形式またはHTML形式で含まれ、これらはデバイスに表示できます。

Adobe Targetの [配信API](https://developers.adobetarget.com/api/delivery-api/) （英語）は、標準 [!DNL Target] リクエストが提供する既存の機能をすべて公開します。

>[!NOTE]
>配信API:
>* 場所とオーディエンスのエクスペリエンスまたはオファーをRESTful方法で取得できます。
>* 認証は不要です。
>* POSTのみ。
>* cookieまたはリダイレクト呼び出しを処理しません。
>* 「ユーザーの役割」を必要としないか、認識しません。 単純にコンテンツを取得するか、 [!DNL Target] エッジサーバーにイベントをレポートします。


配信APIを使用してレコメンデーションなどの [!DNL Target] エクスペリエンスを提供するには、次の手順に従います。

1. （Visual Experience Composerではなく）フォームベースの [!DNL Target] Composerを使用して [!DNL Recommendations]アクティビティ（A/B、XT、APまたは）を作成します。
2. 配信APIを使用して、作成した [!DNL Target] アクティビティによって生成されたリクエストに対する応答を取得します。

<!-- Q: Why are BOTH steps necessary for this? If you have a Form-based recommendation defined for an mbox, what's the point/benefit of ALSO having the Delivery API step in to retrieve results? Why can't you just have the Form-based Rec deliver the results in the destination device...?? A: See use case below... it's when you want to "intercept" the pending results in order to do more stuff prior to displaying the results. Things like real-time comparisons to inventory levels. -->

## フォームベースのExperience Composerを使用したレコメンデーションの作成

配信APIで使用できるレコメンデーションを作成するには、 [フォームベースのComposer](https://docs.adobe.com/content/help/en/target/using/experiences/form-experience-composer.html).

1. 最初に、レコメンデーションで使用するJSONベースのデザインを作成し、保存します。 サンプルJSONの他に、フォームベースのアクティビティの設定時にJSON応答を返す方法に関する背景情報が記載されています。推奨デザインの [作成に関するドキュメントを参照してください](https://docs.adobe.com/content/help/en/target/using/recommendations/recommendations-design/create-design.html)。 この例では、デザインの名前は *Simple JSONです。*

   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

2. で [!DNL Target]、 **[!UICONTROL アクティビティ]/アクティビティを[!UICONTROL 作成]/Recommendationsに移動し、次に「**/を作成 ****/」を選択します。

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

3. プロパティを選択し、「 **[!UICONTROL 次へ]**」をクリックします。
4. レコメンデーションの応答を受け取るユーザーの場所を定義します。 次の例では、 *api_charterという名前の場所を使用しています*。 先ほど作成したJSONベースのデザインの名前は *Simple JSONです。*
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
5. レコメンデーションを保存してアクティブ化します。 結果が生成されます。 [結果の準備が整ったら](https://docs.adobe.com/content/help/en/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html)、配信APIを使用して結果を取得できます。

## 配信APIの使用

[配信APIの構文](https://developers.adobetarget.com/api/delivery-api/#tag/Delivery-API) :

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. クライアントコードは必須です。 お知らせしますが、クライアントコードは **[!UICONTROL Recommendations]/[!UICONTROL 設定に移動するとAdobe Targetに表示されます]**。 「 **[!UICONTROL Recommendation APIトークン]** 」セクションの「 **[!UICONTROL クライアントコード]** 」の値をメモしておきます。
   ![client-code.png](assets/client-code.png)
1. クライアントコードを入手したら、配信API呼び出しを作成します。 以下の例は、 **[!UICONTROL 配信API Postmanコレクションで提供される]** Webバッチmbox配信API呼び出しで始まり [](https://developers.adobetarget.com/api/delivery-api/#section/Getting-Started/Postman-Collection)、関連する変更を行います。 次に例を示します。
   * HTML以外の使用例では **不要なので、** browser **オブジェクトと** address **オブジェクトは** Bodyから削除されました。
   * *api_charter* は、この例で場所名として表示されます。
   * entity.idが指定されているのは、このレコメンデーションがコンテンツの類似性に基づいているためです。これには、現在の品目キーを渡す必要があり [!DNL Target]ます。
      ![server-side-配信-API-call.png](assets/server-side-delivery-api-call2.png)クエリパラメーターを正しく設定するように注意してください。 例えば、必要に応じてを指定して`{{CLIENT_CODE}}` ください。 <!--Q: In the updated call syntax, entity.id is listed as a profileParameter instead of an mboxParameter as in older versions. --> <!--Q: Old image ![server-side-create-recs-post.png](assets/server-side-create-recs-post.png) Old accompanying text: "Note this recommendation is based on Content Similar products based on the entity.id sent via mboxParameters." -->
      ![client-code3](assets/client-code3.png)
1. リクエストを送信します。 この処理は、 *api_charter* (推奨エンティティのリストを出力するJSONデザインで定義されたアクティブなレコメンデーションが実行されている)の場所に対して実行されます。
1. JSONデザインに基づいて応答を受け取ります。
   ![server-side-create-recs-json-response2.png](assets/server-side-create-recs-json-response2.png)この応答には、キーIDと推奨エンティティのエンティティIDが含まれます。

この方法で配信APIを使用す [!DNL Recommendations] ると、HTML以外のデバイスの訪問者にレコメンデーションを表示する前に、追加の手順を実行できます。 例えば、配信APIからの応答を受け取って、最終的な結果を表示する前に、別のシステム（CMS、PIM、eコマースプラットフォームなど）からエンティティ属性の詳細（在庫、価格、評価など）を追加のリアルタイムで参照できます。

このチュートリアルで概要を説明するアプローチを使用すると、の回答を活用する任意のアプリを入手して、パーソナライズされたレコメンデーション [!DNL Target] を提供できます。

## 実装例

次のリソースは、HTMLに重点を置いていない様々な実装の例を示しています。 システムとデバイスが関係するので、実装はすべて一意になることに注意してください。

| リソース | 詳細 |
| --- | --- |
| [AEMでのRESTful APIの使用](https://helpx.adobe.com/experience-manager/using/restful-services.html) | サードパーティのRESTful Webサービスのデータを使用するAdobe Experience ManagerOSGiバンドルを作成および展開する方法。 |
| [Adobe Targetの至る所に — サーバ側またはIoTに実装](https://expleague.azureedge.net/labs/L733/index.html) | Adobe Targetのサーバー側APIを使用するReactアプリケーションに対する実践体験を提供するAdobeサミット2019ラボ。 |
| [AdobeSDKを使用しないモバイルアプリでのAdobe Target](https://community.tealiumiq.com/t5/Universal-Data-Hub/Adobe-Target-in-a-Mobile-App-Without-the-Adobe-SDK/ta-p/26753) | このガイドでは、AdobeSDKをインストールせずにモバイルアプリでAdobe Targetを設定する方法を説明します。 このソリューションでは、Tealium SDK WebビューとRemote Commandsモジュールを使用して、Adobe訪問者API(Experience Cloud)とAdobe TargetAPIにリクエストを送受信します。 |
| [モバイルアプリでのAdobe Targetの仕組み](https://docs.adobe.com/content/help/en/target/using/implement-target/mobile-apps/mobile-how-target-works-mobile-apps.html) | モバイルSDK [!DNL Target] との連携 |
| [API [!DNL Target] extension in Experience Platform Launch and Implementing [!DNL Target] の設定](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/adobe-target) | Experience Platform Launchでの [!DNL Target] 拡張機能の設定、アプリへの拡張機能の追加、アクティビティの要求、オファーのプリフェッチ、ビジュアルプレビューモードの開始のための [!DNL Target][!DNL Target] APIの実装の手順を説明します。 |
| [Adobe Targetノードクライアント](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | オープンソースの [!DNL Target] Node.js SDK v1.0 |
| [サーバー側の概要](https://docs.adobe.com/content/help/en/target/using/implement-target/server-side/api-and-sdk-overview.html) | Adobe Targetサーバ側配信API、サーバ側バッチ配信API、Node.js SDK、Adobe TargetAPIについて取り上げ [!DNL Recommendations] ます。 |
| [Adobe CampaignコンテンツRecommendations（電子メール）](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | Adobe CampaignのAdobe TargetやAdobe I/O Runtime経由の電子メールでコンテンツのレコメンデーションを活用する方法を説明するブログ。 |

## APIを使用した [!DNL Recommendations] セットアップの管理

ほとんどの場合、レコメンデーションはAdobe TargetUIで設定され、上記の節で説明したような理由から、使用または [!DNL Target] API経由でアクセスされます。 このUI-APIの調整は一般的です。 ただし、場合によっては、設定と結果の使用の両方のAPIを使用して、すべての操作を実行したい場合があります。 一般的ではありませんが、ユーザーはAPIを使用してレコメンデーションの結果を完全に設定、実行 *、* 、活用することができます。

先の節で、 [Adobe TargetRecommendations企業を管理し、サーバー側で配信する方法を学びました](manage-catalog.md) 。 同様に、AdobeI/Oを使用すると、Adobe Targetにログインすることなく、条件、プロモーション、コレクション、デザインテンプレートを管理できます。 すべてのAPIの完全なリストはこちら [!DNL Recommendations] で確認できます [](http://developers.adobetarget.com/api/recommendations/)。ただし、参考用に要約を示します。

| リソース | 詳細 |
| --- | --- |
| [コレクション](http://developers.adobetarget.com/api/recommendations/#tag/Collections) | コレクションのリスト、作成、取得、編集および削除を行います。 |
| [条件](http://developers.adobetarget.com/api/recommendations/#tag/Criteria) | リストと条件の取得を参照してください。 |
| [デザイン](http://developers.adobetarget.com/api/recommendations/#tag/Designs) | デザインのリスト、作成、取得、編集、削除および検証を行います。 |
| [エンティティ](http://developers.adobetarget.com/api/recommendations/#tag/Entities) | エンティティを保存、削除、取得します。 |
| [プロモーション](http://developers.adobetarget.com/api/recommendations/#tag/Promotions) | リスト、プロモーションの作成、取得、編集、削除を行います。 |
| [カテゴリ条件](http://developers.adobetarget.com/api/recommendations/#tag/Category-Criteria) | カテゴリ条件のリスト、作成、取得、編集および削除を行います。 |
| [カスタム条件](http://developers.adobetarget.com/api/recommendations/#tag/Custom-Criteria) | カスタム条件のリスト、作成、取得、編集および削除を行います。 |
| [品目基準](http://developers.adobetarget.com/api/recommendations/#tag/Item-Criteria) | リスト条件、作成、取得、編集および削除を行います。 |
| [人気度基準](http://developers.adobetarget.com/api/recommendations/#tag/Popularity-Criteria) | 人気度条件のリスト、作成、取得、編集および削除を行います。 |
| [プロファイル属性の条件](http://developers.adobetarget.com/api/recommendations/#tag/Profile-Attribute-Criteria) | リスト、プロファイル属性条件の作成、取得、編集および削除を行います。 |
| [最近の条件](http://developers.adobetarget.com/api/recommendations/#tag/Recent-Criteria) | 最近使用した条件のリスト、作成、取得、編集および削除を行います。 |
| [シーケンス条件](http://developers.adobetarget.com/api/recommendations/#tag/Sequence-Criteria) | リスト、シーケンス条件の作成、取得、編集および削除を行います。 |

## リファレンスドキュメント

* [Adobe TargetAPIドキュメント](https://developers.adobetarget.com/api/#getting-started)
* [Adobe Target配信API](https://developers.adobetarget.com/api/delivery-api/)
* [電子メ [!DNL Recommendations] ールとの統合](https://docs.adobe.com/content/help/en/target/using/recommendations/recommendations-faq/integrating-recs-email.html)

## 概要とレビュー

おめでとう！ このチュートリアルを終了すると、次の方法を学習できます。
* [RecommendationsAPIを使用したカタログの管理](manage-catalog.md)
* [RecommendationsAPIを使用したカスタム条件の管理](manage-custom-criteria.md)
* [Recommendationsでの配信APIの使用](fetch-recs-server-side-delivery-api.md)
