## Jupyter Notebookからカメラに繋がらない

```python
from jnmouse import Camera

camera = Camera.instance(width=300, height=300)
```

でエラーが出る場合があります。

以下のコマンドでJupyter Labの詳細なログを確認できます。  
ここで出力された内容を見て対処します。

```
$ journalctl -u jetbot_jupyter.service 
```

## カメラに繋がらない

```
Error generated. /dvs/git/dirty/git-master_linux/multimedia/nvgstreamer/gst-nvarguscamera/gstnvarguscamerasrc.cpp, execute:543 Failed to create CaptureSession
[ERROR] [1598606645.153198645]: Could not get gstreamer sample.
```

などとカメラに接続できないエラーが出る場合があります。

以下のコマンドで`nvargus-daemon`を再起動することでセッションをリスタートでき、多くの場合カメラに接続できるようになります。

```
$ sudo systemctl restart nvargus-daemon
```

参考：https://github.com/NVIDIA-AI-IOT/jetbot/issues/1

## `dmesg`のログについて

```
tegra-i2c 7000c400.i2c: no acknowledge from address 0x3c
```
と`dmesg`のログに残ることがあります。  
JetBotにはIPアドレス等ロボットの状態を表示するディスプレイつけますが、そのディスプレイと通信できなかったときにエラーが記録されます。  
（Jetson Nano Mouseにはロボットの状態を表示するディスプレイはありません）

以下のコマンドでロボットの状態を表示するディスプレイとの通信を停止できます。

```
$ sudo systemctl stop jetbot_stats.service
```

以下のコマンドでロボットの状態を表示するディスプレイと通信するためのサービスの自動起動を無効化できます。

```
$ sudo systemctl disable jetbot_stats.service
```