---
title: Adobe TargetAPIの概要
keywords: recommendations;adobe recommendations;premium;api;apis
description: Adobe TargetRecommendationsには、レコメンデーション可能な商品やコンテンツのカタログを管理するためのAPIの専用セットが含まれています。 レコメンデーションのアルゴリズムとキャンペーンを管理します。 Web、モバイル、電子メール、IOTなどのチャネルに表示するJSON、HTMLまたはXMLオブジェクトでレコメンデーションを配信します。
kt: null
audience: developer
doc-type: tutorial
activity: use
feature: api
topics: recommendations;adobe recommendations;premium;api;apis
solution: Adobe Target
author: Judy Kim
translation-type: tm+mt
source-git-commit: b66dbae616c9559f5d1b7bbedf2d9b383840973b
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 2%

---


# Adobe TargetAPIの概要

Adobe TargetAPIは、タイプ別にグループ化できます。

| API のタイプ | 何が可能か | ダウンロードリンク |
| --- | --- | --- |
| 管理者 | アクティビティ、オーディエンス、オファー、およびその他のオブジェクト（エンティティ、基準、設計などを含む）を作成、変更、削除します。 [!DNL Recommendations] APIは管理APIの一種です。) [!DNL Recommendations] | <UL><li>[ターゲット管理APIポストマンコレクション](https://developers.adobetarget.com/api/#admin-postman-collection)</li><li>[RecommendationsAPIポストマンコレクション](https://developers.adobetarget.com/api/recommendations/#section/Postman)</li></ul> |
| 配信 | エンドユーザーに対して配信を行うた [!DNL Target] めに、最適化され、パーソナライズされたコンテンツを取得します。 | [ターゲット配信API Postmanコレクション](https://developers.adobetarget.com/api/delivery-api/#section/Getting-Started/Postman-Collection) |
| レポート | アクティビティ結果とその他のレポート結果を書き出します。 | レポートAPIは、 [ターゲット管理API Postmanコレクションに含まれています](https://developers.adobetarget.com/api/#admin-postman-collection)。 |
| プロファイル | Adobe Targetに保存されているユーザプロファイルを取得して変更します。 | [ターゲットプロファイルAPI Postmanコレクション](https://developers.adobetarget.com/api/#profiles) |

Adobe Targetの様々な側面を設定できる **管理API** （APIを含む）と、コンテンツを取得できる [!DNL Recommendations] 配信API ****(API)の違いに注意してください。 管理APIは認証が必要ですが、配信APIは認証を必要としません。

Adobe Target管理APIを使用するには、まずAdobeI/Oを使用して認証を設定する必要があります。

[次の「認証の設定」>](configure-io-target-integration.md)
