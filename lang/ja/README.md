# Anthropic クックブック

Anthropic クックブックは、Claudeを使った開発をサポートするコードとガイドを提供し、プロジェクトに簡単に統合できるコピー可能なコードスニペットを提供します。

## 前提条件

このクックブックの例を最大限に活用するには、Anthropic APIキーが必要です（[こちら](https://www.anthropic.com)から無料でサインアップできます）。

コード例は主にPythonで書かれていますが、コンセプトはAnthropic APIとのやり取りをサポートする任意のプログラミング言語に適応できます。

Anthropic APIを使った作業が初めての場合は、まず[Anthropic API基礎コース](https://github.com/anthropics/courses/tree/master/anthropic_api_fundamentals)から始めて、しっかりとした基盤を築くことをお勧めします。

## さらに詳しく

ClaudeやAIアシスタントでの体験を向上させるためのリソースをお探しですか？以下の役立つリンクをご確認ください：

- [Anthropic開発者ドキュメント](https://docs.anthropic.com/claude/docs/guide-to-anthropics-prompt-engineering-resources)
- [Anthropicサポートドキュメント](https://support.anthropic.com)
- [AnthropicDiscordコミュニティ](https://www.anthropic.com/discord)

## 貢献について

Anthropic クックブックは開発者コミュニティの貢献によって成り立っています。アイデアの提出、誤字の修正、新しいガイドの追加、既存のガイドの改善など、あなたの貢献を歓迎します。貢献することで、このリソースをすべての人にとってより価値あるものにすることができます。

努力の重複を避けるため、貢献する前に既存のイシューやプルリクエストを確認してください。

新しい例やガイドのアイデアがある場合は、[イシューページ](https://github.com/anthropics/anthropic-cookbook/issues)で共有してください。

## レシピ一覧

### スキル
- [分類](https://github.com/anthropics/anthropic-cookbook/tree/main/skills/classification): Claudeを使ったテキストとデータ分類のテクニックを探求します。
- [検索拡張生成（RAG）](https://github.com/anthropics/anthropic-cookbook/tree/main/skills/retrieval_augmented_generation): 外部知識でClaudeの応答を強化する方法を学びます。
- [要約](https://github.com/anthropics/anthropic-cookbook/tree/main/skills/summarization): Claudeを使った効果的なテキスト要約のテクニックを発見します。

### ツール使用と統合
- [ツール使用](https://github.com/anthropics/anthropic-cookbook/tree/main/tool_use): Claudeを外部ツールや機能と統合してその能力を拡張する方法を学びます。
  - [カスタマーサービスエージェント](https://github.com/anthropics/anthropic-cookbook/blob/main/tool_use/customer_service_agent.ipynb)
  - [計算機統合](https://github.com/anthropics/anthropic-cookbook/blob/main/tool_use/calculator_tool.ipynb)
  - [SQLクエリ](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/how_to_make_sql_queries.ipynb)

### サードパーティ統合
- [検索拡張生成](https://github.com/anthropics/anthropic-cookbook/tree/main/third_party): 外部データソースでClaudeの知識を補完します。
  - [ベクトルデータベース（Pinecone）](https://github.com/anthropics/anthropic-cookbook/blob/main/third_party/Pinecone/rag_using_pinecone.ipynb)
  - [Wikipedia](https://github.com/anthropics/anthropic-cookbook/blob/main/third_party/Wikipedia/wikipedia-search-cookbook.ipynb/)
  - [Webページ](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/read_web_pages_with_haiku.ipynb)
  - [インターネット検索（Brave）](https://github.com/anthropics/anthropic-cookbook/blob/main/third_party/Brave/web_search_using_brave.ipynb)
- [Voyage AIを使った埋め込み](https://github.com/anthropics/anthropic-cookbook/blob/main/third_party/VoyageAI/how_to_create_embeddings.md)

### マルチモーダル機能
- [画像分析](https://github.com/anthropics/anthropic-cookbook/tree/main/multimodal): 画像を分析し、視覚コンテンツを理解するためにClaudeを使用する方法を学びます。

### 高度なテクニック
- [サブエージェント](https://github.com/anthropics/anthropic-cookbook/blob/main/multimodal/using_sub_agents.ipynb): OpusとHaikuを組み合わせてサブエージェントとして使用する方法を学びます。
- [ClaudeにPDFをアップロード](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/pdf_upload_summarization.ipynb): PDFを解析してテキストとしてClaudeに渡します。
- [自動評価](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/building_evals.ipynb): Claudeを使ってプロンプト評価プロセスを自動化します。
- [JSONモードの有効化](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/how_to_enable_json_mode.ipynb): Claudeからの一貫したJSON出力を確保します。
- [モデレーションフィルターの作成](https://github.com/anthropics/anthropic-cookbook/blob/main/misc/building_moderation_filter.ipynb): Claudeを使ってアプリケーション用のコンテンツモデレーションフィルターを作成します。

### パターンと戦略
- [思考の連鎖](https://github.com/anthropics/anthropic-cookbook/tree/main/patterns): Claudeの推論能力を最大化するための効果的なプロンプト戦略を探求します。

## その他のリソース

- [AWS上のAnthropic](https://github.com/aws-samples/anthropic-on-aws): AWSインフラでClaudeを使用するための例とソリューションを探求します。
- [AWSサンプル](https://github.com/aws-samples/): AWSからのコードサンプル集で、Claudeで使用するために適応できます。一部のサンプルはClaudeで最適に動作するように修正が必要な場合があります。