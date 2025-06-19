# 効果的なエージェント構築クックブック

Erik SchluntzとBarry Zhangによる[Building Effective Agents](https://anthropic.com/research/building-effective-agents)の参考実装です。

このリポジトリには、ブログで議論された一般的なエージェントワークフローの最小実装例が含まれています：

- 基本構成要素
  - プロンプトチェーン
  - ルーティング
  - マルチLLM並列化
- 高度なワークフロー
  - オーケストレーター-サブエージェント
  - 評価者-最適化者

## 開始方法
詳細な例についてはJupyterノートブックを参照してください：

- [基本ワークフロー](basic_workflows.ipynb)
- [評価者-最適化者ワークフロー](evaluator_optimizer.ipynb) 
- [オーケストレーター-ワーカーワークフロー](orchestrator_workers.ipynb)