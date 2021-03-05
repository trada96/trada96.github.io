---
layout: post 
title: Hướng Dẫn Cài Đặt Và Sử Dụng Các Mô Hình OCR cho Tiếng Việt 
group: computer_vision
state: publish
---

Cài đặt các mô hình pretrained cho tiếng việt như EasyOCR, TranformerOCR, Tesseract, ...

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/ocr.jpg" height="700"/>
</div>

Trong bài viết này, mình sẽ giới thiệu và hướng dẫn cài đặt các mô hình OCR được train sẵn cho tiếng việt để mọi người
cũng như mình có thể dễ dàng sử dụng. Mà mình cũng nói trước nhé, các mô hình này được thiết kế để ăn sẵn, nó sẽ tốt với
bộ dữ liệu giống với bộ dữ liệu training set của họ(thường là data đẹp ), còn nếu muốn nâng cao lên với các trường hợp
ảnh khó hơn thì không có cách nào khác là tự xây dựng riêng cho mình một model OCR.

Các mô hình mình sử dụng sẽ được dẫn link ở cuối bài, nếu tác giả muốn gỡ thì hãy liên lạc với mình nha
(chắc không có đâu nhưng cứ lo xa chút :vv). Thôi mình không lan man nữa, bắt đầu nào :)))

Bài viết của mình sẽ gồm các phần như sau :

**1. OCR là gì?**

**2. TesseractOCR**

**3. EasyOCR**

**4. TransformerOCR (VietOCR)**

**5. Updating ...**

# ___Chi tiết nội dung___ :

## 1. OCR là gì?

OCR là từ viết tắt của optical character recognition, được dịch sang tiếng việt là nhận diện ký tự quang học, hiểu nôm
na là nó sẽ dự đoán xem ảnh input vào có chứa chữ gì như ví dụ sau:

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/cmnd.png"/>
</div>

**===> OUTPUT: 015071000030** 


### Ứng dụng của OCR là gì ?

Ứng dụng của OCR thì nhiều vô kể, miễn là trong ảnh chứa thông tin liên quan đến text 
thì output sẽ là nội dung của đoạn text đó:
 - Nhận diện thông tin cá nhân từ chứng minh nhân dân, thẻ căn cước, bằng lái xe, hộ chiếu, …
 - Đọc giá trị của công tơ điện từ ảnh chụp.
 - Nhận diện biển số xe, phát hiện từ khóa,..

## 2. Tesseract
Tesseract là ứng dụng OCR mã nguồn mở được tài trợ bằng Google. 
Nó hỗ trợ tốt nhất cho văn bản các văn bản tdf trong điều kiện read-only ( Không thể sao chép trực tiếp nội dung) .
Có thể sử dụng trực tiếp bằng code hoặc thông qua API.

Tesseract được phát triển để chạy trên cả 3 hệ điều hành phổ biến hiện nay như Window, 
Mac và Linux. Nó hỗ trợ trên 100 ngôn ngũo gồm khác nhau bao gồm cả tiếng việt.

### 2.1 Hướng dẫn cài đặt

Mình sẽ hướng dẫn cài đặt để sử dụng với python và linux nhé các bạn:

```console
$ sudo apt-get install tesseract-ocr
```

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/tesseract_install_1.png"/>
</div>

Tiếp tục 

```console
$ pip install pytesseract
```
<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/tesseract_install_2.png"/>
</div>

