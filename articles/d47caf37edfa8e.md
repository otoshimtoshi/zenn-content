---
title: 'モノレポ開発手法: package.jsonのworkspacesを活用する'
emoji: '🛠️'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['モノレポ', 'JavaScript', 'package.json', 'workspaces']
published: false
---

# モノレポ開発手法: package.jsonのworkspacesを活用する

モノレポ（Monorepo）は、複数のプロジェクトを 1 つのリポジトリで管理する手法です。Node.js のエコシステムでは、`package.json`の`workspaces`機能を活用することで、モノレポを効率的に管理できます。本記事では、`workspaces`の基本的な使い方とその利点について解説します。

## workspacesとは？

`workspaces`は、Node.js のパッケージマネージャ（npm や Yarn、pnpm）で提供される機能で、複数のパッケージを 1 つのリポジトリで管理するための仕組みです。これにより、以下のような利点があります。

- **依存関係の一元管理**: 各パッケージの依存関係をリポジトリ全体で統一的に管理できます。
- **ローカル開発の効率化**: パッケージ間の依存関係をローカルでリンクすることで、開発効率が向上します。
- **ビルドやテストの一括実行**: 全パッケージに対して一括でスクリプトを実行できます。

## 基本的な設定方法

以下は、`workspaces`を設定するための基本的な手順です。

1. **`package.json`の作成**:
   ルートディレクトリに`package.json`を作成し、以下のように`workspaces`フィールドを追加します。

   ```json
   {
     "name": "my-monorepo",
     "private": true,
     "workspaces": ["packages/*"]
   }
   ```

2. **パッケージの配置**:
   `packages`ディレクトリを作成し、その中に個別のパッケージを配置します。

   ```
   packages/
   ├── package-a/
   │   └── package.json
   └── package-b/
       └── package.json
   ```

3. **パッケージマネージャのインストール**:
   `pnpm`や`Yarn`を使用して依存関係をインストールします。

   ```bash
   pnpm install
   ```

## 実践例

例えば、`package-a`が`package-b`に依存している場合、`package-a`の`package.json`に以下を記述します。

```json
{
  "dependencies": {
    "package-b": "workspace:*"
  }
}
```

これにより、ローカルの`package-b`がリンクされます。

## まとめ

`workspaces`を活用することで、モノレポの管理が大幅に効率化されます。特に、複数のパッケージ間で依存関係がある場合や、統一的なビルド・テスト環境を構築したい場合に有用です。ぜひ、プロジェクトで試してみてください。
