# 220609012_ros2_env2
Ball Chaser Robot Project
Bu proje, Gazebo Harmonic simülasyon ortamında beyaz bir topu otonom olarak takip eden diferansiyel tahrikli bir robot sistemini içermektedir. ROS 2 Humble ve Python 3 kullanılarak geliştirilmiştir.

🎯 Proje Özellikleri
Otonom Top Takibi: Beyaz küreyi görüntü işleme ile tespit edip takip eder

Manuel Kontrol: Robot ve top için manuel kontrol seçenekleri

Gerçekçi Simülasyon: Gazebo Harmonic'te fizik tabanlı simülasyon

ROS 2 Entegrasyonu: Modern ROS 2 framework'ü ile entegre

TF Yayını: Robot koordinat sistemleri için transform yayını

📁 Proje Yapısı
text
bymodev2/
├── 📄 my_robot.urdf                    # Robot URDF modeli
├── 📄 bym_robot.sdf                    # Gazebo SDF dünyası
├── 📄 ball_controller.py               # Manuel top hareket ettirici
├── 📄 robot_ball_chaser.py             # Otonom top takip sistemi
├── 📄 simple_ball_control.py           # Basit top kontrolü
├── 📄 ssf.sh                          # SSF video kayıt betiği
├── 📄 test_setup.sh                   # Kurulum test betiği
├── 📄 README.md                       # Proje dokümantasyonu
├── 📄 .gitignore                      # Git ignore dosyası
└── 📁 my_robot_bringup/               # ROS 2 paketi
    ├── 📄 package.xml
    ├── 📄 CMakeLists.txt
    ├── 📁 launch/
    │   └── 📄 bringup.launch.py       # Ana launch dosyası
    ├── 📁 config/
    │   └── 📄 robot.rviz             # RViz konfigürasyonu
    ├── 📁 worlds/
    │   └── 📄 ball_chaser_world.sdf  # Gazebo dünyası
    └── 📁 scripts/
        └── 📄 ball_chaser.py         # Ball Chaser düğümü
🛠️ Gereksinimler
Sistem Gereksinimleri
Ubuntu 22.04 LTS (tavsiye edilen)

ROS 2 Humble Hawksbill

Gazebo Harmonic

Python 3.8+

Paket Gereksinimleri
bash
# Temel ROS 2 paketleri
sudo apt install ros-humble-desktop
sudo apt install ros-humble-gazebo-ros-pkgs
sudo apt install ros-humble-cv-bridge
sudo apt install ros-humble-vision-opencv
sudo apt install ros-humble-tf2-tools

# Python kütüphaneleri
pip install opencv-python numpy
⚡ Hızlı Kurulum
1. ROS 2 ve Gazebo Kurulumu
bash
# ROS 2 Humble kurulumu
sudo apt update && sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
sudo apt update
sudo apt install ros-humble-desktop

# Gazebo Harmonic kurulumu
sudo apt install gazebo-harmonic
2. Proje Dosyalarını Hazırlama
bash
# Çalışma dizinini oluştur
mkdir -p ~/Masaüstü/bymodev2
cd ~/Masaüstü/bymodev2

# Tüm proje dosyalarını bu dizine kopyala
# (ball_controller.py, robot_ball_chaser.py, my_robot.urdf, vb.)
3. ROS 2 Çalışma Alanını Kurma
bash
# ROS 2 environment'ını yükle
source /opt/ros/humble/setup.bash

# my_robot_bringup paketini oluştur
mkdir -p src
cp -r my_robot_bringup src/

# Bağımlılıkları kontrol et
rosdep update
rosdep install -i --from-path src --rosdistro humble -y

# Paketi derle
colcon build --packages-select my_robot_bringup
source install/setup.bash
4. Scriptleri Çalıştırılabilir Yapma
bash
chmod +x ball_controller.py
chmod +x robot_ball_chaser.py
chmod +x simple_ball_control.py
chmod +x ssf.sh
chmod +x test_setup.sh
chmod +x my_robot_bringup/scripts/ball_chaser.py
🚀 Kullanım
Senaryo 1: Tam Otomatik Çalıştırma
bash
# Terminal 1: SSF betiğini çalıştır (video kaydı başlar)
./ssf.sh

# Terminal 2: Ana sistemi başlat
ros2 launch my_robot_bringup bringup.launch.py
Senaryo 2: Manuel Test
bash
# Terminal 1: Gazebo'yu başlat
gz sim bym_robot.sdf

# Terminal 2: Manuel top kontrolü
python3 ball_controller.py

# Terminal 3: Otonom takip sistemi
python3 robot_ball_chaser.py
Senaryo 3: Geliştirici Modu
bash
# Kurulumu test et
./test_setup.sh

