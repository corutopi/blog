---
title: デフォルトの AWS Cloud9 環境では使用できないアクションがある件
date: 2023-10-14 00:44:00
tags: 
    - aws
    - aws cloud9
    - 小ネタ
---
AWS Cloud9 ではIAM権限で許可されていても使えないアクションがあるよって話。

---
## 結論
Cloud9 のデフォルトである AWS managed temporary credentials を使用している場合、[ここ](https://docs.aws.amazon.com/cloud9/latest/user-guide/security-iam.html#auth-and-access-control-temporary-managed-credentials)に記載されいているcloud9, iam, stsの一部のアクションは使用できない。

---
## 詳細
IAMリソースの整理がしたくてcloud9環境でaws cliをたたいてたら今回の事象に遭遇した。
```bash
$ aws iam get-user-policy --user-name 'tmp-user' --policy-name 'tmp-policy'

-> An error occurred (InvalidClientTokenId) when calling the GetUserPolicy operation: The security token included in the request is invalid
```
GetUserPolicy 権限は確かにつけているはずなのになぜか失敗する。
CloudShell で同じ権限で実行すると成功するのでしばらく悩んだ。

### AWS managed temporary credentials (AMTC)
特に何も考えずに作成したCloud9環境ではAMTCなるものがデフォルトとなっている。
これがオンになっている場合、Cloud9のコンソールからaws cliをたたくと**Cloud9を作成したIAMユーザーと同じ権限**を使って実行される。
```bash
$ aws sts get-caller-identity

->
{
    "UserId": "*********************",
    "Account": "012345678901",
    "Arn": "arn:aws:iam::012345678901:user/tmp-user"    # ここがcloud9作成者のIAMユーザーになる
}
```

すごく便利なんだけども、なぜか一部のアクションは制限されるらしい。なんで？
- https://docs.aws.amazon.com/cloud9/latest/user-guide/security-iam.html#auth-and-access-control-temporary-managed-credentials

ちなみに CloudShell も同じ仕組みでIAMユーザーの権限が使用できるようなっているが、こちらではアクションの制限はされていないっぽい。
(少なくとも今回私が遭遇した`iam:GetUserPolicy`のアクションは実行できた)

### 対処方法
AMTCを無効化して、EC2インスタンスに必要な権限を持ったIAMインスタンスプロファイルをアタッチすればそちらの権限を使える。

---
## 所感とか
解決はしたけど、なんで一部のアクションだけわざわざ使えないようにしているのかはわからずじまい。
一応ChatGPTに聞いてみたら凄くふわっとした回答でお茶を濁された。
![](/2023/10/14/aws-cloud9-unuse_action/ChatGPT_answer.png)

まぁ無料のGPT3.5だから、GPT4君だったらもっときれいな回答くれるのかも。
(その前にAWS公式の記事とかがないか探したほうがいい)

<!-- ---
## 参考URL -->


<!-- おわり -->