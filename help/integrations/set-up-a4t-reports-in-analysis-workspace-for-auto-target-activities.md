---
title: Analysis Workspace for Auto-Target アクティビティで A4T レポートを設定する方法
description: Analytics for Target(A4T) 統合が完了し、自動ターゲットアクティビティを実行したら、結果を正しく解釈するにはどうすればよいですか？ 次の手順に従って、自動ターゲットアクティビティの実行時に予期した結果を得るためにAnalysis Workspaceで A4T レポートを設定します。
role: User
level: Intermediate
topic: Personalization, Integrations
feature: Analytics for Target (A4T), Auto-Target, Integrations
doc-type: tutorial
kt: null
exl-id: 58006a25-851e-43c8-b103-f143f72ee58d
source-git-commit: 2571964b557f696d8e0377b922d96e90611f2327
workflow-type: tm+mt
source-wordcount: '2639'
ht-degree: 1%

---

# での A4T レポートの設定 [!DNL Analysis Workspace] 対象 [!DNL Auto-Target] アクティビティ

>[!IMPORTANT]
>
>の場合 [!UICONTROL 自動ターゲット] アクティビティの場合は、 [!DNL Analytics Workspace] A4T パネルを手動で作成します。

この [!UICONTROL Analytics for Target] (A4T) 統合： [!DNL Auto-Target] アクティビティ使用 [!DNL Adobe Target]のアンサンブル機械学習 (ML) アルゴリズムを使用します。 [!DNL Adobe Analytics] 目標指標。

