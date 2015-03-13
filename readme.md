# Auto

Kleines Projekt wo ein Auto aus Lego/Legotechnik und ein paar Sensoren zusammengebaut ist.  
Das Auto soll selbständig, oder über später über eine App, in der Gegend herum fahren, vor Hindernissen anhalten und einen anderen Weg suchen.  
  
Erste Testprogramm folgen demnächst.  
  
Folgende Hardware habe ich verwendet:  
  
- [Raspberry Pi Modul B+] (http://www.amazon.de/gp/product/B00LPM6U06?psc=1&redirect=true&ref_=oh_aui_search_detailpage "Raspberry Pi")  
- Wlan Modul: [Edimax EW-7612UAn] (http://www.amazon.de/gp/product/B007H5WXB0?psc=1&redirect=true&ref_=oh_aui_detailpage_o07_s00 "Wlan Adapter")  
- Ultraschallsensor: [HC-SR04] (http://www.amazon.de/gp/product/B00BIZQWYE?psc=1&redirect=true&ref_=oh_aui_detailpage_o03_s00 "HC-SR04")  
- Motor für Lenkung: [28BYJ-48 - 5V Stepper Motor] (http://www.amazon.de/gp/product/B00DGNO6PI?psc=1&redirect=true&ref_=oh_aui_detailpage_o02_s00 "5V Stepper Motor")  
- Motor für Beschleunigung: [70168 Double Gear Box 4-Speed] (http://www.amazon.de/gp/product/B001Q13BIU?psc=1&redirect=true&ref_=oh_aui_detailpage_o07_s00 "Motor für Beschleunigung")  
- Treiber für Beschleunigungsmotor: [ DC-Motortreiber] (http://www.amazon.de/gp/product/B00GZJQ6EE?psc=1&redirect=true&ref_=oh_aui_detailpage_o06_s00 "Motor Treiber")  
- Kamera:  [Raspberry Pi NoIR Kamera-Modul](http://www.amazon.de/gp/product/B00BIZQWYE?psc=1&redirect=true&ref_=oh_aui_detailpage_o03_s00 "Raspberry Pi NoIR Kamera-Modul")  

## Installation der einzelnen Sensoren

Kurze Anleitung um die einzelnen Sensoren lauffähig zu machen.  
  
Als erstes wird ein update und upgrade durchgeführt.
```
apt-get install update
apt-get install upgrade
```
  
Um die I/O Ports des Pi`s anzusprechen verwende ich wiringPi.
```
git clone git://git.dragon.net/wiringPi
cd wiringPi
./build
```
  
### Kamera

Als erstes muss die Kamera in der Config aktiviert und ein paar Tools installiert werden.
```
raspi-config
apt-get install subversion-tools libjpeg8-dev imagemagick
```
  
Um den notwendigen v4l2 Treiber zu laden:
```
rpi-update
modprobe bcm2835-v4l2
```
Will man ihn beim Systemstart automatisch laden muss der entsprechende eintrag in /etc/modules vorgenommen werden.
  
Für das streamen habe ich mich für den mjpg streamer entschieden.
```
mkdir /opt/mjpg-streamer
cd /opt/mjpg-streamer/
svn co https://svn.code.sf.net/p/mjpg-streamer/code/mjpg-streamer/ .
make
make install
```
  
Danach sollte in der Datei /opt/mjpg-streamer/start.sh die aktuelle Zeile auskommentiert und folgende eingefügt werden.
```
mjpg_streamer -i "/usr/local/lib/input_uvc.so -d /dev/video0 -n -y -r 640x480 -f 15" -o "/usr/local/lib/output_http.so -n -w /usr/local/www -p 8080"
```
  
Nun kann auf dem Laptop der Stream über den Browser geladen werden.
```
http://<IP des Raspberry>:8080
```
  