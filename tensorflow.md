## tf.image.resize_images报错ValueError: 'images' contains no shape.
在执行tf.image.resize_images之前tf.image.decode_image的语句替换成

tf.image.decode_jpeg(如果图片是jpeg格式)

tf.image.decode_png(如果图片是png格式)

[参考](https://stackoverflow.com/questions/44942729/tensorflowvalueerror-images-contains-no-shape)


## UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 0: invalid start byte

将tensorflow 升级到1.12，当前python版本为3.6

[参考](https://tensorflow.google.cn/guide/saved_model#install_the_savedmodel_cli)

## dataset导入图片
```
def image_dataset(directory):

    # 函数的功能时将filename对应的图片文件读进来，并缩放到统一的大小
    def _parse_function(filename, label):
        image_string = tf.read_file(filename)
        image_decoded = tf.image.decode_jpeg(image_string)
        image_resized = tf.image.resize_images(image_decoded, [28, 28])
        return image_resized, label

    file_temp = []
    label_temp = []

    pos_directory = os.path.join(directory, 'positive')
    pos_fileList = os.listdir(pos_directory)
    for file in pos_fileList:
        file_temp.append(os.path.join(pos_directory, file))
        label_temp.append(1)

    neg_directory = os.path.join(directory, 'negative')
    neg_fileList = os.listdir(neg_directory)
    for file in neg_fileList:
        file_temp.append(os.path.join(neg_directory, file))
        label_temp.append(0)

    # 图片文件的列表
    filenames = tf.constant(file_temp)
    # label[i]就是图片filenames[i]的label
    labels = tf.constant(label_temp)

    # 此时dataset中的一个元素是(filename, label)
    dataset = tf.data.Dataset.from_tensor_slices((filenames, labels))

    # 此时dataset中的一个元素是(image_resized, label)
    dataset = dataset.map(_parse_function)

    return dataset
```

[参考](https://zhuanlan.zhihu.com/p/30751039)

## SGD的momentum参数作用
梯度下降中的一种加速技术，如果上一次的momentum（冲量）与这一次的负梯度是相同的，那么这次下降的幅就会加大，因此可以起到加速收敛的作用，冲量的建议配置为0.9

# 使用sigmoid函数作为神经元，代价损失函数
如果使用方差代价函数（即采用均方误差MSE），会导致在梯度下降过程中w和b的更新非常慢，采用交叉熵代价函数可以克服这个问题

[参考](https://blog.csdn.net/u012162613/article/details/44239919)

另外，对数似然函数（log-likelihood cost）也常用来作为softmax的损失函数，softmax回归（使用softmax）是logistics回归（使用sigmoid）的多类别推广

[参考](http://deeplearning.stanford.edu/wiki/index.php/Softmax%E5%9B%9E%E5%BD%92)