豊富な分析機能は [!DNL Adobe Analytics] [!DNL Analysis Workspace]( デフォルトの **[!UICONTROL Analytics for Target]** 正しく解釈するには、パネルが必要です [!DNL Auto-Target] アクティビティ ( 実験アクティビティ（手動の A/B および自動配分）とパーソナライゼーションアクティビティ ([!UICONTROL 自動ターゲット]) をクリックします。

このチュートリアルでは、分析に推奨される変更について説明します [!UICONTROL 自動ターゲット] アクティビティ [!DNL Workspace]は、次の主要概念に基づいています。

* この **[!UICONTROL コントロールとターゲット]** ディメンションを使用して、コントロールエクスペリエンスと、 [!UICONTROL 自動ターゲット] アンサンブル ML アルゴリズム。
* 訪問回数は、パフォーマンスのエクスペリエンスレベルの分類を表示する際に、標準化指標として使用する必要があります。 さらに、 [Adobe Analyticsのデフォルトのカウント手法には、ユーザーが実際にアクティビティのコンテンツを見ない訪問が含まれる場合があります](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-viewing-reports.html?lang=en#metrics)に設定されていますが、適切にスコープされたセグメント（以下で詳しく説明）を使用して、このデフォルトの動作を変更できます。
* 訪問ルックバックスコープアトリビューション（所定のアトリビューションモデルの「訪問ルックバックウィンドウ」とも呼ばれます）は、 [!DNL Adobe Target]の ML モデルは、トレーニングフェーズで使用され、目標指標を分類する際には、同じ（デフォルト以外の）アトリビューションモデルを使用する必要があります。

## 用の A4T の作成 [!UICONTROL 自動ターゲット] パネル内 [!DNL Workspace]

用の A4T を作成するには、以下を実行します。 [!UICONTROL 自動ターゲット] レポートは、 **[!UICONTROL Analytics for Target]** パネル内 [!DNL Workspace]、以下に示すように、またはフリーフォームテーブルで始まります。 次に、以下の選択を行います。

1. **[!UICONTROL コントロールエクスペリエンス]**:経験はどれでも選べる。ただし、この選択は後で上書きします。 なお、 [!UICONTROL 自動ターゲット] アクティビティ、コントロールエクスペリエンスは、実際にはコントロール戦略です。a) すべてのエクスペリエンスでランダムに提供する、b) 単一のエクスペリエンスを提供する ( この選択は、 [!DNL Adobe Target]) をクリックします。 (b) 選択を選択したとしても、 [!UICONTROL 自動ターゲット] アクティビティは、特定のエクスペリエンスをコントロールとして指定しました。A4T の分析に関してこのチュートリアルで説明されているアプローチに従う必要があります。 [!UICONTROL 自動ターゲット] アクティビティ。
2. **[!UICONTROL 指標の標準化]**:訪問回数を選択します。
3. **[!UICONTROL 成功指標]**:レポート対象の指標を選択することもできますが、通常、でアクティビティを作成する際に最適化用に選択したのと同じ指標に関するレポートを表示する必要があります。 [!DNL Target].

![図 1.png](assets/Figure1.png)
*図 1: [!UICONTROL Analytics for Target] パネル設定 [!UICONTROL 自動ターゲット] アクティビティ。*

>[!NOTE]
>
>次の手順で [!UICONTROL Analytics for Target] パネル [!UICONTROL 自動ターゲット] アクティビティ、任意のコントロールエクスペリエンスを選択、 [!UICONTROL 訪問回数] 標準化指標として追加し、最適化用に選択したのと同じ目標指標を [!DNL Target] アクティビティの作成。

## 以下を使用： [!UICONTROL コントロールとターゲット] 比較するディメンション [!DNL Target] コントロールに合わせたアンサンブル ML モデル

デフォルトの A4T パネルは、従来の（手動の）A/B テストまたは [!UICONTROL 自動配分] 個々のエクスペリエンスのパフォーマンスをコントロールエクスペリエンスと比較することが目標となるアクティビティ。 In [!UICONTROL 自動ターゲット] ただし、最初の順序の比較は、コントロール *戦略* そしてターゲット *戦略* ( つまり、 [!UICONTROL 自動ターゲット] 制御方式のアンサンブル ML モデル )。

この比較を実行するには、 **[!UICONTROL コントロールとターゲット (Analytics for Target)]** ディメンション。 ドラッグ&amp;ドロップして **[!UICONTROL Target エクスペリエンス]** ディメンションを使用して、A4T デフォルトのレポートに表示できます。

この置き換えにより、A4T パネルのデフォルトの上昇率および信頼性の計算が無効になります。 混乱を避けるために、これらの指標をデフォルトのパネルから削除し、次のレポートを残しておくことができます。

![図 2.png](assets/Figure2.png)

*図 2:に対して推奨されるベースラインレポート [!DNL Auto-Target] アクティビティ。 このレポートは、（アンサンブル ML モデルが提供する）ターゲットトラフィックとコントロールトラフィックを比較するように設定されています。*

>[!NOTE]
>
>現在、上昇率および信頼性の数値は、 [!UICONTROL コントロールとターゲット] 次元 for A4T レポート： [!UICONTROL 自動ターゲット]. サポートが追加されるまでは、上昇率と信頼性は、 [信頼性計算器](https://experienceleague.adobe.com/docs/target/assets/complete_confidence_calculator.xlsx?lang=en).

## エクスペリエンスレベルの指標分類の追加

アンサンブル ML モデルのパフォーマンスをさらに深く理解するには、 **[!UICONTROL コントロールとターゲット]** ディメンション。 In [!DNL Workspace]、 **[!UICONTROL Target エクスペリエンス]** ディメンションをレポートに表示し、各コントロールディメンションとターゲットディメンションを個別に分類します。

![図 3.png](assets/Figure3.png)
*図 3:ターゲットエクスペリエンスによるターゲットディメンションの分類*

結果のレポートの例を次に示します。

![図 4.png](assets/Figure4.png)
*図 4:標準 [!UICONTROL 自動ターゲット] エクスペリエンスレベルの分類でレポートを作成する。 目標指標が異なる場合があり、コントロール戦略が単一のエクスペリエンスを持つ可能性があることに注意してください。*

>[!TIP]
>
>In [!DNL Workspace]で、歯車アイコンをクリックして、 [!UICONTROL コンバージョン率] 列を使用して、エクスペリエンスのコンバージョン率に焦点を当てるのに役立ちます。 コンバージョン率は小数形式で表示され、それに応じて割合として解釈されます。

## 「訪問回数」がに対して正しい標準化指標であるのはなぜですか？ [!UICONTROL 自動ターゲット] アクティビティ

分析の際に [!UICONTROL 自動ターゲット] アクティビティ、常に選択 [!UICONTROL 訪問回数] をデフォルトの標準化指標として使用します。 [!UICONTROL 自動ターゲット] パーソナライゼーションは、訪問ごとに 1 回、訪問者のエクスペリエンスを選択します ( 正式には、 [!DNL Adobe Target] セッション ) とは、ユーザーに表示されるエクスペリエンスは、1 回の訪問ごとに変更できることを意味します。 したがって、 [!UICONTROL 実訪問者数] 標準化指標の場合、1 人のユーザーに複数のエクスペリエンスが（異なる訪問で）表示される可能性があるので、コンバージョン率を混乱させます。

この点を簡単な例で示します。2 つのエクスペリエンスのみを持つキャンペーンに 2 人の訪問者が入るシナリオを考えてみましょう。 最初の訪問者が 2 回訪問します。 これらは最初の訪問でエクスペリエンス A に割り当てられますが、2 回目の訪問でエクスペリエンス B に割り当てられます（2 回目の訪問でのプロファイルの状態の変化が原因）。 2 回目の訪問の後、訪問者は注文をしてコンバージョンします。 コンバージョンは、最も最近表示されたエクスペリエンス（エクスペリエンス B）に関連付けられます。 2 番目の訪問者も 2 回訪問し、エクスペリエンス B が両方とも表示されますが、コンバージョンには至りません。

次に、訪問者レベルのレポートと訪問レベルのレポートを比較します。

| エクスペリエンス | ユニーク訪問者 | 訪問 | コンバージョン数 | 訪問者標準。 変換 レート | 訪問の標準。 変換 レート |
| --- | --- | --- | --- | --- | --- |
| A | 1 | 1 | - | 0% | 0% |
| B | 2 | 3 | 1 | 50% | 33.3% |
| 合計 | 2 | 4 | 1 | 50% | 25％ |

*表 1:例えば、ある訪問に対して決定が定着している（通常の A/B テストとは異なり、訪問者ではない）シナリオについて、訪問者が正規化されたレポートと訪問が正規化されたレポートを比較します。 訪問者が正規化した指標は、このシナリオでは紛らわしいものです。*

表に示すように、訪問者レベルの数値は明らかに不一致です。 合計 2 人の個別訪問者があるにもかかわらず、これは各エクスペリエンスの個別訪問者の合計ではありません。 訪問者レベルのコンバージョン率は必ずしも間違っているとは限りませんが、個々のエクスペリエンスを比較すると、訪問レベルのコンバージョン率は非常に合理的です。 正式な分析単位（「訪問回数」）は、判定の定着度の単位と同じです。つまり、指標のエクスペリエンスレベルの分類を追加して比較できます。

## アクティビティへの実際の訪問数のフィルター

この [!DNL Adobe Analytics] 訪問者のデフォルトのカウント手法 [!DNL Target] アクティビティには、ユーザーが [!DNL Target] アクティビティ。 これは道のりのせいだ [!DNL Target] アクティビティの割り当ては、 [!DNL Analytics] 訪問者コンテキスト。 その結果、 [!DNL Target] アクティビティが水増しされ、コンバージョン率が低下する場合があります。

ユーザーが実際に [!UICONTROL 自動ターゲット] アクティビティ（アクティビティ、表示/訪問イベントまたはコンバージョンのエントリによって）は、次の操作を実行できます。

1. からのヒットを含む特定のセグメントを作成します。 [!DNL Target] 問題となる活動
1. フィルター [!UICONTROL 訪問回数] 指標を使用しています。

**セグメントを作成するには：**

1. を選択します。 **[!UICONTROL コンポーネント/セグメントを作成]** オプション [!DNL Workspace] ツールバー。
2. を入力します。 **[!UICONTROL タイトル]** を設定します。 以下の例では、セグメントの名前はです。 [!DNL "Hit with specific Auto-Target activity"].
3. 次をドラッグ： **[!UICONTROL Target アクティビティ]** セグメントのディメンション **[!UICONTROL 定義]** 」セクションに入力します。
4. 以下を使用： **[!UICONTROL 次と等しい]** 演算子
5. 特定の [!DNL Target] アクティビティ。
6. 歯車アイコンを選択し、「 **[!UICONTROL アトリビューションモデル/インスタンス]** 次の図に示すように。
7. 「**[!UICONTROL 保存]**」をクリックします。

![図 5.png](assets/Figure5.png)
*図 5:ここに示すセグメントなどのセグメントを使用して、 [!UICONTROL 訪問回数] A4T の指標 [!UICONTROL 自動ターゲット] レポート*

セグメントを作成したら、それを使用して [!UICONTROL 訪問回数] 指標、 [!UICONTROL 訪問回数] 指標には、ユーザーが [!DNL Target] アクティビティ。

**フィルターするには [!UICONTROL 訪問回数] 次のセグメントを使用：**

1. 新しく作成したセグメントをコンポーネントツールバーからドラッグし、 **[!UICONTROL 訪問回数]** 青になるまでの指標ラベル **[!UICONTROL フィルター条件]** プロンプトが表示されます。
2. セグメントを解放します。 その指標にフィルターが適用されます。

最後のパネルは、次のように表示されます。

![図 6.png](assets/Figure6.png)
*図 6:「特定の自動ターゲットアクティビティによるヒット」セグメントが [!UICONTROL 訪問回数] 指標。 これにより、ユーザーが実際に [!DNL Target] 該当するアクティビティがレポートに含まれます。*

## 目標指標とアトリビューションが最適化基準と一致していることを確認します。

A4T 統合により、 [!UICONTROL 自動ターゲット] ML モデルは次のようになります *訓練された* と同じコンバージョンイベントデータを使用する [!DNL Adobe Analytics] 使用 *パフォーマンスレポートを生成*. ただし、ML モデルをトレーニングする際に、このデータを解釈する際に使用する必要がある前提条件はいくつかあります。これは、のレポート段階で行われたデフォルトの前提とは異なります。 [!DNL Adobe Analytics].

特に、 [!DNL Adobe Target] ML モデルは、訪問スコープのアトリビューションモデルを使用します。 つまり、コンバージョンが ML モデルによる決定に「帰属」されるためには、コンバージョンがアクティビティのコンテンツの表示と同じ訪問で発生する必要があると想定しています。 これは、 [!DNL Target] モデルの適時の訓練を保証する [!DNL Target] は、コンバージョンまで最大 30 日間待つことはできません ( [!DNL Adobe Analytics]) を含めてから、そのモデルのトレーニングデータに含めます。

したがって、 [!DNL Target] モデル（トレーニング中）とデータのクエリに使用されるデフォルトアトリビューション（レポートの生成中）では、不一致が生じる可能性があります。 実際にはアトリビューションに問題がある場合、ML モデルのパフォーマンスが低いように見えることもあります。

>[!TIP]
>
>レポートに表示している指標とは異なる属性の指標に対して ML モデルが最適化を行っている場合、そのモデルが期待どおりに動作しない可能性があります。 これを避けるには、レポートの目標指標で、Target の ML モデルで使用されるのと同じ指標定義とアトリビューションを使用するようにします。

正確な指標の定義とアトリビューションの設定は、 [最適化基準](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=en#supported) アクティビティの作成中に指定した値。

### 定義されたコンバージョンまたは Analytics 指標を *訪問あたりの指標値の最大化*

指標が Target コンバージョンの場合、または Analytics 指標が **訪問あたりの指標値の最大化**の目標指標を定義すると、同じ訪問で複数のコンバージョンイベントを発生させることができます。
Adobe Targetの ML モデルで使用されているのと同じアトリビューション手法を持つ目標指標を表示するには、次の手順に従います。

![gearicon.png](assets/gearicon.png)

1. 表示されたメニューから、にスクロールします。 **[!UICONTROL データ設定]**.
1. 選択 **[!UICONTROL デフォルト以外のアトリビューションモデルを使用]** （まだ選択されていない場合）:

![non-defaultattributionmodel.png](assets/non-defaultattributionmodel.png)

1. 「**[!UICONTROL 編集]**」をクリックします。
1. 選択 **[!UICONTROL モデル]**: **[!UICONTROL パーティシペーション]**、および **[!UICONTROL ルックバックウィンドウ]**: **[!UICONTROL 訪問]**.

![ParticipationbyVisit.png](assets/ParticipationbyVisit.png)

1. 「**[!UICONTROL 適用]**」をクリックします。

これらの手順は、目標指標イベントが発生した場合に、レポートが目標指標をエクスペリエンスの表示に関連付けるようにします *いつでも* （「パーティシペーション」）と同じ訪問での使用が可能です。

### Analytics 指標と *個別訪問コンバージョン率*

**ポジティブな指標セグメントで訪問を定義する**

選択したシナリオで *個別訪問コンバージョン率の最大化* 最適化の基準として、コンバージョン率の正しい定義は、指標値が正の訪問の割合です。 これは、指標の正の値を持つ訪問に対するフィルタリングを作成し、訪問指標をフィルタリングすることで達成できます。


1. 以前と同様に、 **[!UICONTROL コンポーネント/セグメントを作成]** 」オプションを使用します。
2. を入力します。 **[!UICONTROL タイトル]** を設定します。 以下の例では、セグメントの名前はです。 [!DNL "Visits with an order"].
3. 最適化目標で使用したベース指標をセグメントにドラッグします。 次の例では、 **注文件数** 指標を使用して、注文が記録された訪問の割合をコンバージョン率で測定することができます。
4. セグメント定義コンテナの左上で、「 」を選択します。 **[!UICONTROL 次を含む]** **訪問**.
5. 以下を使用： **[!UICONTROL 次よりも大きい]** 演算子を使用し、値を 0 に設定します（つまり、このセグメントには、注文指標が正の数の訪問が含まれます）。
6. 「**[!UICONTROL 保存]**」をクリックします。

![図 7.png](assets/Figure7.png)
*図 7:正の順序の訪問に対するフィルタリングを行うセグメント定義。 アクティビティの最適化指標に応じて、注文を適切な指標に置き換える必要があります*

**フィルターされたアクティビティの指標の訪問に適用**

このセグメントを使用して、正の数の注文がある訪問と、 [!DNL Auto-Target]アクティビティ。 指標をフィルタリングする手順は、前と同様です。既にフィルタリングされている訪問指標に新しいセグメントを適用した後、レポートパネルは図 8 のようになります

![図 8.png](assets/Figure8.png)
*図 8:正しい個別訪問コンバージョン指標を含むレポートパネル。つまり、アクティビティからのヒットが記録された訪問回数、およびコンバージョン指標（この例では注文件数）がゼロ以外であった場合です。*


## 最終手順：上記のマジックを捉えたコンバージョン率を作成する

を [!UICONTROL 訪問] および前の節の目標指標。 [!UICONTROL 自動ターゲット] レポートパネルでは、適切にフィルタリングされた、適切なアトリビューションを持つ目標指標に対する正しい比率であるコンバージョン率を作成します [!UICONTROL 訪問回数] 指標。

それには、次の手順で計算指標を作成します。

1. を選択します。 **[!UICONTROL コンポーネント/指標を作成]** オプション [!DNL Workspace] ツールバー。
1. を入力します。 **[!UICONTROL タイトル]** を選択します。 例えば、「アクティビティ XXX の訪問修正コンバージョン率」などです。
1. 選択 **[!UICONTROL 形式]** =パーセントおよび **[!UICONTROL 小数点以下の桁数]** = 2.
1. アクティビティに関連する目標指標（例： ）をドラッグします。 [!UICONTROL アクティビティコンバージョン]) を定義に追加し、前述のように、この目標指標の歯車アイコンを使用して、アトリビューションモデルを（パーティシペーション|訪問）に調整します。
1. 選択 **[!UICONTROL 追加/コンテナ]** の右上から **[!UICONTROL 定義]** 」セクションに入力します。
1. 2 つのコンテナ間の除算 (÷) 演算子を選択します。
1. 以前に作成したセグメント (「特定の [!UICONTROL 自動ターゲット] このチュートリアルのアクティビティ」( この特定の [!DNL Auto-Target] アクティビティ。
1. 次をドラッグ： **[!UICONTROL 訪問回数]** 指標をセグメントコンテナに追加します。
1. 「**[!UICONTROL 保存]**」をクリックします。

>[!TIP]
>
> この指標は、 [クイック計算指標機能](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/components/calculated-metrics/quick-calculated-metrics-in-analysis-workspace.html?lang=en).

完全な計算指標の定義がここに表示されます。

![図 9.png](assets/Figure9.png)
*図 9:訪問および属性が修正されたモデルコンバージョン率指標の定義。 ( この指標は、目標指標とアクティビティに依存します。 つまり、この指標定義は、アクティビティをまたいで再利用することはできません )。*

>[!IMPORTANT]
>
>A4T パネルのコンバージョン率指標は、テーブルのコンバージョンイベントや標準化指標にリンクされていません。 このチュートリアルで提案した変更を加えた場合、コンバージョン率は変更に自動的に適応しません。 したがって、コンバージョンイベント属性と標準化指標の一方（または両方）に変更を加える場合は、前述のように、コンバージョン率も変更する最後の手順として覚えておく必要があります。

## 概要：最終サンプル [!DNL Workspace] パネル [!UICONTROL 自動ターゲット] レポート

上記のすべての手順を 1 つのパネルに組み合わせた場合、次の図は、 [!UICONTROL 自動ターゲット] A4T アクティビティ。 このレポートは、 [!DNL Target] 目標指標を最適化する ML モデル。このチュートリアルで説明するすべてのニュアンスと推奨事項が組み込まれています。 また、このレポートは、従来の [!DNL Target]レポート主導 [!UICONTROL 自動ターゲット] アクティビティ。

![図 10.png](assets/Figure10.png)
*図 10:最終的な A4T [!UICONTROL 自動ターゲット] レポート [!DNL Adobe Analytics] [!DNL Workspace]は、このドキュメントの前の節で説明した指標の定義に対するすべての調整を組み合わせたものです。*
