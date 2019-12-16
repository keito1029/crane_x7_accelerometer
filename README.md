# crane_x7_accelerometer
スマホの傾きに対応してcrane_x7が動くというパッケージです。

## ROS Bridgeのインストール
今回は他の方のパッケージを使用させてもらいました。
[https://github.com/AtsushiSaito/ros_smartphone_imu](https://github.com/AtsushiSaito/ros_smartphone_imu)にアクセスし、手順に従ってROS Bridgeのインストール等を行います。これによりスマホの加速度センサ等の値をPCに送ることができます。ngrokのインストールのところまで来たら、一旦このREADMEにあるngrokのインストールの項をお願いします。

## ngrokのインストール
ngrokのインストールは下記URLの手順に従ってお願いします。

[https://qiita.com/RayDoe/items/8a68f40d165819b82463](https://qiita.com/RayDoe/items/8a68f40d165819b82463)

iOSの方はSafariでは使用できないので、AppStoreにてgooglechromeをインストールし、そこから[https://atsushisaito.github.io/ros_smartphone_imu/](https://atsushisaito.github.io/ros_smartphone_imu/)にアクセスします。

IPアドレスは`ip a`というコマンドを実行し、2:の後、inetに続く数字（/まで）がIPアドレスです。

ROS Bridgeを実行し、rvizでスマホの傾きによって矢印が変化しているのが確認できたらそのままにしておきましょう。

注意※ : iOSとAndroidではx, y, zの正負が全て逆転します。a_sensor.pyに関してはiOS準拠ですので、Androidの方はa_sensor_And.pyを実行してもらいます。

## crane_x7_accelerometerのインストール
新規タブを開き、crane_x7_accelerometerを追加します。
```
cd ~/catkin_ws/src/crane_x7_ros
git clone https://github.com/habu94/crane_x7_accelerometer
cd ~/catkin_ws && catkin_make && source ~/catkin_ws/devel/setup.bash
```

## 立ち上げるもの

①まず先程のROSbridge
```
roslaunch ros_smartphone_imu rosbridge.launch
```
②続いてcrane_x7
```
roslaunch crane_x7_gazebo crane_x7_with_table.launch
```
実機の場合はPCと接続してから
```
sudo chmod 777 /dev/ttyUSB0
roslaunch crane_x7_bringup demo.launch fake_execution:=false
```
③そして「ngrok」の３つが立ち上がっていて、スマホの画面で「接続済み」と出ていることを確認して下さい。

# プログラムの実行
実行ファイルのあるディレクトリに移動します。
```
cd ~/catkin_ws/src/crane_x7_ros/crane_x7_accelerometer
```
## start.pyの実行

1.下記コマンドを実行
```
./start.py
```
2. 手先を下向きに移動させるためにスマホを机に画面を下向きで置く
3. 手先が下向きで移動しているのを確認できたら、スマホを傾けたままにすると、次第にconfig.rvizで表示されている矢印の向きに変わる
4. 矢印の位置に手先が来るようになったら、元の位置と往復しようとする前に、矢印の任意の方向に変える

※　取得したｘ座標をマイナスで代入しており、移動させたい目的の位置と移動させる前の位置を往復しますが、それは処理を終了させないための仕様です。

## a_sensorの実行

1. 下記コマンドを実行
```
./a_sensor.py
```
Androidの方
```
./a_sensor_And.py
```
2. スマホを上下左右に傾けると、上下左右のそれぞれ指定した場所に移動します
