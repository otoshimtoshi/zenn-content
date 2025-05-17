---
title: 'packeage.jsonでのworkspaces活用法'
emoji: '🛠️'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['nodejs', 'npm', 'yarn', 'pnpm', 'monorepo']
published: false
---

## はじめに

[otot_dev](https://zenn.dev/otot_dev)です。

`package.json`の`workspaces`機能を活用することで、モノレポ（Monorepo）の管理を効率化できることを知りました。
2022 年 10 月頃にはできるようになっていたようですね。
本記事では、`workspaces`について書いていきたいと思います。



## workspacesとは

`workspaces`とは、Node.js のパッケージマネージャ（npm や yarn、pnpm）が提供している機能の 1 つです。
ルートのパッケージから他のディレクトリの複数のパッケージを管理できます。
要は複数のパッケージを 1 つのリポジトリで管理するための仕組みですね。

あくまでも Node.js のパッケージマネージャを使用している場合の話になりますが、モノレポによる開発であったり、ローカルのモックサーバーを別のパッケージとして管理したり、はたまたテストだけ別のパッケージとして切り出してみたりと色々な使用方法ができそうです。

公式ドキュメントは以下を参照してみてください。

https://docs.npmjs.com/cli/v8/using-npm/workspaces

## 使用方法

以下の`package.json`がルートディレクトリにある状態です。

```json
{
  "name": "my-workspaces",
  "workspaces": ["packages/a"]
}
```

今度は`packages/a`配下にも別のパッケージマネージャーを作成します。

```diff
.
│── package.json
+└── packages
+    └── a
+        └── package.json
```

ルートディレクトリからパッケージのインストールをします。

```sh
npm install
# または
yarn install
# または
pnpm install
```

これだけで、ルートディレクトリ、`packages/a`配下のパッケージがインストールされます。

```diff
.
│── package.json
+│── node_modules
└── packages
    └── a
+        │── node_modules
        └── package.json
```

※pnpm の場合は`pnpm-workspace.yaml`をルート直下に作成する必要があるようです。

```yaml
packages:
  - 'packages/a'
```

## まとめ

`workspaces`は、Node.js を使用する場合に限られはしますが、モノレポ開発やローカル開発を効率化するための強力なツールです。特に、複数のパッケージが 1 つのリポジトリで管理されている場合やパッケージ間で依存関係がある場合などに有用そうです。
