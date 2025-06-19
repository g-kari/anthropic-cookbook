# Promptfooによる評価

### 前提条件 
Promptfooを使用するには、システムにnode.jsとnpmがインストールされている必要があります。詳細については[このガイド](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)を参照してください。

npmを使用してpromptfooをインストールするか、npxを使用して直接実行できます。このガイドではnpxを使用します。

*注意: この例では`npx promptfoo@latest init`を実行する必要はありません。このディレクトリには既に初期化された`promptfooconfig.yaml`ファイルがあります*

公式ドキュメントは[こちら](https://www.promptfoo.dev/docs/getting-started)を参照してください

### 開始方法
評価は`promptfooconfig...` `.yaml`ファイルによって制御されます。我々のアプリケーションでは、検索システムを評価するための`promptfooconfig_retrieval.yaml`と、エンドツーエンドのパフォーマンスを評価するための`promptfooconfig_end_to_end.yaml`に評価ロジックを分割しています。これらの各ファイルで以下のセクションを定義します

### 検索評価

- プロンプト
    - Promptfooは様々な形式でプロンプトをインポートできます。詳細は[こちら](https://www.promptfoo.dev/docs/configuration/parameters)をお読みください。
    - 我々の場合、毎回新しいプロンプトを提供することをスキップし、評価のために各検索「プロバイダー」に単に`{{query}}`を渡すだけです
- プロバイダー
    - 標準のLLMプロバイダーを使用する代わりに、`guide.ipynb`で見つかった各検索方法のためのカスタムプロバイダーを書きました
- テスト
    - `guide.ipynb`で使用されたのと同じデータを使用します。これを`end_to_end_dataset.csv`と`retrieval_dataset.csv`に分割し、各データセットに`__expected`列を追加しました。これにより各行に対して自動的にアサーションを実行できます
    - 検索評価ロジックは`eval_end_to_end.py`で確認できます

### エンドツーエンド評価

- プロンプト
    - Promptfooは様々な形式でプロンプトをインポートできます。詳細は[こちら](https://www.promptfoo.dev/docs/configuration/parameters)をお読みください。
    - エンドツーエンド評価設定では3つのプロンプトがあります：それぞれは使用方法に対応します
        - これらの関数は`guide.ipynb`で使用されたものと同じですが、Anthropic APIを呼び出す代わりにプロンプトを返すだけです。PromptfooがAPIの呼び出しと結果の保存を管理します。
        - プロンプト関数の詳細は[こちら](https://www.promptfoo.dev/docs/configuration/parameters#prompt-functions)をお読みください。Pythonを使用することで、RAGに必要なVectorDBクラスを再利用できます。これは`vectordb.py`で定義されています。
- プロバイダー
    - Promptfooでは様々なプラットフォームの多数のLLMに接続できます。詳細は[こちら](https://www.promptfoo.dev/docs/providers)を参照してください。`guide.ipynb`では温度設定0.0のデフォルトでHaikuを使用しました。Promptfooを使用して異なるモデルを実験します。
- テスト
    - `guide.ipynb`で使用されたのと同じデータを使用します。これを`end_to_end_dataset.csv`と`retrieval_dataset.csv`に分割し、各データセットに`__expected`列を追加しました。これにより各行に対して自動的にアサーションを実行できます
    - Promptfooには[こちら](https://www.promptfoo.dev/docs/configuration/expected-outputs/deterministic)で確認できる豊富な組み込みテストがあります。
    - 検索システムのテストロジックは`eval_retrieval.py`で、エンドツーエンドシステムのテストロジックは`eval_end_to_end.py`で確認できます
- 出力
    - 出力ファイルのパスを定義します。Promptfooは多くの形式で結果を出力できます。[こちら](https://www.promptfoo.dev/docs/configuration/parameters/#output-file)を参照してください。または、PromptfooのWeb UIを使用することもできます。[こちら](https://www.promptfoo.dev/docs/usage/web-ui)を参照してください。

### 評価の実行

Promptfooを開始するには、ターミナルを開いてこのディレクトリ（`./evaluation`）に移動してください。

評価を実行する前に、以下の環境変数を定義する必要があります：

`export ANTHROPIC_API_KEY=YOUR_API_KEY`  
`export VOYAGE_API_KEY=YOUR_API_KEY`

`evaluation`ディレクトリから以下のコマンドのいずれかを実行してください。

- エンドツーエンドシステムのパフォーマンスを評価するには：`npx promptfoo@latest eval -c promptfooconfig_end_to_end.yaml --output ../data/end_to_end_results.json`

- 検索システムのパフォーマンスを単独で評価するには：`npx promptfoo@latest eval -c promptfooconfig_retrieval.yaml --output ../data/retrieval_results.json`

評価が完了すると、ターミナルはデータセットの各行の結果を出力します。`npx promptfoo@latest view`を実行してpromptfoo UIビューアーで出力を表示することもできます。