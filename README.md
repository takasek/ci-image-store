# ci-image-store

CI が自動生成する PR スナップショット画像の保管庫。**手動編集禁止。**

- ブランチは PR ごとに `<source-repo>/pr-<番号>`（例: `pfdsl/pr-1234`）。
- 各ブランチに `base/` `pr/` `diffs/` の PNG を置き、PR コメントから raw URL で参照する。
- PR の close/merge で自動削除される。
- CI が直接 push・削除するため、人手で触らないこと。

## なぜ専用 repo なのか

スナップショット画像をソース repo 本体にコミットすると、生 PNG の blob が本体の
object DB に同居し、`git clone` / `fetch` / pack を肥大させる。画像を本 repo に
分離することで、本体 repo の clone を軽量に保ちつつ PR コメントへの画像埋め込み
（raw URL）を維持する。

このため本 repo は **誰も clone しない前提** で、画像専用ストレージとして肥大しても
本体の開発フローに影響しない。

## 仕組み・実例

pfdsl の pr diff images workflow が生成・更新する:
https://github.com/takasek/pfdsl/blob/main/.github/workflows/pr-diff-images.yml
