## SGD的momentum参数作用
梯度下降中的一种加速技术，如果上一次的momentum（冲量）与这一次的负梯度是相同的，那么这次下降的幅就会加大，因此可以起到加速收敛的作用，冲量的建议配置为0.9

# 使用sigmoid函数作为神经元，代价损失函数
如果使用方差代价函数（即采用均方误差MSE），会导致在梯度下降过程中w和b的更新非常慢，采用交叉熵代价函数可以克服这个问题

[参考](https://blog.csdn.net/u012162613/article/details/44239919)

另外，对数似然函数（log-likelihood cost）也常用来作为softmax的损失函数，softmax回归（使用softmax）是logistics回归（使用sigmoid）的多类别推广

[参考](http://deeplearning.stanford.edu/wiki/index.php/Softmax%E5%9B%9E%E5%BD%92)
