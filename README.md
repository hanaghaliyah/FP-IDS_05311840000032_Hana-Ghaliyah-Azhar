# Project IDS
<b> Sistem Deteksi Intrusi </b> <br>
```
Nama : Hana Ghaliyah Azhar  
NRP  : 05311840000032
Ide  : Sistem Deteksi Plat Kendaraan
```

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Terdapat tiga tahap atau proses yang dilakukan pada sistem ini. Tiga tahan tersebut adalah sebagai berikut:
1. Mendeteksi Plat Kendaraan
2. Segmentasi Karakter Plat
3. Mengenali karakter itu sendiri

### Mendeteksi Plat Kendaraan
Mendeteksi pelat kendaraan adalah langkah yang pertama kali dilakukan untuk mengenali nomor pelat kendaraan, algoritmanya pun banyak untuk dapat melakukan proses ini dari yang sederahana sampai ribet tergantung dari kondisi atau kasus yang ada. Pada kesempatan kali ini saya akan mencoba untuk mengimplementasikan Contour Detection untuk mengetahui keberadaan objek pelat kendaraan. Prinsip dari algoritma ini adalah dengan mengekstrak kontur yang terdapat pada sebuah citra atau gambar, kontur yang berbentuk dan menyerupai pelat kendaraan yang dipilih sebagai kandidat sebuah pelat kendaraan.
#### Input Kendaraan 
- Mobil 1 (Tampak Depan) 
![car3](https://user-images.githubusercontent.com/26424136/104130821-7609e000-53a5-11eb-94ce-3a4008ab9aa4.jpg)
- Mobil 2 (Tampak Belakang)
![car4](https://user-images.githubusercontent.com/26424136/104130807-6c807800-53a5-11eb-86be-39fdacabe609.jpg)

Gambar di atas merupakan gambar input yang akan kita gunakan untuk mencoba mendeteksi suatu pelat kendaraan. 

#### Hasil Binerisasi
Hasil binerisasi ditunjukkan pada gambar di bawah ini
- Mobil 1 (Tampak Depan) 
![blackwhite](https://user-images.githubusercontent.com/26424136/104130899-e1ec4880-53a5-11eb-9a8a-64bee829ca38.jpg)
- Mobil 2 (Tampak Belakang)
![blackwhite](https://user-images.githubusercontent.com/26424136/104130923-09dbac00-53a6-11eb-9703-89769d40fc3a.jpg)

#### Hasil Deteksi Kontur
- Mobil 1 (Tampak Depan) 
![image](https://user-images.githubusercontent.com/26424136/104130901-e4e73900-53a5-11eb-8c76-a3151a9e7b0c.jpg)
- Mobil 2 (Tampak Belakang)
![image](https://user-images.githubusercontent.com/26424136/104130927-0cd69c80-53a6-11eb-8f06-e860bbc7bf65.jpg)


#### Hasil Deteksi Plat
- Mobil 1 (Tampak Depan) <br>
![car3](https://user-images.githubusercontent.com/26424136/104130828-81f5a200-53a5-11eb-9fd4-fbfe42f5cb8b.jpg)
- Mobil 2 (Tampak Belakang) <br>
![car4](https://user-images.githubusercontent.com/26424136/104130831-85892900-53a5-11eb-9011-5c962af531c3.jpg)

### Code
Install paket-paket yang dibutuhkan yaitu opencv-python. Beberapa yang perlu diimport adalah sebagai berikut
```
import cv2
import Utils
import sys
import os
```
Alur program yang akan kita buat adalah dengan cara membaca folder untuk dilakukan pengolahan gambar. Kita membutuhkan looping terkait kebutuhan tersebut
```
# Folder untuk menyimpan dataset
path_slice = "dataset/sliced"
path_source = "dataset/source"

# Template untuk proyeksi vertikal
pv_template = Utils.proyeksi_vertical(cv2.imread("dataset/templates/plate/template.jpg", cv2.IMREAD_ANYCOLOR))

for file_name in sorted(os.listdir(path_source)):
    image = cv2.imread(os.path.join(path_source, file_name))
    src = image.copy()
    blurred = image.copy()
    print(image.shape)
    # Filtering gaussian blur
    for i in range(10):
        blurred = cv2.GaussianBlur(image, (5, 5), 0.5)

    # Conversi image BGR2GRAY
    rgb = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

    # Image binerisasi menggunakan adaptive thresholding
    bw = cv2.adaptiveThreshold(rgb, 255.0, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY_INV, 5, 10)

    # Operasi dilasi
    bw = cv2.dilate(bw, cv2.getStructuringElement(cv2.MORPH_RECT, (1, 1)))
```
Setelah folder dibaca kemudian perlu dilakukan filtering untuk menghilangkan noise atau derau yang terdapat pada citra. Hasil dari citra yang telah dihilangkan noisenya, perlu dilakukan grayscale atau keabuan sebelum diubah menjadi citra biner. Proses binerisasi menggunakan adaptive thresholding. <br>
Selanjutnya setelah proses binerisasi, langkah yang perlu kita lakukan adalah melakukan ekstraksi kontur untuk mendapatkan semua kontur yang terdapat image biner hasil proses sebelumnya. Untuk masalah ekstraksi kontur dapat digunakan baris perintah di bawah ini
```
contours, hierarchy = cv2.findContours(bw, cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
```
Hasil dari ekstraksi kontur sangatlah banyak sehingga perlu dilakukan seleksi kontur untuk mendapatkan kontur yang benar-benar kandidat dari sebuah pelat. Berikut ini code yang dapat digunakan untuk kebutuhan tersebut
```
slices = []
    img_slices = image.copy()
    idx = 0
    for cnt in contours:
        x, y, w, h = cv2.boundingRect(cnt)
        area = cv2.contourArea(cnt)
        ras = format(w / h, '.2f')
        # Pilih kontur dengan ukuran dan rasio tertentu
        if 30 <= h and (100 <= w <= 400) and (2.7 <= float(ras) <= 4):
            idx = idx + 1
            print("x={}, y={}, w={}, h={}, area={}, rasio={}".format(x, y, w, h, area, ras))
            cv2.rectangle(image, (x, y), (x + w, y + h), (0, 0, 255), thickness=1)
            cv2.putText(image, "{}x{}".format(w, h), (x, int(y + (h / 2))), cv2.FONT_HERSHEY_PLAIN, 1, (0, 0, 255))
            cv2.putText(image, "{}".format(ras), (x + int(w / 2), y + h + 13), cv2.FONT_HERSHEY_PLAIN, 1,
                        (0, 0, 255))
            crop = img_slices[y:y - 3 + h + 6, x:x - 3 + w + 6]
            slices.append(crop)
```
Untuk menyeleksi sebuah kandidat pelat membutuhkan ukuran dan rasio dari sebuah pelat sendiri, ukuran dan rasio tergantung dari dataset yang Anda gunakan. Sebenarnya walaupun sudah diseleksi tetap saja masih ada kontur yang menyerupai pelat. <br>
Langkah terakhir yang perlu dilakukan adalah dengan melakukan proyeksi vertikal ataupun proyeksi horizontal. Metode ini adalah dengan cara menambahkan piksel secara vertikal ataupun horizontal. Jangan khawatir sudah saya sertakan untuk operasi tersebut, potongan programnya adalah sebagai berikut
```
def proyeksi_vertical(img):
    blurred = cv2.GaussianBlur(img.copy(), (5, 5), 0)
    gray = cv2.cvtColor(blurred.copy(), cv2.COLOR_BGR2GRAY)
    # cv2.imshow("gray", gray)
    # cv2.waitKey()
    resized = cv2.resize(gray.copy(), (450, 145))
    # cv2.imshow("resized", resized)
    # cv2.waitKey()
    ret, bw = cv2.threshold(resized.copy(), 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)
    # cv2.imshow("bw", bw)
    # cv2.waitKey()
    bw = bw / 255.
    bw_data = np.asarray(bw)
    pvertical = np.sum(bw_data, axis=1)
    return pvertical
```
Kandidat-kandidat pelat yang terdeteksi dilakukan proyeksi vertikal dibandingkan dengan template yang sebelumnya kita buat, untuk membuat template bisa dilakukan secara manual menggunakan tool image editor. Proses perbandingan tersebut tersaji dalam source code seperti di bawah ini
```
result = None
    max_value = sys.float_info.max
    for sl in slices:
        # cv2.imshow("slice ke {}".format(slices.index(sl) + 1), sl)
        # cv2.waitKey()
        pv_numpy = Utils.proyeksi_vertical(sl.copy())
        rs_sum = cv2.sumElems(cv2.absdiff(pv_template, pv_numpy))
        # print("sum: {} slice ke {}".format(rs_sum[0], slices.index(sl) + 1))
        if rs_sum[0] <= max_value:
            max_value = rs_sum[0]
            result = sl
    cv2.waitKey()
    cv2.imwrite(os.path.join(path_slice, file_name), result)
```
