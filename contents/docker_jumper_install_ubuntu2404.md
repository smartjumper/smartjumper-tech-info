
# Docker及びSmartJumperのインストールと設定手順

[↑ 環境構築手順に戻る](../environment_construction.md)  

## 目次

-   [1. UbuntuでのDockerインストール方法(Internetアクセス可の場合)](https://github.com/smartjumper/smartjumper-tech-info/blob/main/contents/docker_jumper_install_ubuntu2404.md#1ubuntu%E3%81%A7%E3%81%AEdocker%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%96%B9%E6%B3%95internet%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E5%8F%AF%E3%81%AE%E5%A0%B4%E5%90%88)

-   [2. UbuntuでのDockerインストール方法(Internetアクセス不可の場合)](https://github.com/smartjumper/smartjumper-tech-info/blob/main/contents/docker_jumper_install_ubuntu2404.md#2ubuntu%E3%81%A7%E3%81%AEdocker%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%96%B9%E6%B3%95internet%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E4%B8%8D%E5%8F%AF%E3%81%AE%E5%A0%B4%E5%90%88)

-   [3. SmartJumperのインストール](https://github.com/smartjumper/smartjumper-tech-info/blob/main/contents/docker_jumper_install_ubuntu2404.md#3smartjumper%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)  

## 動作確認環境

今回確認した環境は以下になります。

#### ・OSのバージョン

-   `ubuntu: version24.04`

#### ・Dockerのバージョン

-   `docker 27.4.0`

#### ・SmartJumperのバージョン

-   `SmartJumper v1.1.0`  

## 前提条件

Dockerが動くディストリビューション(Ubuntu 24.04)がインストール済みであること。  

## 1. UbuntuでのDockerインストール方法(Internetアクセス可の場合)

参考にしたURLは以下となります。  
[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)  

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
#for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done 
```

#### 実行結果

```markdown
#for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています... 完了        
状態情報を読み取っています... 完了        
パッケージ 'docker.io' はインストールされていないため削除もされません
以下のパッケージが自動でインストールされましたが、もう必要とされていません:
  git git-man liberror-perl libslirp0 pigz slirp4netns
これを削除するには 'sudo apt autoremove' を利用してください。
アップグレード: 0 個、新規インストール: 0 個、削除: 0 個、保留: 31 個。       

:
: 省略
:

パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています... 完了        
状態情報を読み取っています... 完了        
パッケージ 'runc' はインストールされていないため削除もされません
以下のパッケージが自動でインストールされましたが、もう必要とされていません:
  git git-man liberror-perl libslirp0 pigz slirp4netns
これを削除するには 'sudo apt autoremove' を利用してください。
アップグレード: 0 個、新規インストール: 0 個、削除: 0 個、保留: 31 個。
```  

### 1-2. Dockerのaptリポジトリを設定

新しいホスト マシンに Docker を初めてインストールする前に、Docker`apt`リポジトリを設定する必要があります。その後は、リポジトリから Docker をインストールおよび更新できます。  
  
  
以下に実行コマンドと実行結果を記載します。

#### 実行コマンド/実行結果

```markdown
#sudo apt-get update
ヒット:1 http://ftp.jaist.ac.jp/pub/Linux/ubuntu noble InRelease
取得:2 http://ftp.jaist.ac.jp/pub/Linux/ubuntu noble-updates InRelease [126 kB]
取得:3 http://ftp.jaist.ac.jp/pub/Linux/ubuntu noble-backports InRelease [126 kB]                                                                      
取得:4 http://ftp.jaist.ac.jp/pub/Linux/ubuntu noble-updates/main amd64 Packages [725 kB]                                                            
取得:5 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]                                     
取得:6 http://ftp.jaist.ac.jp/pub/Linux/ubuntu noble-updates/main amd64 Components [151 kB]                     

:
: 省略
:                                

取得:17 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Components [51.9 kB]
取得:18 http://security.ubuntu.com/ubuntu noble-security/multiverse amd64 Components [212 B]                             
ヒット:19 https://download.docker.com/linux/ubuntu noble InRelease                                                      
2563 kB を 3秒 で取得しました (940 kB/s)                                    
パッケージリストを読み込んでいます... 完了
W: https://download.docker.com/linux/ubuntu/dists/noble/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
#sudo apt-get install ca-certificates curl
パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています... 完了        
状態情報を読み取っています... 完了        
ca-certificates はすでに最新バージョン (20240203) です。
curl はすでに最新バージョン (8.5.0-2ubuntu10.5) です。
以下のパッケージが自動でインストールされましたが、もう必要とされていません:
  git git-man liberror-perl libslirp0 pigz slirp4netns
これを削除するには 'sudo apt autoremove' を利用してください。
アップグレード: 0 個、新規インストール: 0 個、削除: 0 個、保留: 31 個。
#sudo install -m 0755 -d /etc/apt/keyrings
#sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
#sudo chmod a+r /etc/apt/keyrings/docker.asc
#echo \
#"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
#$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
#sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
#sudo apt-get update
ヒット:1 http://ftp.jaist.ac.jp/pub/Linux/ubuntu noble InRelease
ヒット:2 http://ftp.jaist.ac.jp/pub/Linux/ubuntu noble-updates InRelease                        
ヒット:3 http://ftp.jaist.ac.jp/pub/Linux/ubuntu noble-backports InRelease                                                    
ヒット:4 https://download.docker.com/linux/ubuntu noble InRelease                                                             
ヒット:5 http://security.ubuntu.com/ubuntu noble-security InRelease                                                   
パッケージリストを読み込んでいます... 完了
W: ターゲット Packages (stable/binary-amd64/Packages) は /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-noble.list:1 と /etc/apt/sources.list.d/docker.list:1 で複数回設定されています
W: ターゲット Packages (stable/binary-all/Packages) は /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-noble.list:1 と /etc/apt/sources.list.d/docker.list:1 で複数回設定されています

:
: 省略
:

W: ターゲット DEP-11-icons-small (stable/dep11/icons-48x48.tar) は /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-noble.list:1 と /etc/apt/sources.list.d/docker.list:1 で複数回設定されています
W: ターゲット DEP-11-icons (stable/dep11/icons-64x64.tar) は /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-noble.list:1 と /etc/apt/sources.list.d/docker.list:1 で複数回設定されています
```

echo \ 以降は1行づつコピーして貼り付けてください。  

### 1-3. Dockerパッケージのインストール

#### 最新バージョンをインストールするには、以下のコマンドを実施

```markdown
#sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### 実行結果

```markdown
#sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
パッケージリストを読み込んでいます... 完了
依存関係ツリーを作成しています... 完了        
状態情報を読み取っています... 完了        
以下のパッケージが自動でインストールされましたが、もう必要とされていません:
  git git-man liberror-perl libslirp0 pigz slirp4netns
これを削除するには 'sudo apt autoremove' を利用してください。
以下のパッケージは「削除」されます:
  containerd.io* docker-buildx-plugin* docker-ce* docker-ce-cli* docker-ce-rootless-extras* docker-compose-plugin*
アップグレード: 0 個、新規インストール: 0 個、削除: 6 個、保留: 31 個。
この操作後に 444 MB のディスク容量が解放されます。
続行しますか? [Y/n] y
(データベースを読み込んでいます ... 現在 192299 個のファイルとディレクトリがインストールされています。)
docker-ce (5:27.4.0-1~ubuntu.24.04~noble) を削除しています ...

:
: 省略
:

(データベースを読み込んでいます ... 現在 192065 個のファイルとディレクトリがインストールされています。)
docker-ce (5:27.4.0-1~ubuntu.24.04~noble) の設定ファイルを削除しています ...
containerd.io (1.7.24-1) の設定ファイルを削除しています ...
```  

### 1-4. インストールされているか確認

#### 実行コマンド

```markdown
#sudo docker run hello-world
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

## 2. UbuntuでのDockerインストール方法(Internetアクセス不可の場合)  

### 2-1. パッケージからのインストール方法

`apt`でDocker Engineをインストールできない場合は、`deb`リリースのファイルをダウンロードして手動でインストールしてください。

#### 手順

1.  [https://download.docker.com/linux/ubuntu/dists/](https://download.docker.com/linux/ubuntu/dists/) へ移動します
2.  リストから現在使用しているUbuntuのバージョンを選択します  
    　Code nameは[ubuntu wiki](https://wiki.ubuntu.com/Releases)を参照  
    　最新版(24.04)を使用している方のdistsはNobleを選択します  

3.  `pool/stable/` に移動し、該当するアーキテクチャ（`amd64`、`armhf`、`arm64`、または`s390x`）を選択します
4.  以下のパッケージファイルをダウンロードします

-   `containerd.io_<version>_<arch>.deb`
-   `docker-ce_<version>_<arch>.deb`
-   `docker-ce-cli_<version>_<arch>.deb`
-   `docker-buildx-plugin_<version>_<arch>.deb`
-   `docker-compose-plugin_<version>_<arch>.deb`

5.  ダウンロードしたファイルを所定のフォルダに置いて、以下のコマンドでインストールします

#### 実行コマンド

```markdown
#sudo dpkg -i ./containerd.io_<version>_<arch>.deb \
  ./docker-ce_<version>_<arch>.deb \
  ./docker-buildx-plugin_<version>_<arch>.deb \
  ./docker-ce-cli_<version>_<arch>.deb \
  ./docker-compose-plugin_<version>_<arch>.deb
```  

### 2-2. Dockerのversionが正しく表示されることを確認します。

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

### 3-1. インストーラーをダウンロードし、任意のディレクトリへ配置した後にインストーラーを実行する

インストーラーは以下からダウンロードしてください  。
[SmartJumperソフトウェア申請フォーム](https://ws.formzu.net/fgen/S54752725/?_gl=1*1pfteqd*_gcl_au*MjQ0MTAwNDMxLjE3MjkwNDM0OTI.*_ga*NjMzMTA4ODMyLjE1OTM0MDYwMzE.*_ga_78MV2EB8JQ*MTczNjMyMDQ5Ni4zNTAuMS4xNzM2MzIwNTQ1LjExLjAuMA..*_ga_HV6RRN1K5W*MTczNjMyMDQ5Ni4zNTEuMS4xNzM2MzIwNTQ0LjAuMC4w&_ga=2.49847526.1931747171.1736296141-633108832.1593406031)  

### 3-2. インストーラーに実行権限を付与

#### 実行コマンド/実行結果

```markdown
#ls -l /tmp/smartjumper-installer-v1.1.0
-rw-r--r-- 1 root root 713975891 Nov 25 14:18 /tmp/smartjumper-installer-v1.1.0
# 
#chmod +x /tmp/smartjumper-installer-v1.1.0
#ls -l /tmp/smartjumper-installer-v1.1.0
-rwxr-xr-x 1 root root 713975891 Nov 25 14:18 /tmp/smartjumper-installer-v1.1.0
```  

### 3-3. インストーラーを実行

#### 実行コマンド

```markdown
#./tmp/smartjumper-installer-v1.1.0
```

上記は /tmp 配下にインストーラーを配置した場合の実行コマンドです。  
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

#### 実行コマンド/実行結果

```markdown
#smartjumper version
SmartJumper Version: v1.1.0
```  

#### 3-5. SmartJumperの起動

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
