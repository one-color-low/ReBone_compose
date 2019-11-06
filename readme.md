# Description
[ReBone](https://github.com/one-color-low/ReBone_v2)をnginx+gunicornでcomposeする用のリポジトリです。

web/Dockerfileにあるとおり、ReBoneのアプリ本体はgithubからzipをダウンロードするので、ファイル類を入れる必要はありません。

(ビルドを早くしたい場合は直接いれてもok。ただし.gitignoreに追加))

# Usage
## AWS EC2にデプロイ
1. セキュリティグループから、インバウンドのhttpアクセスを許可
2. ssh接続用のキーペアを用意
3. ec2にssh接続し、`git clone このリポジトリ`
4. docker ＆ docker-composeのインストール [参考](https://qiita.com/youtangai/items/ff67ceff5497a0e0b1af)
5. `ReBone_compose/`以下に移動し、`docker-compose up -d --build` でビルド＆起動
6. `http://{ ec2のパブリックip }/` にアクセス

## Note
- `web/Dockerfile`の頭でgpu版 or cpu版を切り替え可
    - cpu版の場合はvmd作成処理に10分弱かかります。