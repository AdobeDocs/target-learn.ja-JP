---
title: 応答トークンとat.jsカスタムイベントの使用方法
description: 応答トークンとat.jsカスタムイベントを使用して、ターゲットからサードパーティ製システムにプロファイル情報を共有する方法を説明します。
role: 開発者
level: 経験豊富な
topic: パーソナライゼーション、アーキテクチャ、開発
feature: 実装
doc-type: technical video
kt: null
thumbnail: null
author: Daniel Wright
translation-type: tm+mt
source-git-commit: b89732fcca0be8bffc6e580e4ae0e62df3c3655d
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 4%

---


# 応答トークンとat.jsカスタムイベントのAdobe Targetでの使用

応答トークンと`at.js`カスタムイベントを使用すると、[!DNL Target]からサードパーティ製システムにプロファイル情報を共有できます。 カスタムプロファイル属性、地理情報、アクティビティの詳細、組み込みプロファイルなど、[!DNL Target]訪問者プロファイル内の任意のオブジェクトを[!DNL Target]応答に追加して、カスタムJavaScriptを使用してサードパーティと統合できます。

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)

## 応答トークンとat.jsカスタムイベントの使用方法

1. [!DNL Target]から必要なデータを決定
1. 「設定」 —>「応答トークン」画面で切り替えを反転して、必要なデータに対する応答トークンをオンにします。
1. 使用する必要があるイベントリスナーを決定する
1. Adobe Targetイベントをリッスンするために必要なJavaScriptを作成し、応答トークンを読み取って、統合に必要な処理を行います
1. 「イベントの読み込み」アクションの後の「起動」のカスタムコードアクションを使用してターゲットリスナーJavaScriptをデプロイするか、セットアップ/実装画面のat.jsの「ライブラリフッター」セクションに追加して、新しいat.jsファイルを保存します
1. 統合のQAおよび公開

## その他のリソース

* [Adobe TargetでExperience Cloud Debuggerを使用](../troubleshooting/troubleshoot-with-the-experience-cloud-debugger.md)
* [レスポンストークンのドキュメント](https://docs.adobe.com/help/en/target/using/administer/response-tokens.html)
* [at.jsカスタムイベントドキュメント](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/atjs-custom-events.html)
* [Adobe Target でのデータプロバイダーの使用](use-data-providers-to-integrate-third-party-data.md)