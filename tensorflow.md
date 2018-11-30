## dataset


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
