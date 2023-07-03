---
title: ' [!DNL Analysis Workspace]  での[!UICONTROL 自動配分]アクティビティ用 A4T レポートの設定方法'
description: ' [!DNL Analysis Workspace]  での[!UICONTROL 自動配分]アクティビティの実行時に予期した結果が得られるように、A4T レポートを設定する方法について説明します。'
role: User
level: Intermediate
topic: Personalization, Integrations
feature: Analytics for Target (A4T), Auto-Target, Integrations
doc-type: tutorial
kt: null
exl-id: 7d53adce-cc05-4754-9369-9cc1763a9450
source-git-commit: bd8283d3c0e5fa9e690e377bc4dfbf6a147dd577
workflow-type: ht
source-wordcount: '1083'
ht-degree: 100%

---

# [!DNL Analysis Workspace] での [!DNL Auto-Allocate] アクティビティ用 A4T レポートの設定

[!DNL Auto-Allocate] アクティビティは、複数のエクスペリエンスの中から勝者を特定し、より多くのトラフィックをその勝者に自動的に再配分します。その間もテストによる学習は続けられます。[!UICONTROL 自動配分]のための [!UICONTROL Analytics for Target]（A4T）統合により、[!DNL Adobe Analytics] でレポートデータを確認できるほか、[!DNL Analytics] で定義されたカスタムイベントや指標を最適化することもできます。

