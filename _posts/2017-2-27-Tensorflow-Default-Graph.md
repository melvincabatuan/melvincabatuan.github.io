

```python
import tensorflow as tf
graph = tf.get_default_graph()
graph.get_operations()
```




    []




```python
input_data = tf.constant(1.0)
operations = graph.get_operations()
operations
```




    [<tf.Operation 'Const' type=Const>,
     <tf.Operation 'Const_1' type=Const>,
     <tf.Operation 'Const_2' type=Const>,
     <tf.Operation 'Variable/initial_value' type=Const>,
     <tf.Operation 'Variable' type=VariableV2>,
     <tf.Operation 'Variable/Assign' type=Assign>,
     <tf.Operation 'Variable/read' type=Identity>,
     <tf.Operation 'mul' type=Mul>,
     <tf.Operation 'init' type=NoOp>,
     <tf.Operation 'Variable_1/initial_value' type=Const>,
     <tf.Operation 'Variable_1' type=VariableV2>,
     <tf.Operation 'Variable_1/Assign' type=Assign>,
     <tf.Operation 'Variable_1/read' type=Identity>,
     <tf.Operation 'mul_1' type=Mul>,
     <tf.Operation 'init_1' type=NoOp>,
     <tf.Operation 'Variable_2/initial_value' type=Const>,
     <tf.Operation 'Variable_2' type=VariableV2>,
     <tf.Operation 'Variable_2/Assign' type=Assign>,
     <tf.Operation 'Variable_2/read' type=Identity>,
     <tf.Operation 'mul_2' type=Mul>,
     <tf.Operation 'init_2' type=NoOp>,
     <tf.Operation 'Const_3' type=Const>]




```python
data
```




    <tf.Tensor 'Const:0' shape=() dtype=float32>




```python
with tf.Session() as sess:
    print(sess.run(data))
```

    10.0



```python
for op in graph.get_operations():
    print(op.name)
```

    Const
    Const_1


## Tensorflow Neuron


```python
weight = tf.Variable(0.6)
output_value = weight * input_data
init_op = tf.global_variables_initializer()
with tf.Session() as sess:
    sess.run(init_op)
    print(sess.run(output_value))
```

    0.6


# Reset


```python
graph = tf.reset_default_graph()
```

## Simple Neuron


```python
x = tf.constant(1.0, name='input')
w = tf.Variable(0.6, name='weight')
y = tf.multiply(w, x, name='output')
summary_writer = tf.summary.FileWriter('log_simple_neuron', sess.graph)
```


```python
%ls log_simple_neuron/
```

    events.out.tfevents.1488178576.DESKTOP-C97ET40



```python
!tensorboard --logdir=log_simple_neuron
```

    Starting TensorBoard b'41' on port 6006


```python
graph = tf.reset_default_graph()

x = tf.constant(1.0, name='input')
w = tf.Variable(0.6, name='weight')
y = tf.multiply(w, x, name='output')
y_ = tf.constant(0.0, name='correct_value')

loss = tf.pow(y - y_, 2, name='loss')

train_step = tf.train.GradientDescentOptimizer(0.025).minimize(loss)

for value in [x, w, y, y_, loss]:
    tf.summary.scalar(value.op.name, value)

summaries = tf.summary.merge_all()

sess = tf.Session()
summary_writer = tf.summary.FileWriter('log_simple_stats', sess.graph)

sess.run(tf.global_variables_initializer())
for i in range(100):
    summary_writer.add_summary(sess.run(summaries), i)
    sess.run(train_step)
```


```python
!tensorboard --logdir=log_simple_stats
```

    Starting TensorBoard b'41' on port 6006