# Manuel olarak tüm bileşenleri başlat
source /opt/ros/humble/setup.bash
source install/setup.bash

# Gazebo
gz sim bym_robot.sdf

# Robot state publisher
ros2 launch my_robot_bringup bringup.launch.py

# Otonom takip
python3 robot_ball_chaser.py
🎮 Kontroller
Manuel Top Kontrolü (ball_controller.py)
W: İleri (X+)

S: Geri (X-)

A: Sol (Y+)

D: Sağ (Y-)

R: Pozisyonu sıfırla

Space: Mevcut pozisyonu göster

Q: Çıkış

Otonom Takip (robot_ball_chaser.py)
Q: Robotu tamamen durdur

Sistem otomatik olarak topu takip eder

🤖 Robot Özellikleri
Fiziksel Model
Gövde: 0.5m × 0.3m × 0.1m

Tekerlekler: 0.1m çap, diferansiyel tahrik

Kamera: 640×480 çözünürlük, 30 FPS

Serbest Teker: Denge için

Yazılım Özellikleri
Görüntü İşleme: OpenCV ile beyaz renk tespiti

PID Tabanlı Kontrol: Yumuşak hareket için

Durum Makinesi: Farklı senaryolara uyum

Hata Yönetimi: Robust hata yakalama

🔧 Teknik Detaylar
TF Frame Yapısı
text
world
└── odom
    └── base_link
        ├── left_wheel_link
        ├── right_wheel_link
        ├── caster_link
        └── camera_link
ROS Topic'leri
/cmd_vel → Robot hız komutları

/camera/image_raw → Kamera görüntüsü

/odom → Odometri bilgisi

/tf → Transform bilgileri

Gazebo Plugin'leri
libgazebo_ros_diff_drive.so → Diferansiyel sürüş

libgazebo_ros_camera.so → Kamera sensörü

🐛 Sorun Giderme
Yaygın Sorunlar ve Çözümleri
1. Kamera görüntüsü gelmiyor:

bash
# Topic'leri kontrol et
ros2 topic list
ros2 topic echo /camera/image_raw --once

# Köprüyü kontrol et
ros2 run ros_gz_bridge parameter_bridge /camera_sensor/image@sensor_msgs/msg/Image@gz.msgs.Image
2. Robot hareket etmiyor:

bash
# cmd_vel topic'ini dinle
ros2 topic echo /cmd_vel

# Servisleri kontrol et
ros2 service list
3. TF hataları:

bash
# TF ağacını görselleştir
ros2 run tf2_tools view_frames

# TF'leri dinle
ros2 run tf2_ros tf2_monitor
4. Paket bulunamıyor:

bash
# Çalışma alanını source'la
source /opt/ros/humble/setup.bash
source install/setup.bash

# Paketi tekrar derle
colcon build --packages-select my_robot_bringup
Debug Komutları
bash
# Sistem durumunu kontrol et
ros2 topic list
ros2 node list
ros2 service list

# Performans metrikleri
ros2 topic hz /camera/image_raw
ros2 topic hz /cmd_vel

# Görüntüyü görselleştir
ros2 run image_tools showimage --ros-args -r /camera/image_raw
📊 Test ve Doğrulama
Otomatik Test
bash
# Kurulum testi
./test_setup.sh

# Manuel test senaryoları
# 1. Top merkezde - Robot ileri gitmeli
# 2. Top sağda - Robot sağa dönmeli
# 3. Top solda - Robot sola dönmeli
# 4. Top yok - Robot aramalı
Performans Metrikleri
Görüntü İşleme: ~30 FPS

Tepki Süresi: <100ms

Hata Marjı: ±50 piksel

Maksimum Hız: 0.8 m/s lineer, 1.2 rad/s açısal

📝 Lisans ve Katkı
Bu proje eğitim amaçlı geliştirilmiştir. Sorularınız ve katkılarınız için:

GitHub Issues kullanın

Proje yapısını değiştirmeyin

Testleri güncellediğinizden emin olun

🔄 Güncellemeler
Son Güncellemeler
Otonom takip algoritması iyileştirmeleri

Manuel kontrol için yeni script'ler

Kapsamlı hata yönetimi

Detaylı dokümantasyon

Gelecek Özellikler
Çoklu top takibi

Engelden kaçınma

Web arayüzü

Veri kaydı ve analiz

Not: Bu proje ROS 2 ve Gazebo'nun temel kavramlarını anlamak ve robotik sistemlerin tasarımını öğrenmek için mükemmel bir başlangıç noktasıdır. Herhangi bir sorunla karşılaşırsanız, önce test betiklerini çalıştırarak kurulumunuzu doğrulayın.
