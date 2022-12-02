# ICP-DEX
ICPと連動するDEX開発用のリポジトリです。  
※ 本体は[こっち](https://github.com/mashharuki/icp__dex)

### スマートコントラクト開発言語 Motoko

Motokoは、DFINITY財団が開発中の新しいソフトウェア言語で、インターネット・コンピュータ・ブロックチェーン上で信頼性と保守性の高いウェブサイト、企業システム、インターネットシステムを、できるだけ幅広い層の開発者が簡単に作成できるように設計されています。

### DIP20

Psychedelicという組織が作成したトークン標準になります。これは、ERC-20のトークン標準をICP上でも利用できるようにMotokoとRustで実装されています。

### Internet Identity

Internet Identityは、ICPがサポートするユーザー認証のフレームワークです。ユーザーは、認証にラップトップの指紋センサーや顔認証システムなどのデバイスと**アンカー（Anchor）**と呼ばれる数字を紐付けることが可能となる。その後、アンカーに紐づけたデバイスを利用して、さまざまなアプリケーションにサインアップし認証を行うことができる。

### stable

変数の宣言にstableキーワードを使用することで、その変数をステーブルなメモリ領域に保存することを指示できる。

### preupgrade と postupgrade

実は、全ての型の変数がstableキーワードで解決できる訳ではありません。例えばHashMapが挙げられます。ステーブル変数だけでは解決できない場合のために、Motokoはユーザー定義のアップグレードフックをサポートしています。このフックは、アップグレードの前後に実行されるものでpreupgradeとpostupgradeという特別な名前を持つsystem関数として宣言されます。アップグレード前に、ステーブルを定義できない変数からステーブル変数へデータを保存し、アップグレード後に元の型へ戻すというフックを定義するという使い方をします。

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

```shell
$ cd icp__dex
$ dfx start --clean --background
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

### メインネットへデプロイする方法

```zsh
export ROOT_PRINCIPAL=$(dfx identity get-principal)
```

```bash
$ dfx deploy GoldDIP20 --argument='("Token Gold Logo", "Token Gold", "TGLD", 8, 1_000_000_000_000, principal '\"$ROOT_PRINCIPAL\"', 0)'  --network ic --with-cycles 1000000000000
$ dfx deploy SilverDIP20 --argument='("Token Silver Logo", "Token Silver", "TSLV", 8, 1_000_000_000_000, principal '\"$ROOT_PRINCIPAL\"', 0)'  --network ic --with-cycles 1000000000000
$ dfx deploy faucet --network ic --with-cycles 1000000000000
```

```zsh
export IC_FAUCET_PRINCIPAL=$(dfx canister id --network ic faucet)
```

トークンをミントしてプールを作成します。

```bash
$ dfx canister call  --network ic GoldDIP20 mint '(principal '\"$IC_FAUCET_PRINCIPAL\"', 100_000)'
$ dfx canister call --network ic GoldDIP20 balanceOf '(principal '\"$IC_FAUCET_PRINCIPAL\"')'
$ dfx canister call  --network ic SilverDIP20 mint '(principal '\"$IC_FAUCET_PRINCIPAL\"', 100_000)'
$ dfx canister call --network ic SilverDIP20 balanceOf '(principal '\"$IC_FAUCET_PRINCIPAL\"')'
```

メインとなるキャニスターをデプロイする

```bash
$ dfx deploy icp__dex_backend --network ic --with-cycles 1000000000000
$ dfx deploy icp__dex_frontend --network ic --with-cycles 1000000000000
```

デプロイされたキャニスターの情報を確認するコマンド

```bash
$ dfx canister status --network ic icp__dex_backend
$ dfx canister status --network ic icp__dex_frontend
```

デプロイした参考情報

```zsh
URLs:
  Frontend canister via browser
    icp__dex_frontend: https://6xxmo-qiaaa-aaaag-aa3yq-cai.ic0.app/
  Backend canister via Candid interface:
    GoldDIP20: https://a4gq6-oaaaa-aaaab-qaa4q-cai.raw.ic0.app/?id=4djj2-viaaa-aaaag-aa3wq-cai
    SilverDIP20: https://a4gq6-oaaaa-aaaab-qaa4q-cai.raw.ic0.app/?id=4kkcg-daaaa-aaaag-aa3xa-cai
    faucet: https://a4gq6-oaaaa-aaaab-qaa4q-cai.raw.ic0.app/?id=4nles-oyaaa-aaaag-aa3xq-cai
    icp__dex_backend: https://a4gq6-oaaaa-aaaab-qaa4q-cai.raw.ic0.app/?id=6qwk2-5qaaa-aaaag-aa3ya-cai
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

```zsh
sh scripts/test.sh
```

result of test

```zsh
#------ faucet ------------
Using identity: "user1".
-n getToken    >  
(variant { Ok = 1_000 : nat })
-n balanceOf   >  
(1_000 : nat)
-e #------ faucet { Err = variant { AlreadyGiven } } ------------
(variant { Err = variant { AlreadyGiven } })
-e
Using identity: "user2".
-n getTOken    >  
(variant { Ok = 1_000 : nat })
-n balanceOf   >  
(1_000 : nat)
-e 

#------ deposit ------------
Using identity: "user1".
(variant { Ok = 6 : nat })
-n deposit     >  
(variant { Ok = 1_000 : nat })
-n getBalance  >  
(1_000 : nat)
-e
Using identity: "user2".
(variant { Ok = 6 : nat })
-n deposit     >  
(variant { Ok = 1_000 : nat })
-n getBalance  >  
(1_000 : nat)
-e 

#------ withdraw ------
Using identity: "user1".
-n withdraw    >  
(variant { Ok = 500 : nat })
-n balanceOf   >  
(500 : nat)
-n DEX balanceOf>  
(500 : nat)
-e #----- withdraw (check { Err = variant { BalanceLow } } -----
(variant { Err = variant { BalanceLow } })
-e 

#------ clean user ------
Using identity: "default".
Removed identity "user1".
Removed identity "user2".
```

### 残高確認コマンド

```bash
$ dfx wallet --network=ic balance
```

### 参考文献
1. [Motoko](https://internetcomputer.org/docs/current/developer-docs/build/cdks/motoko-dfinity/motoko/)
2. [The Motoko base library](https://github.com/dfinity/motoko-base)
3. [Smacon dev](https://smacon.dev/)
4. [Backend Tutorial](https://internetcomputer.org/docs/current/developer-docs/build/backend/explore-templates)
5. [Motoko Example](https://github.com/dfinity/examples/tree/master/motoko)
6. [【GitHub】internet-identity](https://github.com/dfinity/internet-identity/releases)