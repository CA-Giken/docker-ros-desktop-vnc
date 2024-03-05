# ROS環境構築 with Docker
## やってる事
- docker環境(仮想化環境)で色んなROSバージョンを動かしたい
- ROS自体はDocker環境で簡単に動かせるが、GUIが難しい。特にRvizはOpenGLを使用しているため。
- GUI描画のためにVNCでリモートデスクトップ化。OpenGL API利用のためにX11 server/client通信。
- これをひとまとめに動くようにしてくれてるのが、[https://github.com/Tiryoh/docker-ros-desktop-vnc]

## 現状の問題点
- このフォーク元レポジトリでもROS Kineticが対応外になっているので、このフォークレポジトリで対応する

# インストール
`$USER` はユーザ名
## Dockerインストール
```
sudo apt update
sudo apt -y install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$UBUNTU_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-compose
sudo groupadd docker
sudo usermod -aG docker $USER
sudo chmod 666 /var/run/docker.sock
```

### Dockerインストールテスト
```
docker ps -a
docker run hello-world
docker ps -a
docker rm [CONTAINER_ID]
docker rmi hello-world
```

# 起動とコンテナ内作業
## ROS Noetic
1. 以下を実行して環境・リモートデスクトップ立ち上げ
```
cd /path/to/this-repository
docker-compose up -d
```
2. `localhost:6080`をブラウザで起動
   
## ROS Kinetic
1. 以下を実行して環境・リモートデスクトップ立ち上げ
```
docker run -p 6080:80 --shm-size=512m tiryoh/ros-desktop-vnc:kinetic
docker ps -a
docker exec -it [CONTAINER_ID] bash
```
2. `localhost:6080`をブラウザで起動

