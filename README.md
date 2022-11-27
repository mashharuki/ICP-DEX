# ICP-DEX
ICPと連動するDEX開発用のリポジトリです。

### スマートコントラクト開発言語 Motoko

Motokoは、DFINITY財団が開発中の新しいソフトウェア言語で、インターネット・コンピュータ・ブロックチェーン上で信頼性と保守性の高いウェブサイト、企業システム、インターネットシステムを、できるだけ幅広い層の開発者が簡単に作成できるように設計されています。

### dfxのテンプレプロジェクト作成コマンド

```zsh
dfx new icp__dex
```


上手く行くと次のように出力される。

```zsh
Creating git repository...

===============================================================================
        Welcome to the internet computer developer community!
                        You're using dfx 0.11.2                                                     
To learn more before you start coding, see the documentation available online:

- Quick Start: https://internetcomputer.org/docs/current/developer-docs/quickstart/hello10mins/
- SDK Developer Tools: https://internetcomputer.org/docs/current/developer-docs/build/install-upgrade-remove/
- Motoko Language Guide: https://internetcomputer.org/docs/current/developer-docs/build/languages/motoko/
- Motoko Quick Reference: https://internetcomputer.org/docs/current/developer-docs/build/languages/motoko/language-manual

If you want to work on programs right away, try the following commands to get started:

    cd icp__dex
    dfx help
    dfx new --help

===============================================================================
```

### ローカルのキャニスター実行するコマンド

```zsh
cd icp__dex && dfx start --clean --background
```

stopする場合

```zsh
dfx stop
```

### キャニスターを実行環境に登録・ビルド・デプロイするコマンド

```zsh
dfx deploy
```

### 参考文献
1. [Motoko](https://internetcomputer.org/docs/current/developer-docs/build/cdks/motoko-dfinity/motoko/)