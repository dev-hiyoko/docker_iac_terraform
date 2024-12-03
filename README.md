# docker terraform

## 初期設定

```shell
cp .env.example .env &&\
make git/commit-template
```

## terraform コマンドリスト

```shell
docker compose run --rm terraform --version

docker compose run --rm terraform init

docker compose run --rm terraform fmt

docker compose run --rm terraform plan [ -var <キー>=<バリュー> | -var-file <ファイル> ]

docker compose run --rm terraform apply [ -var <キー>=<バリュー> | -var-file <ファイル> | -auto-approve ]

docker compose run --rm terraform destroy [ -auto-approve ]

docker compose run --rm terraform console
```

## aws-cli

ローカル環境をdockerで作成したい場合  

```shell
docker run --rm -it amazon/aws-cli --version
alias aws='docker run --rm -ti -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli'
aws --version
```

## ドキュメント

- [terraform](https://developer.hashicorp.com/terraform)
- [terraform provider](https://registry.terraform.io/browse/providers)
- [ブランチルール](./docs/git/branch.md)
- [コミットルール](./docs/git/commit.md)