
# Docker及びSmartJumperのインストールと設定手順

[↑ 環境構築手順に戻る](../environment_construction.md)

## 目次

-   [1. RHELでのDockerインストール方法(Internetアクセス可の場合)](https://github.com/smartjumper/smartjumper-tech-info/blob/main/contents/docker_jumper_install_rhel94.md#1rhel%E3%81%A7%E3%81%AEdocker%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%96%B9%E6%B3%95internet%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E5%8F%AF%E3%81%AE%E5%A0%B4%E5%90%88)

-   [2. RHELでのDockerインストール方法(Internetアクセス不可の場合)](https://github.com/smartjumper/smartjumper-tech-info/blob/main/contents/docker_jumper_install_rhel94.md#2rhel%E3%81%A7%E3%81%AEdocker%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%96%B9%E6%B3%95internet%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E4%B8%8D%E5%8F%AF%E3%81%AE%E5%A0%B4%E5%90%88)

-   [3. SmartJumperのインストール](https://github.com/smartjumper/smartjumper-tech-info/blob/main/contents/docker_jumper_install_rhel94.md#3smartjumper%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)

## 動作確認環境

今回確認した環境は以下になります。

#### ・OSのバージョン

-   `RHEL： version9.4`

#### ・Dockerのバージョン

-   `docker 27.4.0`

#### ・SmartJumperのバージョン

-   `SmartJumper v1.1.0`

## 前提条件

-   Dockerが動くディストリビューション(RHEL9.4)がインストール済みであること
-   サブスクリプションが登録済みであること(インターネットアクセス可の場合)  
      
    


## 1. RHELでのDockerインストール方法(Internetアクセス可の場合)

参考にしたURLは以下となります。  
[https://docs.docker.com/engine/install/rhel/](https://docs.docker.com/engine/install/rhel/)

### 1-1. 競合の確認、アンインストール

競合するパッケージがあるとインストール時にエラーが出ることがあるため、事前にアンインストールしてください。  

アンインストールする非公式パッケージは次のとおりです。  

-   `docker.io`
-   `docker-compose`
-   `docker-compose-v2`
-   `docker-doc`
-   `podman-docker`

  

競合するパッケージを全てアンインストールするには、以下のコマンドを実行します。

#### 実行コマンド

```markdown
#sudo dnf remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine \
                  podman \
                  runc
```

#### 実行結果

```markdown
サブスクリプション管理リポジトリーを更新しています。
引数に一致する結果がありません: docker
引数に一致する結果がありません: docker-client
引数に一致する結果がありません: docker-client-latest
引数に一致する結果がありません: docker-common
引数に一致する結果がありません: docker-latest
引数に一致する結果がありません: docker-latest-logrotate
引数に一致する結果がありません: docker-logrotate

:
: 省略
:  

=========================================================================================================================================================================================================================
削除中:
 podman                                                 x86_64                                         2:4.9.4-0.1.el9                                          @AppStream                                          53 M
依存関係パッケージの削除:
 cockpit-podman                                         noarch                                         84.1-1.el9                                               @AppStream                                         682 k
未使用の依存関係の削除:
 conmon                                                 x86_64                                         2:2.1.10-1.el9                                           @AppStream                                         170 k

トランザクションの概要
=========================================================================================================================================================================================================================
削除  3 パッケージ

解放された容量: 53 M
これでよろしいですか? [y/N]: y
トランザクションを確認しています
トランザクションの確認に成功しました。


削除しました:
  cockpit-podman-84.1-1.el9.noarch                                          conmon-2:2.1.10-1.el9.x86_64                                          podman-2:4.9.4-0.1.el9.x86_64

完了しました!
```

### 1-2. rpmリポジトリを使用してインストールする

新しいホスト マシンに Docker Engine を初めてインストールする前に、Docker リポジトリを設定する必要があります。
その後は、リポジトリから Docker をインストールおよび更新できます。  
  
  
以下に実行コマンドと実行結果を記載します。

#### 実行コマンド/実行結果

```markdown
#sudo dnf install dnf-plugins-core
#sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo

サブスクリプション管理リポジトリーを更新しています。
メタデータの期限切れの最終確認: 0:05:59 前の 2024年12月20日 15時18分44秒 に実施しました。
パッケージ dnf-plugins-core-4.3.0-13.el9.noarch は既にインストールされています。
依存関係が解決しました。
=========================================================================================================================================================================================================================
 パッケージ                                                アーキテクチャー                        バージョン                                       リポジトリー                                                   サイズ
=========================================================================================================================================================================================================================
アップグレード:
 dnf-plugins-core                                          noarch                                  4.3.0-16.el9                                     rhel-9-for-x86_64-baseos-rpms                                   41 k
 python3-dnf-plugins-core                                  noarch                                  4.3.0-16.el9                                     rhel-9-for-x86_64-baseos-rpms                                  268 k

トランザクションの概要
=========================================================================================================================================================================================================================
アップグレード  2 パッケージ

ダウンロードサイズの合計: 309 k
パッケージのダウンロード:
(1/2): dnf-plugins-core-4.3.0-16.el9.noarch.rpm                                                                                                                                           68 kB/s |  41 kB     00:00
(2/2): python3-dnf-plugins-core-4.3.0-16.el9.noarch.rpm                                                                                                                                  396 kB/s | 268 kB     00:00
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
合計

:
: 省略
:  

サブスクリプション管理リポジトリーを更新しています。
repo の追加: https://download.docker.com/linux/rhel/docker-ce.repo
```

### 1-3. Dockerエンジンのインストール

#### 最新バージョンをインストールするには、以下のコマンドを実施

```markdown
#dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### 実行結果

```markdown
サブスクリプション管理リポジトリーを更新しています。
Docker CE Stable - x86_64                                                                                                                                                                 47 kB/s |  28 kB     00:00
依存関係が解決しました。
=========================================================================================================================================================================================================================
 パッケージ                                                    アーキテクチャー                           バージョン                                          リポジトリー                                         サイズ
=========================================================================================================================================================================================================================
インストール:
 containerd.io                                                 x86_64                                     1.7.24-3.1.el9                                      docker-ce-stable                                      43 M
 docker-buildx-plugin                                          x86_64                                     0.19.3-1.el9                                        docker-ce-stable                                      14 M
 docker-ce                                                     x86_64                                     3:27.4.1-1.el9                                      docker-ce-stable                                      27 M
 docker-ce-cli                                                 x86_64                                     1:27.4.1-1.el9                                      docker-ce-stable                                     8.0 M
 docker-compose-plugin                                         x86_64                                     2.32.1-1.el9                                        docker-ce-stable                                      14 M
弱い依存関係のインストール:
 docker-ce-rootless-extras                                     x86_64                                     27.4.1-1.el9                                        docker-ce-stable                                     4.4 M

トランザクションの概要

:
: 省略
:

インストール済み:
  containerd.io-1.7.24-3.1.el9.x86_64          docker-buildx-plugin-0.19.3-1.el9.x86_64    docker-ce-3:27.4.1-1.el9.x86_64    docker-ce-cli-1:27.4.1-1.el9.x86_64    docker-ce-rootless-extras-27.4.1-1.el9.x86_64
  docker-compose-plugin-2.32.1-1.el9.x86_64

完了しました!
```

### 1-4. Docker エンジンを起動

```markdown
#systemctl enable --now docker
```

#### 実行結果

```markdown
Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /usr/lib/systemd/system/docker.service.
```

### 1-5. インストールされているか確認

#### 実行コマンド

```markdown
#docker run hello-world
```

#### 実行結果

```markdown
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c1ec31eb5944: Pull complete 
Digest: sha256:5b3cc85e16e3058003c13b7821318369dad01dac3dbb877aac3c28182255c724
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
```

## 2. RHELでのDockerインストール方法(Internetアクセス不可の場合)

### 2-1. パッケージからのインストール方法

Docker のリポジトリを使用し`rpm`でDocker Engine をインストールできない場合は、`.rpm`リリースのファイルをダウンロードして手動でインストールできます。  
Docker Engine をアップグレードするたびに、新しいファイルをダウンロードする必要があります。  

#### 手順

1.  \[[https://download.docker.com/linux/rhel/](https://download.docker.com/linux/rhel/)\]へ移動します。
2.  リストから現在使用しているRHELのバージョンを選択します。  
    　RHELのバージョンは以下コマンドで確認できます。

#### 実行コマンド

```markdown
　#cat /etc/redhat-release
```

#### 実行結果

```markdown
Red Hat Enterprise Linux release 9.4 (Plow)
```

3.  リストから確認したバージョン(9.4)を選択します。
<font color="#FF0000">※本手順書はRHEL9.4/Docker27.4.0を前提に記載しています。ダウンロードするファイルのバージョンなどはご利用の環境に合わせて適宜読み替えてください。</font>
    
4.  該当するアーキテクチャ ( `x86_64`、`aarch64`、または`s390x`) を選択し、`stable/Packages/`に進みます。
    
5.  Docker Engine、CLI、containerd、および Docker Compose パッケージの次のファイルをダウンロードします。
    

-   `containerd.io-1.7.24-3.1.el9.x86_64.rpm`
-   `docker-ce-27.4.1-1.el9.x86_64.rpm`
-   `docker-ce-cli-27.4.1-1.el9.x86_64.rpm`
-   `docker-buildx-plugin-0.19.3-1.el9.x86_64.rpm`
-   `docker-compose-plugin-2.32.1-1.el9.x86_64.rpm`

6.  ダウンロードしたファイルを任意のディレクトリに置いて、以下のコマンドでインストールします。

#### 実行コマンド

```markdown
#rpm -ivh containerd.io-1.7.24-3.1.el9.x86_64.rpm 
#rpm -ivh docker-ce-cli-27.4.1-1.el9.x86_64.rpm
#rpm -ivh docker-ce-27.4.1-1.el9.x86_64.rpm 
#rpm -ivh docker-buildx-plugin-0.19.3-1.el9.x86_64.rpm
#rpm -ivh docker-compose-plugin-2.32.1-1.el9.x86_64.rpm

```

### 2-2. Docker エンジンを起動

#### 実行コマンド

```markdown
#systemctl enable --now docker
```

### 2-3. dockerのversionが正しく表示されることを確認

#### 実行コマンド

```markdown
#docker version
```

#### 実行結果

```markdown
Client: Docker Engine - Community
 Version:           27.4.0
 API version:       1.47
 Go version:        go1.22.10
 Git commit:        bde2b89
 Built:             Sat Dec  7 10:40:42 2024
 OS/Arch:           linux/amd64
 Context:           default
```

## 3. SmartJumperのインストール

### 前提条件

#### ・OS

-   `amd64(x86_64) アーキテクチャの、Ubuntu などLinux 環境で動作します`

#### ・ディスク容量

-   `SmartJumper をインストールに約5GB の容量を使います`
`それに加え、ターゲットと接続したときのオペレーションログは装置内に保存するため、保存期間などを  
考慮してディスク容量を確保してください`

#### ・メモリ

-   `SmartJumper は稼働中に定常的に約4GB を使用します`

### 3-1. インストーラーをダウンロードし、任意のディレクトリへ配置した後にインストーラーを実行

インストーラーは以下からダウンロードしてください  
[SmartJumperソフトウェア申請フォーム](https://ws.formzu.net/fgen/S54752725/?_gl=1*1pfteqd*_gcl_au*MjQ0MTAwNDMxLjE3MjkwNDM0OTI.*_ga*NjMzMTA4ODMyLjE1OTM0MDYwMzE.*_ga_78MV2EB8JQ*MTczNjMyMDQ5Ni4zNTAuMS4xNzM2MzIwNTQ1LjExLjAuMA..*_ga_HV6RRN1K5W*MTczNjMyMDQ5Ni4zNTEuMS4xNzM2MzIwNTQ0LjAuMC4w&_ga=2.49847526.1931747171.1736296141-633108832.1593406031)

### 3-2. インストーラーに実行権限を付与

#### 実行コマンド/実行結果

```markdown
#ls -l /tmp/smartjumper-installer-v1.1.0
-rw-r--r-- 1 root root 713975891 Nov 25 14:18 /tmp/smartjumper-installer-v1.1.0

#chmod +x /tmp/smartjumper-installer-v1.1.0
#ls -l /tmp/smartjumper-installer-v1.1.0
-rwxr-xr-x 1 root root 713975891 Nov 25 14:18 /tmp/smartjumper-installer-v1.1.0 
```

### 3-3. インストーラーを実行

#### 実行コマンド

```markdown
#./tmp/smartjumper-installer-v1.1.0
```

上記は /tmp 配下にインストーラーを配置した場合の実行コマンド です。
インストーラーを配置ディレクトリによって実行する指定するディレクトリは変更になります。

#### 実行結果

```markdown
Verifying installer checksum ... OK.
Preparing installer archive ... OK.
:
: 省略
:
stopping smartjumper
[+] Running 9/9
Container smartjumper-sshd-1 Removed 0.2s
Container smartjumper-nginx-1 Removed 0.2s
Smartjumper database is successfully initialized.
```

### 3-4. インストールされているか確認

#### 実行コマンド

```markdown
#smartjumper version
SmartJumper Version: v1.1.0
```

----------

### 3-5. SmartJumperの起動

以下コマンドでSmartJumperを起動します。

#### 実行コマンド

```markdown
#smartjumper start
```

#### 実行結果

```markdown
[+] Running 4/1
 ✔ Network smartjumper_default             Created                                                                                                                                                                  0.2s
 ✔ Container smartjumper-mysql-backend-1   Created                                                                                                                                                                  0.1s[+] Running 6/5
 ✔ Network smartjumper_default             Created                                                                                                                                                                  0.2s
 ⠙ Container smartjumper-mysql-backend-1   Starting                                                                                                                                                                 0.2s
 ⠙ Container smartjumper-mysql-frontend-1  Starting
:
: 省略
:
 ✔ Container smartjumper-sshproxy-1        Started                                                                                                                                                                  7.4s
 ✔ Container smartjumper-nginx-1           Started


```

以下コマンドでSmartJumperの状態を確認します。

#### 実行コマンド

```markdown
#smartjumper status
```

#### 実行結果

```markdown
#smartjumper status
SmartJumper Version: v1.1.0
SmartJumper Directory: /var/lib/smartjumper (exists)
SmartJumper Server: https://127.0.0.1:10443
Running: yes
Failed to get status: unauthenticated. Please login with auth login command

SmartJumper Services:
+----------------+-----------------------------+--------------+
| NAME           | IMAGE                       | IMAGE STATUS |
+----------------+-----------------------------+--------------+
| backend        | smartjumper/backend:v1.1.0  | exists       |
| frontend       | smartjumper/frontend:v1.1.0 | exists       |
| loki           | smartjumper/loki:v1.1.0     | exists       |
| mysql-backend  | smartjumper/mysql:v1.1.0    | exists       |
| mysql-frontend | smartjumper/mysql:v1.1.0    | exists       |
| nginx          | smartjumper/nginx:v1.1.0    | exists       |
| sshd           | smartjumper/sshd:v1.1.0     | exists       |
| sshproxy       | smartjumper/sshproxy:v1.1.0 | exists       |
+----------------+-----------------------------+--------------+ 
```

確認点としては以下になります。

-   `Running: yes となっていること`
-   `IMAGE のタグ情報が、インストールしたバージョンであること`
-   `IMAGE STATUS が全てexists であること`

### 3-6. 起動後の確認：GUIアクセス

SmartJumper のGUI アクセスが正常に動作しているかを確認します。

-   `ID : admin`
-   `パスワード: admin`
-   `プロトコル: HTTPS`
-   `ポート番号: 10443`

ブラウザーを起動して、URL 欄に  
https://<SmartJumper をインストールしたサーバーIP アドレスもしくはホスト名>:10443  
を入力し上記IDとパスワードを入力します。  
URLを叩いたらログイン画面が表示される事を確認します。

[↑ 環境構築手順に戻る](../environment_construction.md)
