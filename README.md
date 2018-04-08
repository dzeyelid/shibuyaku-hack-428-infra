シブヤクハック 428チーム インフラ構築スクリプト
====

先日開催された [シブヤクハック](https://www.shibuyaku-hack.tech/) （ハッカソン）で作成した環境を再現するスクリプトです。

デプロイについて
====

Azure CLI 2.0 でデプロイする
----

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) でデプロイします。

まずこのリポジトリを `git clone` します。
そして、 `parameters.json` の下記の値を入力してから、デプロイしてください。

| キー名 | 説明 |
| --- | --- |
| `databaseForMySqlAdminName` | データベースの管理ユーザー |
| `databaseForMySqlAdminPassword` | データベースの管理ユーザーのパスワード |

```bash
GROUP=<resoure group name>
LOCATION=<locaton>
az group create --name ${GROUP} --location ${LOCATION}
az group deployment create --resource-group ${GROUP} --template-file azuredeploy.json --parameters @parameters.json
```

デプロイしたのち、 `Azure Database for MySQL` で下記を設定してください。

-  「接続のセキュリティ」をひらき、アクセス設定の「Azure サービスへのアクセスを許可」を「オン」に変更し、保存する。

その後、`Azure Web App` の「コンソール」で以下を実行して、データベースの初期化を行ってください。

```
bin\cake migrations migrate
```

`http://<resource group name>web.azurewebsites.net/users` にアクセスし、エラーが出なければデプロイ完了です。