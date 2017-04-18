# tensorflow ctc
在语音识别中,我们经常会使用到`ctc loss`.`tensorflow`中有对于`ctc los
s`的实现,我们来看一下如何使用这个`API`.

## tf.nn.ctc_loss(labels, inputs, sequence_length, preprocess_collapse_repeated=False, ctc_merge_repeated=True)
此函数用来计算`ctc loss`.
- labels:是一个`int32`的`SparseTensor`, `labels.indices[i, :] == [b, t]` 表示 `labels.values[i]` 保存着`(batch b, time t)`的 `id`.

- inputs:一个`3D Tensor` `(max_time * batch_size * num_classes)`.保存着 `logits`.(通常是`RNN`接上一个线性神经元的输出)

- sequence_length: 1-D int32 向量, `size`为 `[batch_size]`. 序列的长度.此 `sequence_length` 和用在`dynamic_rnn`中的sequence_length是一致的,使用来表示`rnn`的哪些输出不是`pad`的.

- preprocess_collapse_repeated:设置为`True`的话,`tensorflow`会对输入的`labels`进行预处理,连续重复的会被合成一个

- ctc_merge_repeated:
返回值:
一个 1-D float Tensor, `size` 为 `[batch]`, 包含着负的 $\text{log}p$.加起来即为`batch loss`.

## tf.nn.ctc_greedy_decoder(inputs, sequence_length, merge_repeated=True)
上面的函数是用在训练过程中,专注与计算`loss`,此函数是用于`inference`过程中,用于解码.

- inputs:一个`3D Tensor` `(max_time * batch_size * num_classes)`.保存着 `logits`.(通常是`RNN`接上一个线性神经元的输出)

- sequence_length: 1-D int32 向量, `size`为 `[batch_size]`. 序列的长度.此 `sequence_length` 和用在`dynamic_rnn`中的sequence_length是一致的,使用来表示`rnn`的哪些输出不是`pad`的.

返回值:
一个`tuple (decoded, log_probabilities)`

- decoded: 一个只有一个元素的哦`list`. `decoded[0]`是一个`SparseTensor`,保存着解码的结果.
  -  decoded[0].indices: 索引矩阵,size为`(total_decoded_outputs * 2)`,每行中保存着`[batch, time ]`.
  -  decoded[0].values: 值向量,`size`为 `(total_decoded_outputs)`.向量中保存的是解码的类别.
  -  decoded[0].shape: 稠密`Tensor`的`shape`, size为`(2)`.`shape`的值为`[batch_size, max_decoded_length]`.

- log_probability: 一个浮点型矩阵`(batch_size*1)`包含着序列的log 概率.

## tf.nn.ctc_beam_search_decoder(inputs, sequence_length, beam_width=100, top_paths=1, merge_repeated=True)

## 参考资料
[http://stackoverflow.com/questions/38059247/using-tensorflows-connectionist-temporal-classification-ctc-implementation](http://stackoverflow.com/questions/38059247/using-tensorflows-connectionist-temporal-classification-ctc-implementation)
[https://www.tensorflow.org/versions/r0.10/api_docs/python/nn/conectionist_temporal_classification__ctc_](https://www.tensorflow.org/versions/r0.10/api_docs/python/nn/conectionist_temporal_classification__ctc_)
