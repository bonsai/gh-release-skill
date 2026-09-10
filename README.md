# gh-release-skill

GitHub Release を安全に判断・実行するための Agent Skill。

## 目的

GitHub REST API を新しく作るのではなく、既存の GitHub Release API をエージェントから一貫した手順で利用する。

## 構成

- `SKILL.md` — Agent 向けの判断・実行手順
- `release-policy.yaml` — Release / 非Release の判定基準

## 基本フロー

```text
request
  ↓
Release対象か判定
  ↓
version / tag を確認
  ↓
preflight
  ↓
GitHub Release API
  ↓
Release確認
```

## Release対象

- ブラウザ拡張などの配布可能なプロダクト
- CLI / executable
- library / package
- deployable web app
- versioned dataset snapshot
- packaged template

## Releaseしないもの

- source-only commit
- docs-only change
- Issue / PR
- workflow / CI-only change
- test-only change
- internal config
- draft work

タグを作っただけでは Release とみなさない。明示的な Release 意図が必要。

## Version

標準は SemVer の `vMAJOR.MINOR.PATCH`。必要に応じて prerelease suffix を使用する。

## 実行

実際の作成処理には GitHub が提供する Release REST API を利用する。常駐する独自 API サーバーは持たない。