Download [pretrained cho tiếng việt](https://github.com/tesseract-ocr/tessdata/blob/master/vie.traineddata)
, rồi copy sang thư mục như sau:

```console
$ sudo cp vie.traineddata /usr/share/tesseract-ocr/4.00/tessdata/
```

### 2.2 Thử nghiệm


```python
import cv2
import pytesseract

# Load image
image = cv2.imread('17-35-29.png')

# Convert raw image -> Binaray image
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
gray = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)
gray = gray[1]
# Image To Text
text = pytesseract.image_to_string(gray, lang='vie')

print(text)
```

Ảnh đầu vào :

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/tesseract_input.png"/>
</div>

Output:

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/tesseract_output.png"/>
</div>

## 3. EasyOCR
EasyOCR là mô hình pretrained được hỗ trợ với 70+ ngôn ngữ khác nhau, danh sách ngôn ngữ được hỗ trợ có thể xem tại []
(https://www.jaided.ai/easyocr)


<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/easy_ocr_pipline.jpeg"/>
</div>

### 3.1 Hướng dẫn cài đặt

```console
$ pip install easyocr
```

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/easy_ocr_install.png"/>
</div>

Lưu ý, trong đoạn code easyocr.Reader([‘en’, ‘vi’]), bạn có thể chọn 1 hoặc nhiều hơn một ngôn ngữ để phát hiện.

### 3.2 Thực nghiệm

```python
import easyocr

reader = easyocr.Reader(['en', 'vi'])
result = reader.readtext('17-35-29.png')

print(result)
```

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/easy_ocr_input.png"/>
</div>

Kết quả: 

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/easy_ocr_output.png"/>
</div>

## 4. TransformerOCR (VietOCR)

Phương pháp này được xây dựng từ tác giả Phạm Bá Cường Quốc, vì trong blog này tác giả đã trình bày chi tiết về thuật toán cũng như so sánh ưu nhược điểm với phương pháp AttentionOCR hay CRNN+CTC Loss rồi nên mình sẽ không nói lại quá nhiều. Có thể hiểu đại ý như sau:

 - Phương pháp OCR thông thường là CNN + LSTM, mô hình cắt ảnh thì nhiều phần nhỏ theo chiều dọc, mỗi phần đó được đưa qua một mạng CNN nào đó (ở đây tác giả dùng VGG) để ra được 1 feature đặc trưng, rồi cho một list feature đó qua LSTM để dự đoán các kí tự trong ảnh.

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/viet_ocr_pipline.jpeg"/>
</div>

 - Phương pháp TransformerOCR cải tiến hơn bằng cách thay thế LSTM bằng một mô hình transformer (là mô hình nền tảng của BERT khá nổi tiếng).

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/viet_ocr_pipline_2.jpeg"/>
</div>

 - Ngoài ra phương pháp TransformerOCR sử dụng CrossEntropyLoss thay vì sử dụng CTC Loss do phương pháp CTC Loss có nhiều hạn chế.

### 4.1 Hướng dẫn cài đặt

```console
pip install vietocr==0.3.5
```

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/viet_ocr_install.png"/>
</div>

### 4.2 Thử nghiệm

```python
from vietocr.tool.predictor import Predictor
from vietocr.tool.config import Cfg
from PIL import Image

# sử dụng config của các bạn được export lúc train nếu đã thay đổi 
# tham số
# config = Cfg.load_config_from_file('config.yml')
# sử dụng config mặc định của mô hình
config = Cfg.load_config_from_name('vgg_transformer')
# đường dẫn đến trọng số đã huấn luyện hoặc comment để sử dụng 
#pretrained model mặc định
config['weights'] = 'transformerocr.pth'
config['device'] = 'cpu' # device chạy 'cuda:0', 'cuda:1', 'cpu'

detector = Predictor(config)

img = '17-35-29.png'
img = Image.open(img)
# dự đoán 
# muốn trả về xác suất của câu dự đoán thì đổi return_prob=True
s = detector.predict(img, return_prob=False)
print('TEXT OUTPUT: ',s)
```

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/viet_ocr_input.png"/>
</div>

Kết quả:

<div class="img-div" markdown="0">
    <img src="/images/ocr_pretrain/viet_ocr_output.png"/>
</div>


Một số lưu ý từ tác giả :
 - Phương pháp TransformerOCR dù được sử dụng công nghệ hiện đại nhưng chưa cải tiến quá nhiều so với phương pháp SOTA trước đó như AttentionOCR mà lại có thời gian dự đoán chậm hơn khá nhiều.
 - Pretrained model được huấn luyện với 10m ảnh chữ in nên nếu muốn sử dụng trong hệ thống product thực tế của bạn thì cần phải training thêm

# Updating ...

# Nguồn

- https://pbcquoc.github.io/vietocr/
- https://github.com/pbcquoc/vietocr
- https://github.com/JaidedAI/EasyOCR
- https://tesseract-ocr.github.io/