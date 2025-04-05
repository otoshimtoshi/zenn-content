---
title: 'vscodeのエージェントモードを試してみよう'
emoji: '🐶'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['vscode', 'mcp', 'githubcopilot']
published: false
---

## はじめに

こんにちは、[otot_dev](https://zenn.dev/otot_dev)です。
2025 年 3 月の vscode リリースで Agent mode がリリースされましたね。

リリースノート
https://code.visualstudio.com/updates/v1_99

早速 vscode の Agent mode を試してみたいと思います。

## MCPサーバーを追加する

今回は vscode に Agent mode が追加されたので、Github の MCP サーバーを追加したいと思います。

`⌘+shift+P`で MCP:add server と出るのでそちらを選択します。
今回は npm package を選択して、`@modelcontextprotocol/server-github`と入力します。

`Install @modelcontextprotocol/server-github from ~~~?`と聞かれるので、allow。
User Settings か Workspace Settings は任意ですね。

すると settings.json に以下が追加されます。`Workspace Settings`の場合は、mcp.json に追加されると思います。

```json
  "mcp": {
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_PERSONAL_ACCESS_TOKEN": "<YOUR_TOKEN>"
        }
      }
    }
  }
```

事前に作成しておいたアクセストークンを設定してあげたら OK みたいです。

## 実際に試してみる

### リポジトリのissueのリストを見てもらう

![](/images/bdc979ce08fae4/github-server_1.png)

issue はひとつもないので正しいですね。

### issueを作成してもらう

![](/images/bdc979ce08fae4/github-server_2.png)
![](/images/bdc979ce08fae4/github-server_3.png)

権限に引っかかってしまいました。
Github の personal access token にアクセスして Issues へのアクセスを Read and Write に変更しました。

![](/images/bdc979ce08fae4/github-server_4.png)

### issueを再度作成してもらう

![](/images/bdc979ce08fae4/github-server_5.png)

できたみたいです。

![](/images/bdc979ce08fae4/github-server_6.png)

確認すると、ちゃんとリポジトリに issue が作成されていますね。

### ブランチの作成

![](/images/bdc979ce08fae4/github-server_7.png)

ブランチの作成はしてくれましたが、まだ commit して差分がないので pull request の作成まではしてくれないみたいですね。

### pull requestの作成

![](/images/bdc979ce08fae4/github-server_8.png)

今度は差分があるので pull request を作成してくれました。

![](/images/bdc979ce08fae4/github-server_9.png)

## おわりに

Copilot Chat 経由で特定の mcp サーバーを使用することで Chat だけで多くの作業を完結できることが分かりました。
issue template や pull request template が整備されているような業務ではさらに品質が高まるはずなので、今後やってみたいと思います。
