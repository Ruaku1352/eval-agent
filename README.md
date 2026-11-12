# eval-agent

実装の前に評価（eval）を設計する、Claude Code 用のサブエージェントです。

「先に成功条件を決めてから作る」を毎回の開発で行うためのもので、評価を作る役と実装する役を分けることで、自分で作ったテストを自分で甘くする問題を避けます。

## 中身

- `.claude/agents/eval-designer.md`：エージェント本体
  - **設計モード**：成功条件・テストケース・grader・実行コマンドを `evals/` に作る
  - **レビューモード**：eval の失敗結果を読み、実装の不具合か評価側の不備かを分類して評価を直す

## 使い方

1. `.claude/agents/eval-designer.md` を、使いたいリポジトリの同じパスに置く（全プロジェクトで使うなら `~/.claude/agents/`）。
2. Claude Code で「eval-designer で、〇〇機能の評価を設計して」と頼む。
3. `evals/DESIGN.md` の「要確認事項」と合格ライン案を自分で読み、決める。
4. 実装を進め、`evals/` の実行コマンドで確認する。
5. 失敗したら「eval-designer で結果をレビューして」と頼む。

## CLAUDE.md に足すと便利な記述

毎回自動でこの流れにしたい場合は、プロジェクトの `CLAUDE.md` に次を追加します。

```markdown
## 評価を先に作る
- AI/LLM を使う機能を新しく作る・変えるときは、実装の前に eval-designer エージェントで評価を設計する。
- 実装中は `evals/` の中身を編集しない。評価がおかしいと思ったら、eval-designer にレビューを頼む。
- 完了を報告する前に `evals/` の実行コマンドを動かし、結果を件数で報告する。
```

## 人間がやること

エージェントは評価の案を作りますが、次は人が決めます。

- 何が「良い」か、何が絶対にダメか
- 合格ライン
- 基準を下げる変更を認めるかどうか

## 参考にした資料

- Anthropic, [Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
- Hamel Husain, [Your AI Product Needs Evals](https://hamel.dev/blog/posts/evals/)
- OpenAI, [Evaluation best practices](https://developers.openai.com/api/docs/guides/evaluation-best-practices)
- Sergii Makarevych, [Evals: how to know whether an AI system actually works](https://x.com/sermakarevich/status/2106453816757354947)
