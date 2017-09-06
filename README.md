# awselb

実行元のEC2インスタンスを、同VPC内にある `Classic Load Balancer(ELB)` から着脱させるコマンドです。

## 要件

- AWS EC2インスタンス用シェルコマンドです。
- ELBはClassicのみに対応しています。
- 使用するには適切なIAMポリシーが必要となります。
- 下記のコマンドに依存しているため、インストールが必要となります。
  - [aws cli](https://aws.amazon.com/jp/cli/)
  - [jq](https://stedolan.github.io/jq/)

## インストール

スクリプトを任意の場所にダウンロードしてから、実行権限の付与をしてください。

設置先はpathが通っている、 `/usr/local/bin` か `$HOME/bin` が推奨です。

```bash
 $ wget https://raw.githubusercontent.com/logicraft/awselb/master/awselb
 $ chmod +x awselb
```

必要なコマンドをインストールしてください。

```bash
 $ yum install -y jq
 $ pip install awscli   # Amazon Linuxはプレインストール済み
```

AWS CLIの設定を行って下さい。

IAMロールを利用するため、リージョン設定のみが必須となります。

```bash
$ aws configure
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: ap-northeast-1
Default output format [None]:
```

AWS ConsoleからIAMロールの作成をして、EC2インスタンスに紐付けてください。

必要なポリシーは下記です。

```json
{
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "elasticloadbalancing:DescribeInstanceHealth",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
                "elasticloadbalancing:DeregisterInstancesFromLoadBalancer"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

## 使い方

*コマンド*

`awselb`

*オプション*

- -x　　デバッグモード
- -h　　簡易ヘルプの表示

*サブコマンド*

- status　　EC2インスタンスが紐付いているELBが存在するか確認します。
- attach　　EC2インスタンスをELBに取り付けます。同じVPCに複数ELBが存在する場合は対話モードになります。
- detach　　ELBからEC2インスタンスを取り外します。


## ライセンス

[MIT](https://github.com/logicraft/awselb/blob/master/LICENSE)
