# Tesseract OCR
打印字体识别引擎

## 安装
下载Leptonica源并安装:
```
$ wget http://www.leptonica.com/source/leptonica-1.76.0.tar.gz
$ tar xzvf leptonica-1.76.0.tar.gz
$ cd leptonica-1.76.0
$ ./configure & make & sudo make install
```

下载Tesseract源并安装:
```
$ wget https://sourceforge.net/projects/tesseract-ocr-alt/files/tesseract-ocr-3.02.02.tar.gz
$ tar xzf tesseract-ocr-3.02.02.tar.gz
$ cd tesseract-ocr
$ ./autogen.sh & ./configure & make & sudo make install & sudo ldconfig
```

下载语言文件：
```
$ wget https://sourceforge.net/projects/tesseract-ocr-alt/files/tesseract-ocr-3.02.eng.tar.gz
$ wget https://nchc.dl.sourceforge.net/project/tesseract-ocr-alt/tesseract-ocr-3.02.chi_sim.tar.gz
$ tar xzf tesseract-ocr-3.02.eng.tar.gz
$ sudo cp tesseract-ocr/tessdata/* /usr/local/share/tessdata
```

运行：
```
$ tesseract /path/to/input/test.jpg /path/to/output/abc.txt -l eng
```

## 报错：Error in pixReadStreamJpeg: function not present
读取jpeg stream错误，执行tesseract -v指令，发现缺少libjpeg，libpng，libtiff模块

安装libjpeg模块，`yum install -y libjpeg-devel`

安装libpng模块，`yum install -y libpng-devel`

安装libtiff模块，`yum install -y libtiff-devel`

重新安装leptonica

## 图片转灰度或者黑白
[参考](https://stackoverflow.com/questions/18777873/convert-rgb-to-black-or-white)
```
from PIL import Image

col = Image.open("cat-tied-icon.png")
gray = col.convert('L')
gray.save(‘result_grey.png’)

bw = gray.point(lambda x: 0 if x<128 else 255, '1')
bw.save("result_bw.png")
```
