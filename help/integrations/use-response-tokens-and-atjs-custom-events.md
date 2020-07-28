---
title: 応答トークンとat.jsカスタムイベントのAdobe Targetでの使用
seo-title: 応答トークンとat.jsカスタムイベントのAdobe Targetでの使用
description: 応答トークンとat.jsカスタムイベントを使用すると、Targetからサードパーティ製システムにプロファイル情報を共有できます。 カスタムプロファイル属性、地理情報、アクティビティの詳細、組み込みプロファイルなど、Target訪問者プロファイル内の任意のオブジェクトをTargetの応答に追加し、カスタムJavaScriptを使用してサードパーティと統合できます。
audience: developer
difficulty: 5
author: Daniel Wright
doc-type: implement
activity-type: technical-video
translation-type: tm+mt
source-git-commit: b331bb29c099bd91df27300ebe199cafa12516db
workflow-type: tm+mt
source-wordcount: '284'
ht-degree: 2%

---


# 応答トークンとat.jsカスタムイベントのAdobe Targetでの使用

応答トークンと `at.js` カスタムイベントを使用すると、サードパーティ製システム [!DNL Target] 間でプロファイル情報を共有できます。 カスタムプロファイル属性、地理情報、アクティビティの詳細、組み込みプロファイルなど、 [!DNL Target] 訪問者プロファイル内の任意のオブジェクトを [!DNL Target] 応答に追加して、カスタムJavaScriptを使用してサードパーティと統合できます。

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)

## 応答トークンとat.jsカスタムイベントの使用方法

1. 必要なデータを [!DNL Target]
1. 「設定」 —>「応答トークン」画面で切り替えを反転して、必要なデータに対する応答トークンをオンにします。
1. 使用する必要があるイベントリスナーを決定する
1. Adobe Targetイベントをリッスンするために必要なJavaScriptを作成し、応答トークンを読み取って、統合に必要な処理を行います
1. 「イベントの読み込み」アクションの後の「起動」のカスタムコードアクションを使用してTargetリスナーJavaScriptをデプロイするか、セットアップ/実装画面のat.jsの「ライブラリフッター」セクションに追加して、新しいat.jsファイルを保存します
1. 統合のQAおよび公開

## その他のリソース

* [Adobe TargetでExperience Cloud Debuggerを使用する](../troubleshooting/troubleshoot-with-the-experience-cloud-debugger.md)
* [レスポンストークンのドキュメント](https://docs.adobe.com/help/en/target/using/administer/response-tokens.html)
* [at.jsカスタムイベントドキュメント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/atjs-custom-events.html)
* [Adobe Target でのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)