

```python
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt
```

# Linear regression:  $y = Wx + b$


```python
W = tf.Variable(tf.zeros([1,1]))
x = tf.placeholder(tf.float32,[None, 1])
b = tf.Variable(tf.zeros([1]))
product = tf.matmul(x,W)
y = product + b
y_ = tf.placeholder(tf.float32,[None, 1])
```

## Cost function: $\dfrac{\sum \left( y\_\_{i} - y\_{i} \right)^2}{n}$


```python
cost = tf.reduce_mean(tf.square(y_-y))
```

## Gradient Descent for minimizing cost


```python
train_step = tf.train.GradientDescentOptimizer(0.0000001).minimize(cost)
```

## Running the graph for training


```python
with tf.Session() as sess:
    init_op = tf.global_variables_initializer()
    sess.run(init_op)
    steps = 1000
    for i in range(steps):
        # Create fake data for y = W.x + b where W = 2, b = 0
        xs = np.array([[i]])
        ys = np.array([[2*i]])
        # Train
        feed = { x: xs, y_: ys }
        sess.run(train_step, feed_dict=feed)
        print("After %d iteration:" % i)
        print("W: %f" % sess.run(W))
        print("b: %f" % sess.run(b))
        print("cost: %f" % sess.run(cost, feed_dict=feed))
```

    After 0 iteration:
    W: 0.000000
    b: 0.000000
    cost: 0.000000
    After 1 iteration:
    W: 0.000000
    b: 0.000000
    cost: 3.999997
    After 2 iteration:
    W: 0.000002
    b: 0.000001
    cost: 15.999958
    After 3 iteration:
    W: 0.000006
    b: 0.000002
    cost: 35.999771
    After 4 iteration:
    W: 0.000012
    b: 0.000004
    cost: 63.999168
    After 5 iteration:
    W: 0.000022
    b: 0.000006
    cost: 99.997673
    After 6 iteration:
    W: 0.000036
    b: 0.000008
    cost: 143.994553
    After 7 iteration:
    W: 0.000056
    b: 0.000011
    cost: 195.988708
    After 8 iteration:
    W: 0.000082
    b: 0.000014
    cost: 255.978638
    After 9 iteration:
    W: 0.000114
    b: 0.000018
    cost: 323.962433
    After 10 iteration:
    W: 0.000154
    b: 0.000022
    cost: 399.937531
    After 11 iteration:
    W: 0.000202
    b: 0.000026
    cost: 483.900879
    After 12 iteration:
    W: 0.000260
    b: 0.000031
    cost: 575.848755
    After 13 iteration:
    W: 0.000328
    b: 0.000036
    cost: 675.776672
    After 14 iteration:
    W: 0.000406
    b: 0.000042
    cost: 783.679382
    After 15 iteration:
    W: 0.000496
    b: 0.000048
    cost: 899.550903
    After 16 iteration:
    W: 0.000598
    b: 0.000054
    cost: 1023.383911
    After 17 iteration:
    W: 0.000714
    b: 0.000061
    cost: 1155.170898
    After 18 iteration:
    W: 0.000843
    b: 0.000068
    cost: 1294.902100
    After 19 iteration:
    W: 0.000988
    b: 0.000076
    cost: 1442.568115
    After 20 iteration:
    W: 0.001148
    b: 0.000084
    cost: 1598.157593
    After 21 iteration:
    W: 0.001324
    b: 0.000092
    cost: 1761.657471
    After 22 iteration:
    W: 0.001517
    b: 0.000101
    cost: 1933.054443
    After 23 iteration:
    W: 0.001729
    b: 0.000110
    cost: 2112.333008
    After 24 iteration:
    W: 0.001959
    b: 0.000120
    cost: 2299.477051
    After 25 iteration:
    W: 0.002209
    b: 0.000130
    cost: 2494.468018
    After 26 iteration:
    W: 0.002479
    b: 0.000140
    cost: 2697.286377
    After 27 iteration:
    W: 0.002770
    b: 0.000151
    cost: 2907.911377
    After 28 iteration:
    W: 0.003083
    b: 0.000162
    cost: 3126.319580
    After 29 iteration:
    W: 0.003419
    b: 0.000174
    cost: 3352.487549
    After 30 iteration:
    W: 0.003779
    b: 0.000186
    cost: 3586.387451
    After 31 iteration:
    W: 0.004162
    b: 0.000198
    cost: 3827.992188
    After 32 iteration:
    W: 0.004571
    b: 0.000211
    cost: 4077.271484
    After 33 iteration:
    W: 0.005006
    b: 0.000224
    cost: 4334.192871
    After 34 iteration:
    W: 0.005467
    b: 0.000238
    cost: 4598.723633
    After 35 iteration:
    W: 0.005956
    b: 0.000252
    cost: 4870.826172
    After 36 iteration:
    W: 0.006472
    b: 0.000266
    cost: 5150.462402
    After 37 iteration:
    W: 0.007018
    b: 0.000281
    cost: 5437.594238
    After 38 iteration:
    W: 0.007594
    b: 0.000296
    cost: 5732.176270
    After 39 iteration:
    W: 0.008200
    b: 0.000312
    cost: 6034.166016
    After 40 iteration:
    W: 0.008837
    b: 0.000327
    cost: 6343.513672
    After 41 iteration:
    W: 0.009507
    b: 0.000344
    cost: 6660.172363
    After 42 iteration:
    W: 0.010209
    b: 0.000360
    cost: 6984.088867
    After 43 iteration:
    W: 0.010945
    b: 0.000378
    cost: 7315.209961
    After 44 iteration:
    W: 0.011715
    b: 0.000395
    cost: 7653.475586
    After 45 iteration:
    W: 0.012520
    b: 0.000413
    cost: 7998.830078
    After 46 iteration:
    W: 0.013361
    b: 0.000431
    cost: 8351.208984
    After 47 iteration:
    W: 0.014239
    b: 0.000450
    cost: 8710.548828
    After 48 iteration:
    W: 0.015154
    b: 0.000469
    cost: 9076.780273
    After 49 iteration:
    W: 0.016107
    b: 0.000488
    cost: 9449.835938
    After 50 iteration:
    W: 0.017099
    b: 0.000508
    cost: 9829.639648
    After 51 iteration:
    W: 0.018131
    b: 0.000529
    cost: 10216.118164
    After 52 iteration:
    W: 0.019202
    b: 0.000549
    cost: 10609.191406
    After 53 iteration:
    W: 0.020315
    b: 0.000570
    cost: 11008.779297
    After 54 iteration:
    W: 0.021470
    b: 0.000592
    cost: 11414.794922
    After 55 iteration:
    W: 0.022667
    b: 0.000613
    cost: 11827.153320
    After 56 iteration:
    W: 0.023907
    b: 0.000635
    cost: 12245.762695
    After 57 iteration:
    W: 0.025191
    b: 0.000658
    cost: 12670.533203
    After 58 iteration:
    W: 0.026520
    b: 0.000681
    cost: 13101.362305
    After 59 iteration:
    W: 0.027894
    b: 0.000704
    cost: 13538.155273
    After 60 iteration:
    W: 0.029313
    b: 0.000728
    cost: 13980.808594
    After 61 iteration:
    W: 0.030780
    b: 0.000752
    cost: 14429.214844
    After 62 iteration:
    W: 0.032294
    b: 0.000776
    cost: 14883.268555
    After 63 iteration:
    W: 0.033856
    b: 0.000801
    cost: 15342.855469
    After 64 iteration:
    W: 0.035467
    b: 0.000826
    cost: 15807.861328
    After 65 iteration:
    W: 0.037127
    b: 0.000852
    cost: 16278.167969
    After 66 iteration:
    W: 0.038837
    b: 0.000878
    cost: 16753.656250
    After 67 iteration:
    W: 0.040597
    b: 0.000904
    cost: 17234.197266
    After 68 iteration:
    W: 0.042409
    b: 0.000931
    cost: 17719.666016
    After 69 iteration:
    W: 0.044273
    b: 0.000958
    cost: 18209.933594
    After 70 iteration:
    W: 0.046190
    b: 0.000985
    cost: 18704.861328
    After 71 iteration:
    W: 0.048160
    b: 0.001013
    cost: 19204.320312
    After 72 iteration:
    W: 0.050183
    b: 0.001041
    cost: 19708.160156
    After 73 iteration:
    W: 0.052262
    b: 0.001069
    cost: 20216.244141
    After 74 iteration:
    W: 0.054395
    b: 0.001098
    cost: 20728.423828
    After 75 iteration:
    W: 0.056583
    b: 0.001127
    cost: 21244.552734
    After 76 iteration:
    W: 0.058828
    b: 0.001157
    cost: 21764.474609
    After 77 iteration:
    W: 0.061130
    b: 0.001187
    cost: 22288.035156
    After 78 iteration:
    W: 0.063490
    b: 0.001217
    cost: 22815.076172
    After 79 iteration:
    W: 0.065907
    b: 0.001248
    cost: 23345.435547
    After 80 iteration:
    W: 0.068382
    b: 0.001279
    cost: 23878.945312
    After 81 iteration:
    W: 0.070917
    b: 0.001310
    cost: 24415.445312
    After 82 iteration:
    W: 0.073511
    b: 0.001341
    cost: 24954.757812
    After 83 iteration:
    W: 0.076165
    b: 0.001373
    cost: 25496.710938
    After 84 iteration:
    W: 0.078880
    b: 0.001406
    cost: 26041.132812
    After 85 iteration:
    W: 0.081656
    b: 0.001438
    cost: 26587.835938
    After 86 iteration:
    W: 0.084494
    b: 0.001471
    cost: 27136.650391
    After 87 iteration:
    W: 0.087394
    b: 0.001505
    cost: 27687.378906
    After 88 iteration:
    W: 0.090356
    b: 0.001538
    cost: 28239.843750
    After 89 iteration:
    W: 0.093381
    b: 0.001572
    cost: 28793.853516
    After 90 iteration:
    W: 0.096470
    b: 0.001607
    cost: 29349.210938
    After 91 iteration:
    W: 0.099622
    b: 0.001641
    cost: 29905.726562
    After 92 iteration:
    W: 0.102839
    b: 0.001676
    cost: 30463.203125
    After 93 iteration:
    W: 0.106121
    b: 0.001712
    cost: 31021.439453
    After 94 iteration:
    W: 0.109468
    b: 0.001747
    cost: 31580.234375
    After 95 iteration:
    W: 0.112880
    b: 0.001783
    cost: 32139.380859
    After 96 iteration:
    W: 0.116358
    b: 0.001819
    cost: 32698.683594
    After 97 iteration:
    W: 0.119903
    b: 0.001856
    cost: 33257.925781
    After 98 iteration:
    W: 0.123514
    b: 0.001893
    cost: 33816.894531
    After 99 iteration:
    W: 0.127193
    b: 0.001930
    cost: 34375.386719
    After 100 iteration:
    W: 0.130938
    b: 0.001967
    cost: 34933.187500
    After 101 iteration:
    W: 0.134751
    b: 0.002005
    cost: 35490.078125
    After 102 iteration:
    W: 0.138632
    b: 0.002043
    cost: 36045.847656
    After 103 iteration:
    W: 0.142582
    b: 0.002082
    cost: 36600.273438
    After 104 iteration:
    W: 0.146600
    b: 0.002120
    cost: 37153.140625
    After 105 iteration:
    W: 0.150687
    b: 0.002159
    cost: 37704.222656
    After 106 iteration:
    W: 0.154842
    b: 0.002198
    cost: 38253.304688
    After 107 iteration:
    W: 0.159067
    b: 0.002238
    cost: 38800.164062
    After 108 iteration:
    W: 0.163362
    b: 0.002278
    cost: 39344.570312
    After 109 iteration:
    W: 0.167726
    b: 0.002318
    cost: 39886.304688
    After 110 iteration:
    W: 0.172160
    b: 0.002358
    cost: 40425.140625
    After 111 iteration:
    W: 0.176664
    b: 0.002398
    cost: 40960.859375
    After 112 iteration:
    W: 0.181238
    b: 0.002439
    cost: 41493.230469
    After 113 iteration:
    W: 0.185883
    b: 0.002480
    cost: 42022.023438
    After 114 iteration:
    W: 0.190598
    b: 0.002522
    cost: 42547.011719
    After 115 iteration:
    W: 0.195384
    b: 0.002563
    cost: 43067.984375
    After 116 iteration:
    W: 0.200241
    b: 0.002605
    cost: 43584.699219
    After 117 iteration:
    W: 0.205168
    b: 0.002647
    cost: 44096.937500
    After 118 iteration:
    W: 0.210166
    b: 0.002690
    cost: 44604.472656
    After 119 iteration:
    W: 0.215235
    b: 0.002732
    cost: 45107.082031
    After 120 iteration:
    W: 0.220375
    b: 0.002775
    cost: 45604.535156
    After 121 iteration:
    W: 0.225586
    b: 0.002818
    cost: 46096.621094
    After 122 iteration:
    W: 0.230868
    b: 0.002862
    cost: 46583.109375
    After 123 iteration:
    W: 0.236221
    b: 0.002905
    cost: 47063.777344
    After 124 iteration:
    W: 0.241645
    b: 0.002949
    cost: 47538.410156
    After 125 iteration:
    W: 0.247140
    b: 0.002993
    cost: 48006.785156
    After 126 iteration:
    W: 0.252706
    b: 0.003037
    cost: 48468.695312
    After 127 iteration:
    W: 0.258342
    b: 0.003081
    cost: 48923.910156
    After 128 iteration:
    W: 0.264049
    b: 0.003126
    cost: 49372.222656
    After 129 iteration:
    W: 0.269826
    b: 0.003171
    cost: 49813.429688
    After 130 iteration:
    W: 0.275674
    b: 0.003216
    cost: 50247.308594
    After 131 iteration:
    W: 0.281592
    b: 0.003261
    cost: 50673.667969
    After 132 iteration:
    W: 0.287581
    b: 0.003306
    cost: 51092.292969
    After 133 iteration:
    W: 0.293639
    b: 0.003352
    cost: 51502.984375
    After 134 iteration:
    W: 0.299767
    b: 0.003397
    cost: 51905.542969
    After 135 iteration:
    W: 0.305964
    b: 0.003443
    cost: 52299.773438
    After 136 iteration:
    W: 0.312230
    b: 0.003489
    cost: 52685.484375
    After 137 iteration:
    W: 0.318566
    b: 0.003536
    cost: 53062.480469
    After 138 iteration:
    W: 0.324970
    b: 0.003582
    cost: 53430.585938
    After 139 iteration:
    W: 0.331442
    b: 0.003629
    cost: 53789.609375
    After 140 iteration:
    W: 0.337983
    b: 0.003675
    cost: 54139.367188
    After 141 iteration:
    W: 0.344592
    b: 0.003722
    cost: 54479.703125
    After 142 iteration:
    W: 0.351267
    b: 0.003769
    cost: 54810.429688
    After 143 iteration:
    W: 0.358010
    b: 0.003816
    cost: 55131.378906
    After 144 iteration:
    W: 0.364820
    b: 0.003864
    cost: 55442.390625
    After 145 iteration:
    W: 0.371696
    b: 0.003911
    cost: 55743.312500
    After 146 iteration:
    W: 0.378637
    b: 0.003959
    cost: 56033.988281
    After 147 iteration:
    W: 0.385644
    b: 0.004006
    cost: 56314.269531
    After 148 iteration:
    W: 0.392716
    b: 0.004054
    cost: 56584.000000
    After 149 iteration:
    W: 0.399853
    b: 0.004102
    cost: 56843.046875
    After 150 iteration:
    W: 0.407054
    b: 0.004150
    cost: 57091.281250
    After 151 iteration:
    W: 0.414318
    b: 0.004198
    cost: 57328.570312
    After 152 iteration:
    W: 0.421645
    b: 0.004246
    cost: 57554.781250
    After 153 iteration:
    W: 0.429034
    b: 0.004295
    cost: 57769.812500
    After 154 iteration:
    W: 0.436485
    b: 0.004343
    cost: 57973.531250
    After 155 iteration:
    W: 0.443998
    b: 0.004392
    cost: 58165.839844
    After 156 iteration:
    W: 0.451571
    b: 0.004440
    cost: 58346.632812
    After 157 iteration:
    W: 0.459204
    b: 0.004489
    cost: 58515.820312
    After 158 iteration:
    W: 0.466897
    b: 0.004537
    cost: 58673.300781
    After 159 iteration:
    W: 0.474649
    b: 0.004586
    cost: 58819.000000
    After 160 iteration:
    W: 0.482458
    b: 0.004635
    cost: 58952.835938
    After 161 iteration:
    W: 0.490325
    b: 0.004684
    cost: 59074.734375
    After 162 iteration:
    W: 0.498249
    b: 0.004733
    cost: 59184.625000
    After 163 iteration:
    W: 0.506229
    b: 0.004782
    cost: 59282.457031
    After 164 iteration:
    W: 0.514264
    b: 0.004831
    cost: 59368.175781
    After 165 iteration:
    W: 0.522354
    b: 0.004880
    cost: 59441.722656
    After 166 iteration:
    W: 0.530497
    b: 0.004929
    cost: 59503.078125
    After 167 iteration:
    W: 0.538694
    b: 0.004978
    cost: 59552.187500
    After 168 iteration:
    W: 0.546942
    b: 0.005027
    cost: 59589.042969
    After 169 iteration:
    W: 0.555242
    b: 0.005076
    cost: 59613.609375
    After 170 iteration:
    W: 0.563593
    b: 0.005125
    cost: 59625.867188
    After 171 iteration:
    W: 0.571993
    b: 0.005174
    cost: 59625.835938
    After 172 iteration:
    W: 0.580442
    b: 0.005223
    cost: 59613.480469
    After 173 iteration:
    W: 0.588939
    b: 0.005273
    cost: 59588.843750
    After 174 iteration:
    W: 0.597483
    b: 0.005322
    cost: 59551.906250
    After 175 iteration:
    W: 0.606073
    b: 0.005371
    cost: 59502.710938
    After 176 iteration:
    W: 0.614709
    b: 0.005420
    cost: 59441.277344
    After 177 iteration:
    W: 0.623389
    b: 0.005469
    cost: 59367.632812
    After 178 iteration:
    W: 0.632112
    b: 0.005518
    cost: 59281.816406
    After 179 iteration:
    W: 0.640877
    b: 0.005567
    cost: 59183.886719
    After 180 iteration:
    W: 0.649684
    b: 0.005616
    cost: 59073.906250
    After 181 iteration:
    W: 0.658532
    b: 0.005665
    cost: 58951.906250
    After 182 iteration:
    W: 0.667418
    b: 0.005713
    cost: 58817.964844
    After 183 iteration:
    W: 0.676343
    b: 0.005762
    cost: 58672.171875
    After 184 iteration:
    W: 0.685306
    b: 0.005811
    cost: 58514.589844
    After 185 iteration:
    W: 0.694305
    b: 0.005860
    cost: 58345.312500
    After 186 iteration:
    W: 0.703339
    b: 0.005908
    cost: 58164.441406
    After 187 iteration:
    W: 0.712407
    b: 0.005957
    cost: 57972.062500
    After 188 iteration:
    W: 0.721509
    b: 0.006005
    cost: 57768.281250
    After 189 iteration:
    W: 0.730642
    b: 0.006053
    cost: 57553.222656
    After 190 iteration:
    W: 0.739807
    b: 0.006102
    cost: 57327.000000
    After 191 iteration:
    W: 0.749001
    b: 0.006150
    cost: 57089.742188
    After 192 iteration:
    W: 0.758224
    b: 0.006198
    cost: 56841.578125
    After 193 iteration:
    W: 0.767475
    b: 0.006246
    cost: 56582.636719
    After 194 iteration:
    W: 0.776752
    b: 0.006294
    cost: 56313.066406
    After 195 iteration:
    W: 0.786055
    b: 0.006341
    cost: 56033.015625
    After 196 iteration:
    W: 0.795382
    b: 0.006389
    cost: 55742.636719
    After 197 iteration:
    W: 0.804731
    b: 0.006436
    cost: 55442.097656
    After 198 iteration:
    W: 0.814103
    b: 0.006484
    cost: 55131.558594
    After 199 iteration:
    W: 0.823495
    b: 0.006531
    cost: 54811.191406
    After 200 iteration:
    W: 0.832907
    b: 0.006578
    cost: 54481.164062
    After 201 iteration:
    W: 0.842337
    b: 0.006625
    cost: 54141.660156
    After 202 iteration:
    W: 0.851784
    b: 0.006672
    cost: 53792.867188
    After 203 iteration:
    W: 0.861247
    b: 0.006718
    cost: 53434.976562
    After 204 iteration:
    W: 0.870725
    b: 0.006765
    cost: 53068.167969
    After 205 iteration:
    W: 0.880216
    b: 0.006811
    cost: 52692.656250
    After 206 iteration:
    W: 0.889720
    b: 0.006857
    cost: 52308.644531
    After 207 iteration:
    W: 0.899235
    b: 0.006903
    cost: 51916.328125
    After 208 iteration:
    W: 0.908759
    b: 0.006949
    cost: 51515.921875
    After 209 iteration:
    W: 0.918292
    b: 0.006994
    cost: 51107.640625
    After 210 iteration:
    W: 0.927832
    b: 0.007040
    cost: 50691.695312
    After 211 iteration:
    W: 0.937379
    b: 0.007085
    cost: 50268.320312
    After 212 iteration:
    W: 0.946930
    b: 0.007130
    cost: 49837.726562
    After 213 iteration:
    W: 0.956485
    b: 0.007175
    cost: 49400.156250
    After 214 iteration:
    W: 0.966043
    b: 0.007220
    cost: 48955.824219
    After 215 iteration:
    W: 0.975601
    b: 0.007264
    cost: 48504.968750
    After 216 iteration:
    W: 0.985160
    b: 0.007308
    cost: 48047.828125
    After 217 iteration:
    W: 0.994717
    b: 0.007352
    cost: 47584.632812
    After 218 iteration:
    W: 1.004272
    b: 0.007396
    cost: 47115.625000
    After 219 iteration:
    W: 1.013823
    b: 0.007440
    cost: 46641.039062
    After 220 iteration:
    W: 1.023369
    b: 0.007483
    cost: 46161.128906
    After 221 iteration:
    W: 1.032908
    b: 0.007526
    cost: 45676.128906
    After 222 iteration:
    W: 1.042440
    b: 0.007569
    cost: 45186.285156
    After 223 iteration:
    W: 1.051964
    b: 0.007612
    cost: 44691.847656
    After 224 iteration:
    W: 1.061477
    b: 0.007655
    cost: 44193.058594
    After 225 iteration:
    W: 1.070979
    b: 0.007697
    cost: 43690.179688
    After 226 iteration:
    W: 1.080469
    b: 0.007739
    cost: 43183.433594
    After 227 iteration:
    W: 1.089945
    b: 0.007780
    cost: 42673.078125
    After 228 iteration:
    W: 1.099407
    b: 0.007822
    cost: 42159.371094
    After 229 iteration:
    W: 1.108852
    b: 0.007863
    cost: 41642.554688
    After 230 iteration:
    W: 1.118280
    b: 0.007904
    cost: 41122.871094
    After 231 iteration:
    W: 1.127689
    b: 0.007945
    cost: 40600.566406
    After 232 iteration:
    W: 1.137079
    b: 0.007985
    cost: 40075.890625
    After 233 iteration:
    W: 1.146448
    b: 0.008026
    cost: 39549.093750
    After 234 iteration:
    W: 1.155795
    b: 0.008066
    cost: 39020.402344
    After 235 iteration:
    W: 1.165119
    b: 0.008105
    cost: 38490.074219
    After 236 iteration:
    W: 1.174419
    b: 0.008145
    cost: 37958.355469
    After 237 iteration:
    W: 1.183693
    b: 0.008184
    cost: 37425.480469
    After 238 iteration:
    W: 1.192940
    b: 0.008223
    cost: 36891.671875
    After 239 iteration:
    W: 1.202160
    b: 0.008261
    cost: 36357.160156
    After 240 iteration:
    W: 1.211350
    b: 0.008300
    cost: 35822.214844
    After 241 iteration:
    W: 1.220511
    b: 0.008338
    cost: 35287.050781
    After 242 iteration:
    W: 1.229641
    b: 0.008375
    cost: 34751.875000
    After 243 iteration:
    W: 1.238738
    b: 0.008413
    cost: 34216.937500
    After 244 iteration:
    W: 1.247802
    b: 0.008450
    cost: 33682.453125
    After 245 iteration:
    W: 1.256832
    b: 0.008487
    cost: 33148.644531
    After 246 iteration:
    W: 1.265826
    b: 0.008523
    cost: 32615.710938
    After 247 iteration:
    W: 1.274784
    b: 0.008560
    cost: 32083.884766
    After 248 iteration:
    W: 1.283705
    b: 0.008595
    cost: 31553.361328
    After 249 iteration:
    W: 1.292586
    b: 0.008631
    cost: 31024.369141
    After 250 iteration:
    W: 1.301429
    b: 0.008667
    cost: 30497.099609
    After 251 iteration:
    W: 1.310230
    b: 0.008702
    cost: 29971.748047
    After 252 iteration:
    W: 1.318990
    b: 0.008736
    cost: 29448.509766
    After 253 iteration:
    W: 1.327708
    b: 0.008771
    cost: 28927.576172
    After 254 iteration:
    W: 1.336382
    b: 0.008805
    cost: 28409.119141
    After 255 iteration:
    W: 1.345012
    b: 0.008839
    cost: 27893.341797
    After 256 iteration:
    W: 1.353597
    b: 0.008872
    cost: 27380.421875
    After 257 iteration:
    W: 1.362135
    b: 0.008906
    cost: 26870.531250
    After 258 iteration:
    W: 1.370627
    b: 0.008938
    cost: 26363.826172
    After 259 iteration:
    W: 1.379070
    b: 0.008971
    cost: 25860.474609
    After 260 iteration:
    W: 1.387464
    b: 0.009003
    cost: 25360.640625
    After 261 iteration:
    W: 1.395809
    b: 0.009035
    cost: 24864.482422
    After 262 iteration:
    W: 1.404104
    b: 0.009067
    cost: 24372.138672
    After 263 iteration:
    W: 1.412347
    b: 0.009098
    cost: 23883.765625
    After 264 iteration:
    W: 1.420538
    b: 0.009129
    cost: 23399.494141
    After 265 iteration:
    W: 1.428676
    b: 0.009160
    cost: 22919.462891
    After 266 iteration:
    W: 1.436760
    b: 0.009190
    cost: 22443.812500
    After 267 iteration:
    W: 1.444790
    b: 0.009221
    cost: 21972.660156
    After 268 iteration:
    W: 1.452765
    b: 0.009250
    cost: 21506.123047
    After 269 iteration:
    W: 1.460684
    b: 0.009280
    cost: 21044.324219
    After 270 iteration:
    W: 1.468547
    b: 0.009309
    cost: 20587.363281
    After 271 iteration:
    W: 1.476353
    b: 0.009338
    cost: 20135.347656
    After 272 iteration:
    W: 1.484100
    b: 0.009366
    cost: 19688.384766
    After 273 iteration:
    W: 1.491790
    b: 0.009394
    cost: 19246.566406
    After 274 iteration:
    W: 1.499420
    b: 0.009422
    cost: 18809.970703
    After 275 iteration:
    W: 1.506991
    b: 0.009450
    cost: 18378.697266
    After 276 iteration:
    W: 1.514501
    b: 0.009477
    cost: 17952.810547
    After 277 iteration:
    W: 1.521951
    b: 0.009504
    cost: 17532.408203
    After 278 iteration:
    W: 1.529340
    b: 0.009530
    cost: 17117.531250
    After 279 iteration:
    W: 1.536667
    b: 0.009557
    cost: 16708.259766
    After 280 iteration:
    W: 1.543931
    b: 0.009583
    cost: 16304.651367
    After 281 iteration:
    W: 1.551133
    b: 0.009608
    cost: 15906.760742
    After 282 iteration:
    W: 1.558272
    b: 0.009634
    cost: 15514.630859
    After 283 iteration:
    W: 1.565347
    b: 0.009659
    cost: 15128.332031
    After 284 iteration:
    W: 1.572358
    b: 0.009683
    cost: 14747.867188
    After 285 iteration:
    W: 1.579304
    b: 0.009708
    cost: 14373.293945
    After 286 iteration:
    W: 1.586186
    b: 0.009732
    cost: 14004.628906
    After 287 iteration:
    W: 1.593002
    b: 0.009755
    cost: 13641.916016
    After 288 iteration:
    W: 1.599753
    b: 0.009779
    cost: 13285.165039
    After 289 iteration:
    W: 1.606439
    b: 0.009802
    cost: 12934.390625
    After 290 iteration:
    W: 1.613058
    b: 0.009825
    cost: 12589.616211
    After 291 iteration:
    W: 1.619610
    b: 0.009847
    cost: 12250.837891
    After 292 iteration:
    W: 1.626097
    b: 0.009870
    cost: 11918.072266
    After 293 iteration:
    W: 1.632516
    b: 0.009891
    cost: 11591.313477
    After 294 iteration:
    W: 1.638868
    b: 0.009913
    cost: 11270.555664
    After 295 iteration:
    W: 1.645153
    b: 0.009934
    cost: 10955.786133
    After 296 iteration:
    W: 1.651371
    b: 0.009955
    cost: 10647.017578
    After 297 iteration:
    W: 1.657520
    b: 0.009976
    cost: 10344.202148
    After 298 iteration:
    W: 1.663603
    b: 0.009996
    cost: 10047.339844
    After 299 iteration:
    W: 1.669617
    b: 0.010017
    cost: 9756.415039
    After 300 iteration:
    W: 1.675563
    b: 0.010036
    cost: 9471.385742
    After 301 iteration:
    W: 1.681441
    b: 0.010056
    cost: 9192.226562
    After 302 iteration:
    W: 1.687251
    b: 0.010075
    cost: 8918.914062
    After 303 iteration:
    W: 1.692994
    b: 0.010094
    cost: 8651.395508
    After 304 iteration:
    W: 1.698667
    b: 0.010113
    cost: 8389.638672
    After 305 iteration:
    W: 1.704273
    b: 0.010131
    cost: 8133.620117
    After 306 iteration:
    W: 1.709811
    b: 0.010149
    cost: 7883.280762
    After 307 iteration:
    W: 1.715280
    b: 0.010167
    cost: 7638.550781
    After 308 iteration:
    W: 1.720681
    b: 0.010185
    cost: 7399.433105
    After 309 iteration:
    W: 1.726015
    b: 0.010202
    cost: 7165.843750
    After 310 iteration:
    W: 1.731280
    b: 0.010219
    cost: 6937.729004
    After 311 iteration:
    W: 1.736477
    b: 0.010236
    cost: 6715.024414
    After 312 iteration:
    W: 1.741607
    b: 0.010252
    cost: 6497.694336
    After 313 iteration:
    W: 1.746670
    b: 0.010268
    cost: 6285.661621
    After 314 iteration:
    W: 1.751664
    b: 0.010284
    cost: 6078.869141
    After 315 iteration:
    W: 1.756592
    b: 0.010300
    cost: 5877.247559
    After 316 iteration:
    W: 1.761452
    b: 0.010315
    cost: 5680.746582
    After 317 iteration:
    W: 1.766246
    b: 0.010330
    cost: 5489.277832
    After 318 iteration:
    W: 1.770973
    b: 0.010345
    cost: 5302.789062
    After 319 iteration:
    W: 1.775634
    b: 0.010360
    cost: 5121.191406
    After 320 iteration:
    W: 1.780228
    b: 0.010374
    cost: 4944.439941
    After 321 iteration:
    W: 1.784756
    b: 0.010388
    cost: 4772.436523
    After 322 iteration:
    W: 1.789219
    b: 0.010402
    cost: 4605.126953
    After 323 iteration:
    W: 1.793617
    b: 0.010416
    cost: 4442.413086
    After 324 iteration:
    W: 1.797949
    b: 0.010429
    cost: 4284.239258
    After 325 iteration:
    W: 1.802217
    b: 0.010442
    cost: 4130.525391
    After 326 iteration:
    W: 1.806420
    b: 0.010455
    cost: 3981.190918
    After 327 iteration:
    W: 1.810559
    b: 0.010468
    cost: 3836.155518
    After 328 iteration:
    W: 1.814635
    b: 0.010480
    cost: 3695.347168
    After 329 iteration:
    W: 1.818647
    b: 0.010492
    cost: 3558.686035
    After 330 iteration:
    W: 1.822596
    b: 0.010504
    cost: 3426.100098
    After 331 iteration:
    W: 1.826482
    b: 0.010516
    cost: 3297.496094
    After 332 iteration:
    W: 1.830307
    b: 0.010528
    cost: 3172.802734
    After 333 iteration:
    W: 1.834070
    b: 0.010539
    cost: 3051.928467
    After 334 iteration:
    W: 1.837771
    b: 0.010550
    cost: 2934.810303
    After 335 iteration:
    W: 1.841412
    b: 0.010561
    cost: 2821.370850
    After 336 iteration:
    W: 1.844992
    b: 0.010571
    cost: 2711.514404
    After 337 iteration:
    W: 1.848512
    b: 0.010582
    cost: 2605.178955
    After 338 iteration:
    W: 1.851972
    b: 0.010592
    cost: 2502.270996
    After 339 iteration:
    W: 1.855374
    b: 0.010602
    cost: 2402.722900
    After 340 iteration:
    W: 1.858717
    b: 0.010612
    cost: 2306.461670
    After 341 iteration:
    W: 1.862002
    b: 0.010622
    cost: 2213.396973
    After 342 iteration:
    W: 1.865229
    b: 0.010631
    cost: 2123.452393
    After 343 iteration:
    W: 1.868400
    b: 0.010640
    cost: 2036.563110
    After 344 iteration:
    W: 1.871514
    b: 0.010649
    cost: 1952.643066
    After 345 iteration:
    W: 1.874571
    b: 0.010658
    cost: 1871.613281
    After 346 iteration:
    W: 1.877574
    b: 0.010667
    cost: 1793.416992
    After 347 iteration:
    W: 1.880521
    b: 0.010675
    cost: 1717.971924
    After 348 iteration:
    W: 1.883414
    b: 0.010684
    cost: 1645.202515
    After 349 iteration:
    W: 1.886254
    b: 0.010692
    cost: 1575.039551
    After 350 iteration:
    W: 1.889040
    b: 0.010700
    cost: 1507.410034
    After 351 iteration:
    W: 1.891773
    b: 0.010708
    cost: 1442.247070
    After 352 iteration:
    W: 1.894454
    b: 0.010715
    cost: 1379.475952
    After 353 iteration:
    W: 1.897084
    b: 0.010723
    cost: 1319.041016
    After 354 iteration:
    W: 1.899663
    b: 0.010730
    cost: 1260.865479
    After 355 iteration:
    W: 1.902191
    b: 0.010737
    cost: 1204.891602
    After 356 iteration:
    W: 1.904669
    b: 0.010744
    cost: 1151.041504
    After 357 iteration:
    W: 1.907099
    b: 0.010751
    cost: 1099.260010
    After 358 iteration:
    W: 1.909479
    b: 0.010758
    cost: 1049.484741
    After 359 iteration:
    W: 1.911812
    b: 0.010764
    cost: 1001.650635
    After 360 iteration:
    W: 1.914097
    b: 0.010770
    cost: 955.701904
    After 361 iteration:
    W: 1.916335
    b: 0.010777
    cost: 911.568909
    After 362 iteration:
    W: 1.918527
    b: 0.010783
    cost: 869.209595
    After 363 iteration:
    W: 1.920673
    b: 0.010789
    cost: 828.560608
    After 364 iteration:
    W: 1.922775
    b: 0.010794
    cost: 789.564026
    After 365 iteration:
    W: 1.924832
    b: 0.010800
    cost: 752.166809
    After 366 iteration:
    W: 1.926845
    b: 0.010805
    cost: 716.313721
    After 367 iteration:
    W: 1.928815
    b: 0.010811
    cost: 681.954468
    After 368 iteration:
    W: 1.930742
    b: 0.010816
    cost: 649.036560
    After 369 iteration:
    W: 1.932627
    b: 0.010821
    cost: 617.512207
    After 370 iteration:
    W: 1.934471
    b: 0.010826
    cost: 587.331543
    After 371 iteration:
    W: 1.936274
    b: 0.010831
    cost: 558.449219
    After 372 iteration:
    W: 1.938037
    b: 0.010836
    cost: 530.812439
    After 373 iteration:
    W: 1.939760
    b: 0.010840
    cost: 504.384033
    After 374 iteration:
    W: 1.941445
    b: 0.010845
    cost: 479.116638
    After 375 iteration:
    W: 1.943091
    b: 0.010849
    cost: 454.969635
    After 376 iteration:
    W: 1.944699
    b: 0.010854
    cost: 431.900940
    After 377 iteration:
    W: 1.946270
    b: 0.010858
    cost: 409.869720
    After 378 iteration:
    W: 1.947805
    b: 0.010862
    cost: 388.833923
    After 379 iteration:
    W: 1.949304
    b: 0.010866
    cost: 368.757660
    After 380 iteration:
    W: 1.950767
    b: 0.010870
    cost: 349.603729
    After 381 iteration:
    W: 1.952195
    b: 0.010873
    cost: 331.338196
    After 382 iteration:
    W: 1.953590
    b: 0.010877
    cost: 313.921661
    After 383 iteration:
    W: 1.954951
    b: 0.010880
    cost: 297.322510
    After 384 iteration:
    W: 1.956278
    b: 0.010884
    cost: 281.509979
    After 385 iteration:
    W: 1.957574
    b: 0.010887
    cost: 266.450226
    After 386 iteration:
    W: 1.958837
    b: 0.010891
    cost: 252.112534
    After 387 iteration:
    W: 1.960069
    b: 0.010894
    cost: 238.467194
    After 388 iteration:
    W: 1.961271
    b: 0.010897
    cost: 225.483658
    After 389 iteration:
    W: 1.962442
    b: 0.010900
    cost: 213.137894
    After 390 iteration:
    W: 1.963583
    b: 0.010903
    cost: 201.399475
    After 391 iteration:
    W: 1.964696
    b: 0.010906
    cost: 190.244308
    After 392 iteration:
    W: 1.965780
    b: 0.010908
    cost: 179.647354
    After 393 iteration:
    W: 1.966836
    b: 0.010911
    cost: 169.582901
    After 394 iteration:
    W: 1.967865
    b: 0.010914
    cost: 160.027756
    After 395 iteration:
    W: 1.968867
    b: 0.010916
    cost: 150.959564
    After 396 iteration:
    W: 1.969843
    b: 0.010919
    cost: 142.358215
    After 397 iteration:
    W: 1.970792
    b: 0.010921
    cost: 134.200012
    After 398 iteration:
    W: 1.971717
    b: 0.010923
    cost: 126.466385
    After 399 iteration:
    W: 1.972617
    b: 0.010926
    cost: 119.139412
    After 400 iteration:
    W: 1.973492
    b: 0.010928
    cost: 112.196503
    After 401 iteration:
    W: 1.974344
    b: 0.010930
    cost: 105.623795
    After 402 iteration:
    W: 1.975172
    b: 0.010932
    cost: 99.400314
    After 403 iteration:
    W: 1.975978
    b: 0.010934
    cost: 93.510933
    After 404 iteration:
    W: 1.976761
    b: 0.010936
    cost: 87.940987
    After 405 iteration:
    W: 1.977522
    b: 0.010938
    cost: 82.674088
    After 406 iteration:
    W: 1.978263
    b: 0.010940
    cost: 77.694580
    After 407 iteration:
    W: 1.978982
    b: 0.010941
    cost: 72.990662
    After 408 iteration:
    W: 1.979681
    b: 0.010943
    cost: 68.547768
    After 409 iteration:
    W: 1.980360
    b: 0.010945
    cost: 64.353027
    After 410 iteration:
    W: 1.981019
    b: 0.010946
    cost: 60.393124
    After 411 iteration:
    W: 1.981659
    b: 0.010948
    cost: 56.656311
    After 412 iteration:
    W: 1.982281
    b: 0.010950
    cost: 53.133102
    After 413 iteration:
    W: 1.982885
    b: 0.010951
    cost: 49.810822
    After 414 iteration:
    W: 1.983471
    b: 0.010952
    cost: 46.679989
    After 415 iteration:
    W: 1.984039
    b: 0.010954
    cost: 43.729839
    After 416 iteration:
    W: 1.984591
    b: 0.010955
    cost: 40.952499
    After 417 iteration:
    W: 1.985126
    b: 0.010956
    cost: 38.336536
    After 418 iteration:
    W: 1.985644
    b: 0.010958
    cost: 35.875595
    After 419 iteration:
    W: 1.986148
    b: 0.010959
    cost: 33.560608
    After 420 iteration:
    W: 1.986635
    b: 0.010960
    cost: 31.383657
    After 421 iteration:
    W: 1.987108
    b: 0.010961
    cost: 29.337852
    After 422 iteration:
    W: 1.987566
    b: 0.010962
    cost: 27.415297
    After 423 iteration:
    W: 1.988011
    b: 0.010963
    cost: 25.609135
    After 424 iteration:
    W: 1.988441
    b: 0.010964
    cost: 23.914034
    After 425 iteration:
    W: 1.988857
    b: 0.010965
    cost: 22.322510
    After 426 iteration:
    W: 1.989261
    b: 0.010966
    cost: 20.829218
    After 427 iteration:
    W: 1.989651
    b: 0.010967
    cost: 19.429026
    After 428 iteration:
    W: 1.990030
    b: 0.010968
    cost: 18.115976
    After 429 iteration:
    W: 1.990396
    b: 0.010969
    cost: 16.885458
    After 430 iteration:
    W: 1.990750
    b: 0.010970
    cost: 15.733541
    After 431 iteration:
    W: 1.991093
    b: 0.010970
    cost: 14.654074
    After 432 iteration:
    W: 1.991424
    b: 0.010971
    cost: 13.644059
    After 433 iteration:
    W: 1.991745
    b: 0.010972
    cost: 12.698365
    After 434 iteration:
    W: 1.992055
    b: 0.010973
    cost: 11.814308
    After 435 iteration:
    W: 1.992355
    b: 0.010973
    cost: 10.987622
    After 436 iteration:
    W: 1.992644
    b: 0.010974
    cost: 10.215094
    After 437 iteration:
    W: 1.992924
    b: 0.010975
    cost: 9.492898
    After 438 iteration:
    W: 1.993195
    b: 0.010975
    cost: 8.818913
    After 439 iteration:
    W: 1.993456
    b: 0.010976
    cost: 8.189644
    After 440 iteration:
    W: 1.993708
    b: 0.010976
    cost: 7.602500
    After 441 iteration:
    W: 1.993952
    b: 0.010977
    cost: 7.054691
    After 442 iteration:
    W: 1.994188
    b: 0.010978
    cost: 6.543903
    After 443 iteration:
    W: 1.994415
    b: 0.010978
    cost: 6.067634
    After 444 iteration:
    W: 1.994634
    b: 0.010979
    cost: 5.624112
    After 445 iteration:
    W: 1.994846
    b: 0.010979
    cost: 5.210787
    After 446 iteration:
    W: 1.995050
    b: 0.010979
    cost: 4.826099
    After 447 iteration:
    W: 1.995247
    b: 0.010980
    cost: 4.468280
    After 448 iteration:
    W: 1.995436
    b: 0.010980
    cost: 4.135404
    After 449 iteration:
    W: 1.995619
    b: 0.010981
    cost: 3.825672
    After 450 iteration:
    W: 1.995796
    b: 0.010981
    cost: 3.537862
    After 451 iteration:
    W: 1.995966
    b: 0.010982
    cost: 3.270570
    After 452 iteration:
    W: 1.996130
    b: 0.010982
    cost: 3.022258
    After 453 iteration:
    W: 1.996287
    b: 0.010982
    cost: 2.791698
    After 454 iteration:
    W: 1.996439
    b: 0.010983
    cost: 2.577726
    After 455 iteration:
    W: 1.996586
    b: 0.010983
    cost: 2.379246
    After 456 iteration:
    W: 1.996727
    b: 0.010983
    cost: 2.195223
    After 457 iteration:
    W: 1.996862
    b: 0.010983
    cost: 2.024680
    After 458 iteration:
    W: 1.996993
    b: 0.010984
    cost: 1.866532
    After 459 iteration:
    W: 1.997119
    b: 0.010984
    cost: 1.720094
    After 460 iteration:
    W: 1.997240
    b: 0.010984
    cost: 1.584550
    After 461 iteration:
    W: 1.997356
    b: 0.010985
    cost: 1.459135
    After 462 iteration:
    W: 1.997468
    b: 0.010985
    cost: 1.343132
    After 463 iteration:
    W: 1.997575
    b: 0.010985
    cost: 1.235729
    After 464 iteration:
    W: 1.997679
    b: 0.010985
    cost: 1.136572
    After 465 iteration:
    W: 1.997778
    b: 0.010985
    cost: 1.044927
    After 466 iteration:
    W: 1.997874
    b: 0.010986
    cost: 0.960242
    After 467 iteration:
    W: 1.997965
    b: 0.010986
    cost: 0.882228
    After 468 iteration:
    W: 1.998053
    b: 0.010986
    cost: 0.810154
    After 469 iteration:
    W: 1.998138
    b: 0.010986
    cost: 0.743569
    After 470 iteration:
    W: 1.998219
    b: 0.010986
    cost: 0.682358
    After 471 iteration:
    W: 1.998297
    b: 0.010987
    cost: 0.625802
    After 472 iteration:
    W: 1.998372
    b: 0.010987
    cost: 0.573725
    After 473 iteration:
    W: 1.998444
    b: 0.010987
    cost: 0.525855
    After 474 iteration:
    W: 1.998513
    b: 0.010987
    cost: 0.481679
    After 475 iteration:
    W: 1.998579
    b: 0.010987
    cost: 0.441060
    After 476 iteration:
    W: 1.998642
    b: 0.010987
    cost: 0.403703
    After 477 iteration:
    W: 1.998703
    b: 0.010987
    cost: 0.369332
    After 478 iteration:
    W: 1.998761
    b: 0.010988
    cost: 0.337837
    After 479 iteration:
    W: 1.998817
    b: 0.010988
    cost: 0.308830
    After 480 iteration:
    W: 1.998870
    b: 0.010988
    cost: 0.282227
    After 481 iteration:
    W: 1.998922
    b: 0.010988
    cost: 0.257750
    After 482 iteration:
    W: 1.998971
    b: 0.010988
    cost: 0.235388
    After 483 iteration:
    W: 1.999018
    b: 0.010988
    cost: 0.214833
    After 484 iteration:
    W: 1.999063
    b: 0.010988
    cost: 0.196027
    After 485 iteration:
    W: 1.999106
    b: 0.010988
    cost: 0.178752
    After 486 iteration:
    W: 1.999147
    b: 0.010988
    cost: 0.163012
    After 487 iteration:
    W: 1.999186
    b: 0.010988
    cost: 0.148515
    After 488 iteration:
    W: 1.999224
    b: 0.010989
    cost: 0.135275
    After 489 iteration:
    W: 1.999260
    b: 0.010989
    cost: 0.123167
    After 490 iteration:
    W: 1.999294
    b: 0.010989
    cost: 0.112117
    After 491 iteration:
    W: 1.999327
    b: 0.010989
    cost: 0.102014
    After 492 iteration:
    W: 1.999359
    b: 0.010989
    cost: 0.092760
    After 493 iteration:
    W: 1.999389
    b: 0.010989
    cost: 0.084335
    After 494 iteration:
    W: 1.999417
    b: 0.010989
    cost: 0.076615
    After 495 iteration:
    W: 1.999445
    b: 0.010989
    cost: 0.069587
    After 496 iteration:
    W: 1.999471
    b: 0.010989
    cost: 0.063173
    After 497 iteration:
    W: 1.999496
    b: 0.010989
    cost: 0.057332
    After 498 iteration:
    W: 1.999520
    b: 0.010989
    cost: 0.052024
    After 499 iteration:
    W: 1.999543
    b: 0.010989
    cost: 0.047186
    After 500 iteration:
    W: 1.999565
    b: 0.010989
    cost: 0.042736
    After 501 iteration:
    W: 1.999585
    b: 0.010989
    cost: 0.038721
    After 502 iteration:
    W: 1.999605
    b: 0.010989
    cost: 0.035065
    After 503 iteration:
    W: 1.999624
    b: 0.010989
    cost: 0.031763
    After 504 iteration:
    W: 1.999642
    b: 0.010989
    cost: 0.028728
    After 505 iteration:
    W: 1.999659
    b: 0.010989
    cost: 0.025983
    After 506 iteration:
    W: 1.999675
    b: 0.010989
    cost: 0.023488
    After 507 iteration:
    W: 1.999691
    b: 0.010989
    cost: 0.021226
    After 508 iteration:
    W: 1.999706
    b: 0.010989
    cost: 0.019179
    After 509 iteration:
    W: 1.999720
    b: 0.010990
    cost: 0.017300
    After 510 iteration:
    W: 1.999733
    b: 0.010990
    cost: 0.015610
    After 511 iteration:
    W: 1.999746
    b: 0.010990
    cost: 0.014093
    After 512 iteration:
    W: 1.999758
    b: 0.010990
    cost: 0.012708
    After 513 iteration:
    W: 1.999770
    b: 0.010990
    cost: 0.011461
    After 514 iteration:
    W: 1.999781
    b: 0.010990
    cost: 0.010315
    After 515 iteration:
    W: 1.999792
    b: 0.010990
    cost: 0.009300
    After 516 iteration:
    W: 1.999802
    b: 0.010990
    cost: 0.008360
    After 517 iteration:
    W: 1.999811
    b: 0.010990
    cost: 0.007512
    After 518 iteration:
    W: 1.999820
    b: 0.010990
    cost: 0.006749
    After 519 iteration:
    W: 1.999829
    b: 0.010990
    cost: 0.006065
    After 520 iteration:
    W: 1.999837
    b: 0.010990
    cost: 0.005454
    After 521 iteration:
    W: 1.999845
    b: 0.010990
    cost: 0.004892
    After 522 iteration:
    W: 1.999852
    b: 0.010990
    cost: 0.004410
    After 523 iteration:
    W: 1.999859
    b: 0.010990
    cost: 0.003952
    After 524 iteration:
    W: 1.999865
    b: 0.010990
    cost: 0.003549
    After 525 iteration:
    W: 1.999872
    b: 0.010990
    cost: 0.003181
    After 526 iteration:
    W: 1.999878
    b: 0.010990
    cost: 0.002859
    After 527 iteration:
    W: 1.999883
    b: 0.010990
    cost: 0.002554
    After 528 iteration:
    W: 1.999889
    b: 0.010990
    cost: 0.002290
    After 529 iteration:
    W: 1.999894
    b: 0.010990
    cost: 0.002051
    After 530 iteration:
    W: 1.999898
    b: 0.010990
    cost: 0.001836
    After 531 iteration:
    W: 1.999903
    b: 0.010990
    cost: 0.001642
    After 532 iteration:
    W: 1.999907
    b: 0.010990
    cost: 0.001469
    After 533 iteration:
    W: 1.999911
    b: 0.010990
    cost: 0.001314
    After 534 iteration:
    W: 1.999915
    b: 0.010990
    cost: 0.001177
    After 535 iteration:
    W: 1.999919
    b: 0.010990
    cost: 0.001046
    After 536 iteration:
    W: 1.999922
    b: 0.010990
    cost: 0.000939
    After 537 iteration:
    W: 1.999926
    b: 0.010990
    cost: 0.000837
    After 538 iteration:
    W: 1.999929
    b: 0.010990
    cost: 0.000748
    After 539 iteration:
    W: 1.999932
    b: 0.010990
    cost: 0.000663
    After 540 iteration:
    W: 1.999935
    b: 0.010990
    cost: 0.000596
    After 541 iteration:
    W: 1.999937
    b: 0.010990
    cost: 0.000527
    After 542 iteration:
    W: 1.999940
    b: 0.010990
    cost: 0.000472
    After 543 iteration:
    W: 1.999942
    b: 0.010990
    cost: 0.000421
    After 544 iteration:
    W: 1.999944
    b: 0.010990
    cost: 0.000372
    After 545 iteration:
    W: 1.999946
    b: 0.010990
    cost: 0.000331
    After 546 iteration:
    W: 1.999949
    b: 0.010990
    cost: 0.000292
    After 547 iteration:
    W: 1.999950
    b: 0.010990
    cost: 0.000260
    After 548 iteration:
    W: 1.999952
    b: 0.010990
    cost: 0.000233
    After 549 iteration:
    W: 1.999954
    b: 0.010990
    cost: 0.000204
    After 550 iteration:
    W: 1.999955
    b: 0.010990
    cost: 0.000184
    After 551 iteration:
    W: 1.999957
    b: 0.010990
    cost: 0.000161
    After 552 iteration:
    W: 1.999958
    b: 0.010990
    cost: 0.000143
    After 553 iteration:
    W: 1.999960
    b: 0.010990
    cost: 0.000129
    After 554 iteration:
    W: 1.999961
    b: 0.010990
    cost: 0.000113
    After 555 iteration:
    W: 1.999962
    b: 0.010990
    cost: 0.000100
    After 556 iteration:
    W: 1.999963
    b: 0.010990
    cost: 0.000088
    After 557 iteration:
    W: 1.999964
    b: 0.010990
    cost: 0.000079
    After 558 iteration:
    W: 1.999965
    b: 0.010990
    cost: 0.000071
    After 559 iteration:
    W: 1.999966
    b: 0.010990
    cost: 0.000061
    After 560 iteration:
    W: 1.999967
    b: 0.010990
    cost: 0.000055
    After 561 iteration:
    W: 1.999968
    b: 0.010990
    cost: 0.000048
    After 562 iteration:
    W: 1.999969
    b: 0.010990
    cost: 0.000043
    After 563 iteration:
    W: 1.999969
    b: 0.010990
    cost: 0.000039
    After 564 iteration:
    W: 1.999970
    b: 0.010990
    cost: 0.000034
    After 565 iteration:
    W: 1.999971
    b: 0.010990
    cost: 0.000030
    After 566 iteration:
    W: 1.999972
    b: 0.010990
    cost: 0.000026
    After 567 iteration:
    W: 1.999972
    b: 0.010990
    cost: 0.000024
    After 568 iteration:
    W: 1.999973
    b: 0.010990
    cost: 0.000020
    After 569 iteration:
    W: 1.999973
    b: 0.010990
    cost: 0.000018
    After 570 iteration:
    W: 1.999974
    b: 0.010990
    cost: 0.000016
    After 571 iteration:
    W: 1.999974
    b: 0.010990
    cost: 0.000014
    After 572 iteration:
    W: 1.999975
    b: 0.010990
    cost: 0.000013
    After 573 iteration:
    W: 1.999975
    b: 0.010990
    cost: 0.000012
    After 574 iteration:
    W: 1.999975
    b: 0.010990
    cost: 0.000010
    After 575 iteration:
    W: 1.999976
    b: 0.010990
    cost: 0.000009
    After 576 iteration:
    W: 1.999976
    b: 0.010990
    cost: 0.000008
    After 577 iteration:
    W: 1.999976
    b: 0.010990
    cost: 0.000007
    After 578 iteration:
    W: 1.999977
    b: 0.010990
    cost: 0.000006
    After 579 iteration:
    W: 1.999977
    b: 0.010990
    cost: 0.000005
    After 580 iteration:
    W: 1.999977
    b: 0.010990
    cost: 0.000005
    After 581 iteration:
    W: 1.999977
    b: 0.010990
    cost: 0.000004
    After 582 iteration:
    W: 1.999978
    b: 0.010990
    cost: 0.000004
    After 583 iteration:
    W: 1.999978
    b: 0.010990
    cost: 0.000003
    After 584 iteration:
    W: 1.999978
    b: 0.010990
    cost: 0.000003
    After 585 iteration:
    W: 1.999978
    b: 0.010990
    cost: 0.000003
    After 586 iteration:
    W: 1.999979
    b: 0.010990
    cost: 0.000002
    After 587 iteration:
    W: 1.999979
    b: 0.010990
    cost: 0.000002
    After 588 iteration:
    W: 1.999979
    b: 0.010990
    cost: 0.000002
    After 589 iteration:
    W: 1.999979
    b: 0.010990
    cost: 0.000002
    After 590 iteration:
    W: 1.999979
    b: 0.010990
    cost: 0.000001
    After 591 iteration:
    W: 1.999979
    b: 0.010990
    cost: 0.000001
    After 592 iteration:
    W: 1.999979
    b: 0.010990
    cost: 0.000001
    After 593 iteration:
    W: 1.999980
    b: 0.010990
    cost: 0.000001
    After 594 iteration:
    W: 1.999980
    b: 0.010990
    cost: 0.000001
    After 595 iteration:
    W: 1.999980
    b: 0.010990
    cost: 0.000001
    After 596 iteration:
    W: 1.999980
    b: 0.010990
    cost: 0.000001
    After 597 iteration:
    W: 1.999980
    b: 0.010990
    cost: 0.000001
    After 598 iteration:
    W: 1.999980
    b: 0.010990
    cost: 0.000001
    After 599 iteration:
    W: 1.999980
    b: 0.010990
    cost: 0.000001
    After 600 iteration:
    W: 1.999980
    b: 0.010990
    cost: 0.000001
    After 601 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000001
    After 602 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 603 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 604 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 605 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 606 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 607 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 608 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 609 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 610 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 611 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 612 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 613 iteration:
    W: 1.999981
    b: 0.010990
    cost: 0.000000
    After 614 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 615 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 616 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 617 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 618 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 619 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 620 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 621 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 622 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 623 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 624 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 625 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 626 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 627 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 628 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 629 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 630 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 631 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 632 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 633 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 634 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 635 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 636 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 637 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 638 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 639 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 640 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 641 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 642 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 643 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 644 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 645 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 646 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 647 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 648 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 649 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 650 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 651 iteration:
    W: 1.999982
    b: 0.010990
    cost: 0.000000
    After 652 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 653 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 654 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 655 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 656 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 657 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 658 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 659 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 660 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 661 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 662 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 663 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 664 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 665 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 666 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 667 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 668 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 669 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 670 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 671 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 672 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 673 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 674 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 675 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 676 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 677 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 678 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 679 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 680 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 681 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 682 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 683 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 684 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 685 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 686 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 687 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 688 iteration:
    W: 1.999983
    b: 0.010990
    cost: 0.000000
    After 689 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 690 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 691 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 692 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 693 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 694 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 695 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 696 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 697 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 698 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 699 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 700 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 701 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 702 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 703 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 704 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 705 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 706 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 707 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 708 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 709 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 710 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 711 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 712 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 713 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 714 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 715 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 716 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 717 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 718 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 719 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 720 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 721 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 722 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 723 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 724 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 725 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 726 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 727 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 728 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 729 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 730 iteration:
    W: 1.999984
    b: 0.010990
    cost: 0.000000
    After 731 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 732 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 733 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 734 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 735 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 736 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 737 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 738 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 739 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 740 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 741 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 742 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 743 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 744 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 745 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 746 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 747 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 748 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 749 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 750 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 751 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 752 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 753 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 754 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 755 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 756 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 757 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 758 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 759 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 760 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 761 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 762 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 763 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 764 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 765 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 766 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 767 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 768 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 769 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 770 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 771 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 772 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 773 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 774 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 775 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 776 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 777 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 778 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 779 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 780 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 781 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 782 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 783 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 784 iteration:
    W: 1.999985
    b: 0.010990
    cost: 0.000000
    After 785 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 786 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 787 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 788 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 789 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 790 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 791 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 792 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 793 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 794 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 795 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 796 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 797 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 798 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 799 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 800 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 801 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 802 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 803 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 804 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 805 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 806 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 807 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 808 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 809 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 810 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 811 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 812 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 813 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 814 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 815 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 816 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 817 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 818 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 819 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 820 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 821 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 822 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 823 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 824 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 825 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 826 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 827 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 828 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 829 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 830 iteration:
    W: 1.999986
    b: 0.010990
    cost: 0.000000
    After 831 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 832 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 833 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 834 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 835 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 836 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 837 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 838 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 839 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 840 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 841 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 842 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 843 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 844 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 845 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 846 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 847 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 848 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 849 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 850 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 851 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 852 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 853 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 854 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 855 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 856 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 857 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 858 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 859 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 860 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 861 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 862 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 863 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 864 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 865 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 866 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 867 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 868 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 869 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 870 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 871 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 872 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 873 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 874 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 875 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 876 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 877 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 878 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 879 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 880 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 881 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 882 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 883 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 884 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 885 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 886 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 887 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 888 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 889 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 890 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 891 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 892 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 893 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 894 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 895 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 896 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 897 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 898 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 899 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 900 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 901 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 902 iteration:
    W: 1.999987
    b: 0.010990
    cost: 0.000000
    After 903 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 904 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 905 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 906 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 907 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 908 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 909 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 910 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 911 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 912 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 913 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 914 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 915 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 916 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 917 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 918 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 919 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 920 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 921 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 922 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 923 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 924 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 925 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 926 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 927 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 928 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 929 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 930 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 931 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 932 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 933 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 934 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 935 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 936 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 937 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 938 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 939 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 940 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 941 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 942 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 943 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 944 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 945 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 946 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 947 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 948 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 949 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 950 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 951 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 952 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 953 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 954 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 955 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 956 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 957 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 958 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 959 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 960 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 961 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 962 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 963 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 964 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 965 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 966 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 967 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 968 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 969 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 970 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 971 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 972 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 973 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 974 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 975 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 976 iteration:
    W: 1.999988
    b: 0.010990
    cost: 0.000000
    After 977 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 978 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 979 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 980 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 981 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 982 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 983 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 984 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 985 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 986 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 987 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 988 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 989 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 990 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 991 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 992 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 993 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 994 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 995 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 996 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 997 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 998 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000
    After 999 iteration:
    W: 1.999989
    b: 0.010990
    cost: 0.000000

-Based on https://github.com/nethsix
