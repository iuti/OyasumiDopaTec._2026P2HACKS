# 002: Projects ボードの自動化とブランチの自動削除

- 日付：2026-11-25
- 決めた人：UMEINoriaki（API で変えられる設定はパソコン側の Claude Code が、Workflows 画面の設定は UMEINoriaki が担当）

## 決めたこと

- Projects ボード「OyasumiDopaTec. 2026P2HACKS」（https://github.com/users/noriakiumei/projects/1）の Workflows を次のように設定する。

  | Workflow | 動くとき | 設定 |
  |---|---|---|
  | Auto-add to project | 条件 `is:issue,pr is:open` に合う Issue・PR ができたとき | ボードに追加する |
  | Item added to project | ボードに追加されたとき | Status を「未着手」にする |
  | Pull request linked to issue | PR が Issue につながったとき | Status を「進行中」にする |
  | Item closed | Issue・PR が閉じたとき | Status を「完了」にする |
  | Pull request merged | PR がマージされたとき | Status を「完了」にする |
  | Auto-close issue | Status が「完了」になったとき | Issue を閉じる |
  | Auto-add sub-issues to project | サブ Issue ができたとき | ボードに追加する（初期設定のまま） |

  「Code changes requested」「Code review approved」「Item reopened」「Auto-archive items」はオフのままにする。

- リポジトリ設定で、PR をマージしたらブランチを自動で削除する（`delete_branch_on_merge: true`）。

## 理由

- 5人がそれぞれ Issue を作るので、ボードへの追加や Status の更新を手作業にすると漏れる。GitHub の操作（Issue を作る、PR をつなぐ、マージする）だけでボードが正しい状態になるようにした。
- マージ済みのブランチが残ると、どれが作業中か分かりにくくなるため自動で消す。消したブランチは PR の画面から元に戻せる。

## 検討したほかの案

- Status を手で動かす：設定はいらないが、本番中は更新が後回しになりやすい。
- GitHub Actions でボードを更新する：細かく制御できるが、組み込みの Workflows で足りるので使わない。

## 影響（変わること・注意すること）

- Workflows の設定は API で変えられないため、変えるときはボードの Workflows 画面で行う。
- Status の選択肢の名前を変えると、Workflows の行き先が外れて赤い「!」になる。名前を変えたら各 Workflow の行き先を選び直す。
- 確認結果：テスト用の Issue #4 で、作成時にボードへ追加され Status が変わり、閉じたときにもう一度 Status が変わったことを Issue の履歴で確かめた（履歴には変わった先の名前が出ないため、行き先は Workflows 画面の設定で確認した）。
