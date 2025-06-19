# Promptfooによる評価

### この評価スイートについての注意

1) 以下の指示、特に必要なパッケージに関する前提条件に従ってください。

2) 完全な評価スイートの実行には通常より高いレート制限が必要な場合があります。promptfooでテストのサブセットのみを実行することを検討してください。

3) すべてのテストが初期状態で合格するわけではありません - 我々は評価を適度に挑戦的になるよう設計しました。

### 前提条件 
Promptfooを使用するには、システムにnode.jsとnpmがインストールされている必要があります。詳細については[このガイド](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)を参照してください。

npmを使用してpromptfooをインストールするか、npxを使用して直接実行できます。このガイドではnpxを使用します。

*注意: この例では`npx promptfoo@latest init`を実行する必要はありません。このディレクトリには既に初期化された`promptfooconfig.yaml`ファイルがあります*

公式ドキュメントは[こちら](https://www.promptfoo.dev/docs/getting-started)を参照してください

#### 注意 - 追加の依存関係
この例では、custom_evalsが適切に動作するために以下の依存関係をインストールする必要があります。

`pip install nltk rouge-score`

### 開始方法

開始するには、ANTHROPIC_API_KEY環境変数、または選択したプロバイダーに必要な他のキーを設定してください。`export ANTHROPIC_API_KEY=YOUR_API_KEY`を実行できます。

次に、`evaluation`ディレクトリに`cd`し、`npx promptfoo@latest eval -c promptfooconfig.yaml --output ../data/results.csv`と記述してください。

その後、`npx promptfoo@latest view`を実行して結果を表示できます。

### 動作方法

promptfooconfig.yamlファイルは評価設定の中心です。いくつかの重要なセクションを定義します：

プロンプト:
- プロンプトはprompts.pyファイルからインポートされます。
- これらのプロンプトはLMパフォーマンスの様々な側面をテストするよう設計されています。

プロバイダー:
- ここで異なるClaudeバージョンとその設定を構成します。
- これにより、複数のモデルでテストしたり、異なるパラメータ（例：異なる温度設定）でテストできます。

テスト:
- テストケースはこのファイルで定義されるか、この場合はtests.yamlからインポートされます。
- これらのテストは評価のための入力と期待される出力を指定します。
- Promptfooは様々な組み込みテストタイプ（ドキュメント参照）を提供するか、独自に定義できます。我々は3つのカスタム評価と1つの初期状態（contains method）があります：
    - `bleu_eval.py`: BLEU（Bilingual Evaluation Understudy）スコアを実装し、機械生成テキストと参照テキストの類似性を測定します。
    - `rouge_eval.py`: ROUGE（Recall-Oriented Understudy for Gisting Evaluation）スコアを実装し、参照要約と比較して要約の品質を評価します。
    - `llm_eval.py`: 生成されたテキストの一貫性、関連性、事実の正確性など様々な側面を評価するために言語モデルを活用するカスタム評価メトリクスが含まれています。

出力:
- 評価結果の形式と場所を指定します。
- Promptfooは様々な出力形式もサポートしています！

### Pythonバイナリの上書き

デフォルトでは、promptfooはシェルでpythonを実行します。pythonが適切な実行可能ファイルを指していることを確認してください。

pythonバイナリが存在しない場合、「python: command not found」エラーが表示されます。

Pythonバイナリを上書きするには、PROMPTFOO_PYTHON環境変数を設定します。パス（/path/to/python3.11など）またはPATH内の実行可能ファイル（python3.11など）に設定できます。