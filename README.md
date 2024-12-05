# docker terraform

aws-vaultを使用している

## 初期設定

1. コマンドの実行

    ```shell
    cp .env.example .env &&\
    make git/commit-template
    ```

2. ~/.aws等の設定

3. .envの設定

    ```text
    # terraformディレクトリ
    WORK_DIR=src
    ```

4. aws-vault設定

    ```shell
    # プロファイルの追加
    aws-vault add <プロファイル名>

    # プロファイルの実行
    aws-vault exec <プロファイル名>

    # 終了(aws-vaultを終了時には実行すること)
    exit
    ```

## terraform コマンドリスト

```shell
docker compose run --rm terraform --version

docker compose run --rm terraform init

docker compose run --rm terraform fmt

docker compose run --rm terraform validate

docker compose run --rm terraform plan [ -var <キー>=<バリュー> | -var-file <ファイル> ]

docker compose run --rm terraform apply [ -var <キー>=<バリュー> | -var-file <ファイル> | -auto-approve ]

docker compose run --rm terraform destroy [ -auto-approve ]

docker compose run --rm terraform console
```

## aws-cli

.zshrcに記載

```shell
aws() {
    envs=$(env | awk -v ORS=' ' '/AWS_(REGION|ACCESS_KEY_ID|SECRET_ACCESS_KEY|SESSION_TOKEN)/ {print "-e " $1}');
    zsh -c "docker run --rm -it $envs amazon/aws-cli $*"
}
```

動作確認

```shell
aws configure list 
```

## ドキュメント

- [terraform](https://developer.hashicorp.com/terraform)
- [terraform provider](https://registry.terraform.io/browse/providers)
- [ブランチルール](./docs/git/branch.md)
- [コミットルール](./docs/git/commit.md)

## aws-vaultを使用しない場合

1. .envの設定

    ```text
    # aws profile名(aws-vault不使用時)
    AWS_PROFILE=hiyoko-terraform
    ```

2. docker compose terraformサービスの修正

    ```text
    # 追加ブロック
    env_file:
      - .env
    # 削除ブロック
    # environment
    ```

3. aws-cli dockerの設定

    ```shell
    docker run --rm -it amazon/aws-cli --version
    alias aws='docker run --rm -ti -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli'
    aws --version
    ```
