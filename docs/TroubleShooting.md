# トラブルシューティング

このページではJetson Nano Mouseの開発中に遭遇したエラー等の不具合への対処方法についてメモしています。

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
