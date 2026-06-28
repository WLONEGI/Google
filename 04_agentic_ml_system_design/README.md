# 04 Agentic & ML システムデザイン対策

**優先度：** ★★★（技術的深度を問う最重要ラウンド）
**形式：** エンドツーエンドでのAI/エージェントシステム設計

---

## このラウンドで見られること

| 評価軸 | 内容 |
|---|---|
| システム分解 | 複雑な設計を明確でテスト可能なコンポーネントに分解できるか |
| ML/エージェント知識 | RAG・モデル統合・エージェントワークフローへの精通度 |
| トレードオフ思考 | コスト・レイテンシ・信頼性・スケーラビリティのバランス |
| デプロイ意識 | 実際の顧客環境でどう動くかを考慮した設計ができるか |
| コミュニケーション | アーキテクチャの判断を論理的に説明できるか |

---

## 重点技術領域

### RAG（Retrieval-Augmented Generation）
- ベクトルDB選定（Vertex AI Vector Search、Pinecone、Weaviate）
- チャンキング戦略・埋め込みモデル選定
- ハイブリッド検索（ベクトル + BM25）
- 評価指標（RAGAS: Faithfulness / Answer Relevancy / Context Recall）

### マルチエージェントシステム
- エージェントパターン：ReAct・自己分析（Self-Reflection）・階層的委任
- フレームワーク：LangGraph・CrewAI・Google ADK
- 状態管理・メモリ設計
- MCP（Model Context Protocol）サーバー

### 本番デプロイ・オブザーバビリティ
- レイテンシ最適化（ストリーミング・バッチ・キャッシュ）
- LLMネイティブ指標（トークン/秒・リクエストあたりコスト）
- トレース・ログ・評価パイプライン（Langfuse等）
- セキュリティ境界・データ分離

### GCP / Vertex AI スタック
- Vertex AI（Gemini API・Vertex AI Search・Model Garden）
- Cloud Run / GKE（エージェントのデプロイ先）
- BigQuery・Spanner・Cloud Storage（データ層）
- Secret Manager・VPC Service Controls（セキュリティ）

---

## 典型的な設計問題と解答フレーム

### 解答の進め方（5ステップ）

```
1. 要件確認（2分）
   - ユーザー規模・レイテンシ要件・データ機密性
   - 成功指標の定義

2. 全体アーキテクチャの提示（3分）
   - コンポーネント図を描いて概観を示す

3. 各コンポーネントの詳細化（8分）
   - データフロー・モデル選定・インフラ

4. トレードオフの議論（4分）
   - なぜこの選択か、代替案との比較

5. 本番グレードの考慮事項（3分）
   - 評価パイプライン・監視・障害対応
```

---

## 練習問題

- [`design_rag_system.md`](./design_rag_system.md) — 顧客データへのRAGシステム設計
- [`design_multi_agent.md`](./design_multi_agent.md) — マルチエージェントワークフロー設計
- [`design_llm_integration.md`](./design_llm_integration.md) — 既存パイプラインへのLLM統合設計

---

## 参考リソース

- [Google ADK ドキュメント](https://google.github.io/adk-docs/)
- [Vertex AI ドキュメント](https://cloud.google.com/vertex-ai/docs)
- [LangGraph ドキュメント](https://langchain-ai.github.io/langgraph/)
- [RAGASドキュメント](https://docs.ragas.io/)
