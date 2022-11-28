# ICP-DEX
ICPと連動するDEX開発用のリポジトリです。  
※ 本体は[こっち](https://github.com/mashharuki/icp__dex)

### スマートコントラクト開発言語 Motoko

Motokoは、DFINITY財団が開発中の新しいソフトウェア言語で、インターネット・コンピュータ・ブロックチェーン上で信頼性と保守性の高いウェブサイト、企業システム、インターネットシステムを、できるだけ幅広い層の開発者が簡単に作成できるように設計されています。

### DIP20

Psychedelicという組織が作成したトークン標準になります。これは、ERC-20のトークン標準をICP上でも利用できるようにMotokoとRustで実装されています。

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

### サブモジュールをインポートするコマンド

```zsh
git submodule add https://github.com/Psychedelic/DIP20.git
```

### デプロイをするユーザプリンシパル（識別子）を変数に登録するコマンド

```zsh
export ROOT_PRINCIPAL=$(dfx identity get-principal)
```

### DIP規格のトークンをデプロイするコマンド

- GoldTokenの場合
```zsh
dfx deploy GoldDIP20 --argument='("Token Gold Logo", "Token Gold", "TGLD", 8, 10_000_000_000_000_000, principal '\"$ROOT_PRINCIPAL\"', 0)'
```

- SilverTokenの場合

```zsh
dfx deploy SilverDIP20 --argument='("Token Silver Logo", "Token Silver", "TSLV", 8, 10_000_000_000_000_000, principal '\"$ROOT_PRINCIPAL\"', 0)'
```

上手く行くと下記のようにできる

```zsh
Creating a wallet canister on the local network.
The wallet canister on the "local" network for user "default" is "rwlgt-iiaaa-aaaaa-aaaaa-cai"
Deploying: GoldDIP20
Creating canisters...
Creating canister GoldDIP20...
GoldDIP20 canister created with canister id: rrkah-fqaaa-aaaaa-aaaaq-cai
Building canisters...
Installing canisters...
Creating UI canister on the local network.
The UI canister on the "local" network is "ryjl3-tyaaa-aaaaa-aaaba-cai"
Installing code for canister GoldDIP20, with canister ID rrkah-fqaaa-aaaaa-aaaaq-cai
Deployed canisters.
URLs:
  Backend canister via Candid interface:
    GoldDIP20: http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai
```

### メタデータを獲得するコマンド

- GoldTokenの場合
```zsh
dfx canister call GoldDIP20 getMetadata
```

- SilverTokenの場合
```zsh
dfx canister call SilverDIP20 getMetadata
```

取得した結果

```zsh
(
  record {
    fee = 0 : nat;
    decimals = 8 : nat8;
    owner = principal "wro4r-gzno2-ul3gr-lrqmq-rcww4-ye7v5-7fwxp-jlndv-pvwhh-zoveg-nae";
    logo = "Token Gold Logo";
    name = "Token Gold";
    totalSupply = 10_000_000_000_000_000 : nat;
    symbol = "TGLD";
  },
)
```

```zsh
(
  record {
    fee = 0 : nat;
    decimals = 8 : nat8;
    owner = principal "wro4r-gzno2-ul3gr-lrqmq-rcww4-ye7v5-7fwxp-jlndv-pvwhh-zoveg-nae";
    logo = "Token Silver Logo";
    name = "Token Silver";
    totalSupply = 10_000_000_000_000_000 : nat;
    symbol = "TSLV";
  },
)
```

### 予期せぬエラーを防ぐために.dfxフォルダは削除するようにしましょう！

```zsh
rm -rf .dfx
```

### テストコマンドの実行方法



### 参考文献
1. [Motoko](https://internetcomputer.org/docs/current/developer-docs/build/cdks/motoko-dfinity/motoko/)
2. [The Motoko base library](https://github.com/dfinity/motoko-base)
3. [Smacon dev](https://smacon.dev/)
4. [Backend Tutorial](https://internetcomputer.org/docs/current/developer-docs/build/backend/explore-templates)
5. [Motoko Example](https://github.com/dfinity/examples/tree/master/motoko)