[!DNL Adobe Analytics] [!DNL Analysis Workspace] には豊富な分析機能が備わっていますが、[最適化条件](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=ja#supported){target=_blank}の微妙な違いにより、[!DNL Auto-Allocate] アクティビティを正しく解釈するには、デフォルトの **[!UICONTROL Analytics for Target]** パネルにいくつかの変更を加える必要があります。

このチュートリアルでは、[!DNL Analysis Workspace] の [!DNL Auto-Allocate] アクティビティ分析に推奨される変更について、順番に説明します。主な概念は次のとおりです。

* [!UICONTROL 訪問者数]は、[!DNL Auto-Allocate] アクティビティの正規化指標として常に使用する必要があります。
* 指標が [!DNL Adobe Analytics] 指標である場合、コンバージョン率の適切な分子は、アクティビティの設定時に選択した最適化条件のタイプに応じて異なります。
   * 「ユニーク訪問者のコンバージョン率の最大化」の最適化条件には、指標が正の値を持つユニーク訪問者の数を分子とするコンバージョン率があります。
   * 「訪問者あたりの指標値の最大化」には、[!DNL Adobe Analytics] の通常の指標値を分子とするコンバージョン率があります。これは、[!DNL Analysis Workspace] の **[!UICONTROL Analytics for Target]** パネルにデフォルトで指定されます。
* 最適化指標が [!DNL Target] 定義済みのコンバージョン指標である場合、[!DNL Analysis Workspace] のデフォルトの **[!UICONTROL Analytics for Target]** パネルでパネルの設定を処理します。
* [!DNL Target Standard/Premium] 23.3.1 リリース（2023年3月30日（PT））より前に作成したすべての[!UICONTROL 自動配分]アクティビティでは、[!DNL Analytics Workspace] と [!DNL Target] の[!UICONTROL 信頼性]に同じ値が表示されます。

  2023年3月30日（PT）以降に作成したすべての[!UICONTROL 自動配分]アクティビティについては、これらのアクティビティに次の&#x200B;*両方*&#x200B;の条件がある場合、[!DNL Analysis Workspace] に表示される信頼区間の値には、[[!UICONTROL 自動配分]で使用されるより保守的な統計](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=ja#section_98388996F0584E15BF3A99C57EEB7629){target=_blank}が反映されません。

   * レポートソースとしての [!DNL Analytics]（A4T）
   * [!DNL Analytics] 最適化指標

  これらの条件の&#x200B;*両方*&#x200B;が存在する場合は、信頼性指標を A4T パネルから削除する必要があります。代わりに、[!DNL Target] レポートでこれらの値を参照してください。

## [!DNL Analysis Workspace] での [!DNL Auto-Allocate] パネル用の A4T の作成

[!DNL Auto-Allocate] 用の A4T レポートを作成するには、次に示すように、[!DNL Analysis Workspace] の **[!UICONTROL Analytics for Target]** パネルから開始します。次に、以下の選択を行います。

1. **[!UICONTROL コントロールエクスペリエンス]**：任意のエクスペリエンスを選択できます。
2. **[!UICONTROL 指標の正規化]**：訪問者数を選択します。[!DNL Auto-Allocate] では、ユニーク訪問者ごとのコンバージョン率が常に正規化されます。
3. **[!UICONTROL 成功指標]**：アクティビティの作成時に使用した指標と同じ指標を選択します。これが [!DNL Target] 定義済みのコンバージョン指標である場合、「**アクティビティのコンバージョン**」を選択します。それ以外の場合は、使用した [!DNL Adobe Analytics] 指標を選択します。

[!DNL Auto-Allocate] アクティビティの ![[!UICONTROL Analytics for Target] パネルの設定。](assets/AAFigure1.png)

*図 1：[!DNL Auto-Allocate] アクティビティの [!UICONTROL Analytics for Target] パネルの設定。*

>[!NOTE]
>
> [!DNL Adobe Target] のレポート画面からリンクをクリックすると、事前定義済みの **[!UICONTROL Analytics for Target]** パネルにアクセスすることもできます。

## 「訪問者あたりの指標値の最大化」の最適化条件を使用した、[!DNL Target] の[!UICONTROL コンバージョン]指標または [!DNL Analytics] 指標

デフォルトの A4T パネルでは、目標指標が [!DNL Target] コンバージョンまたは最適化条件の「訪問者あたりの指標値の最大化」を持つ [!DNL Analytics] 指標である [!DNL Auto-Allocate] アクティビティを処理します。

このパネルの例の 1 つは、[!UICONTROL 収益]指標に対して示されます。ここでは、アクティビティ作成時に最適化条件として「訪問者あたりの指標値の最大化」が選択されています。前述したように、[!DNL Auto-Allocate] では、**[!UICONTROL Analytics for Target]** パネルで使用する計算と比較して、より保守的な信頼性の計算を使用します。アドビでは、A4T パネルから信頼性指標に加え、関連する下限および上限の上昇率指標も削除することをお勧めします。代わりに、[!DNL Target] レポートでこれらの値を参照してください。

![[!UICONTROL Analytics for Target - 自動配分レポート]パネル](assets/AAFigure2.png)

*図 2：[!DNL Analytics] 指標の「訪問者あたりの指標値の最大化」の最適化条件を使用した [!DNL Auto-Allocate] アクティビティの推奨レポート。これらのタイプの指標および [!DNL Target] 定義済みのコンバージョン指標については、[!DNL Analysis Workspace] のデフォルトの&#x200B;**[!UICONTROL Analytics for Target]**パネルを使用できます。*

## 「ユニーク訪問者のコンバージョン率の最大化」の最適化条件を備えた [!DNL Analytics] 指標

[!DNL Adobe Analytics] 指標を&#x200B;*ユニーク訪問者のコンバージョン率の最大化*&#x200B;の最適化条件で使用する場合、[!DNL Analysis Workspace] のデフォルトの **[!UICONTROL Analytics for Target]** パネルを変更する必要があります。

成功指標は、コンバージョン指標が肯定的だったユニーク訪問者の数になりました。これは、指標の正の値を持つヒットをフィルタリングするセグメントを作成することで実現できます。次のように、このセグメントを作成します。

1. [!DNL Analysis Workspace] ツールバーで&#x200B;**[!UICONTROL コンポーネント]**／**[!UICONTROL セグメントを作成]**&#x200B;オプションを選択します。
1. アクティビティの作成時に使用する指標を、左側のパネルからセグメントの「**[!UICONTROL 定義]**」ボックスにドラッグします。
1. 0 の数値&#x200B;**より大きい**&#x200B;指標の値を選択します。
1. **[!UICONTROL 含める]**&#x200B;ドロップダウンから「**[!UICONTROL 訪問者数]**」を選択します。
1. セグメントに適切な名前を付けます。

次の図に、セグメント作成の例を示します。ここでは、「[!UICONTROL 収益がプラス訪問者数]」を選択しています。

[!DNL Analysis Workspace]](assets/AAFigure3.png) での![[!UICONTROL 収益がプラスの訪問者数]セグメント

*図 3：「[!UICONTROL ユニーク訪問者のコンバージョン率の最大化]」に等しい最適化条件を使用した、[!DNL Adobe Analytics] 指標のセグメント作成。この例では、指標は[!UICONTROL 収益]であり、最適化の目標は収益がプラスの訪問者数を最大化することです。*

適切なセグメントを作成したら、[!DNL Analysis Workspace] のデフォルトの **[!UICONTROL Analytics for Target]** パネルを変更できます。

1. 既存の[!UICONTROL 訪問者数]指標列の横に 2 番目の&#x200B;**ユニーク訪問者**&#x200B;指標を追加します。
2. 新しく作成したセグメントを最初の列の下にドラッグして、図 4 のようなパネルを生成します。違いがあります。収益がプラスのユニーク訪問者の数は、各エクスペリエンスに割り当てられたユニーク訪問者の合計数の一部です。

   ![Figure4.png](assets/AAFigure4.png)

   *図 4：新しく作成したセグメントによる[!UICONTROL ユニーク訪問者]のフィルタリング*

3. コンバージョン率は、1 列目と 2 列目の両方をハイライト表示して右クリックし、**[!UICONTROL 選択から指標を作成]**／**[!UICONTROL 除算]**&#x200B;を選択することで[すばやく計算](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/components/calculated-metrics/quick-calculated-metrics-in-analysis-workspace.html?lang=ja)できます。

   次の図に示すように、デフォルトのコンバージョン率を削除し、この新しい計算指標に置き換える必要があります。図のように、新しく作成した計算指標を編集して、**[!UICONTROL 形式]**／**[!UICONTROL パーセント]**&#x200B;として小数点以下 2 桁まで表示する必要がある場合があります。

   ![Figure5.png](assets/AAFigure5.png)

   *図 5：2 値化された収益コンバージョン指標のコンバージョン率を示す最後の[!UICONTROL 自動配分]パネル*

## まとめ

このチュートリアルの手順では、[!UICONTROL 自動配分]レポートデータを表示するように [!DNL Analysis Workspace] を正しく設定する方法を説明しました。

まとめると、次のようになります。

* 指標が [!DNL Target] 定義済みのコンバージョン指標である場合や、「訪問者あたりの指標値の最大化」という最適化条件を備えた [!DNL Adobe Analytics] 指標である場合は、正規化指標として訪問者数を使用して設定されたデフォルトのワークスペースパネルを使用する必要があります。
* 指標が「ユニーク訪問者のコンバージョン率の最大化」という最適化条件を備えた [!DNL Adobe Analytics] 指標である場合、指標が肯定的となる訪問者数の割合として定義されたコンバージョン率を使用する必要があります。これを行うには、[!UICONTROL ユニーク訪問者]指標をフィルタリングする、対応するセグメントを作成します。
