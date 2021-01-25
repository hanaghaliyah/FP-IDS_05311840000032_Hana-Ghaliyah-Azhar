# Final Project IDS
<b> Sistem Deteksi Intrusi </b> <br>
```
Nama : Hana Ghaliyah Azhar  
NRP  : 05311840000032
Ide  : Sistem Deteksi Plat Kendaraan
```

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Program
Program ini digunakan untuk mendeteksi pelat nomor kendaraan sebagai representasi keberadaan kendaraan dalam sebuah gambar, video dan webcam. Pada program ini saya menggunakan You Only Look Once versi 3 (YOLO v.3) untuk mendeteksi pelat nomor di dalam gambar masukan. Metode ini memiliki keunggulan akurasi tinggi dan kinerja waktu nyata, menurut arsitektur YOLO v.3. Sistem yang disajikan menerima serangkaian gambar kendaraan dan menghasilkan gambar yang diproses dengan menambahkan kotak pembatas yang berisi pelat nomor kendaraan.

### Cara menjalankan
Test on a single image:
```
python object_detection_yolo.py --image=namafile.jpg
```
Test on a single video file:
```
python object_detection_yolo.py --video=namafile.mp4
```

### Hasil Deteksi Plat
- Output hasil deteksi dalam bentuk image
![car _yolo_out_py](https://user-images.githubusercontent.com/26424136/105654782-4c969b80-5ef1-11eb-9900-b1a875ae0334.jpg)
- Output hasil deteksi dalam bentuk video
![image](https://user-images.githubusercontent.com/26424136/105636029-b9ca1280-5e98-11eb-8ac8-86bf184c36fc.png)
 
### Notifikasi
Setelah sistem selesai melakukan deteksi plat, maka sistem akan mengirimkan notifikasi melalui Whatsapp dengan bantuan <b>twilio</b>
![Screenshot (538)](https://user-images.githubusercontent.com/26424136/105635848-d3b72580-5e97-11eb-98d5-2c8dcf2b72a7.png)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Referensi : 
https://github.com/0d3ng/plate-detection-pi <br>
https://github.com/alitourani/yolo-license-plate-detection <br>
https://github.com/pjreddie/darknet <br>
https://pjreddie.com/darknet/yolo/ <br>
Download model.weights -> https://drive.google.com/file/d/1vXjIoRWY0aIpYfhj3TnPUGdmJoHnWaOc/edit
