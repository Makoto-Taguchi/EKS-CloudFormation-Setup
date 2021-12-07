# できるだけCloudFormationでEKSを構築
## 説明
AWS Load Balancer ControllerのTargetGroupBindingを利用することで、
CFnで作成したALBとEKS側で作成したfargate中のdeployment（pod群）を結合する。

## 構築手順
### 1. VPC, 踏み台EC2, ALB作成
CFn/VPC.ymlをデプロイ
CFn/EC2Bastion.ymlをデプロイ
CFn/ALB.ymlをデプロイ

### 2. EKSクラスター作成
CFn/EKS-Cluster.ymlをデプロイ

### 3. fargateプロファイル作成
適宜パラメータを記入
$ eksctl create fargateprofile -f eksctl/fp-all.yml

### 4. AWS Load Balancer ControllerをEKSにインストール
other/AWS-LBControllerインストール手順.txtを参考にインストール

### 5. Service, Deployment(nginxのサンプルアプリケーション)をデプロイ
$ kubectl apply -f kubectl/nginx.yml

### 6. TargetGroupBindingをデプロイ
適宜パラメータを記入
$ kubectl apply -f kubectl/target-group-binding.yml

### 7. デプロイ確認
ALBのDNS名でアクセスし、Nginxのデフォルト画面表示されればOK