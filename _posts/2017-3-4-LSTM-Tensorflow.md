Deep Learning (in Tensorflow)
=============

Assignment 6
------------

After training a skip-gram model in `5_word2vec.ipynb`, the goal of this notebook is to train a LSTM character model over [Text8](http://mattmahoney.net/dc/textdata) data.


```python
# These are all the modules we'll be using later. Make sure you can import them
# before proceeding further.
from __future__ import print_function
import os
import numpy as np
import random
import string
import tensorflow as tf
import zipfile
from six.moves import range
from six.moves.urllib.request import urlretrieve
```


```python
url = 'http://mattmahoney.net/dc/'

def maybe_download(filename, expected_bytes):
  """Download a file if not present, and make sure it's the right size."""
  if not os.path.exists(filename):
    filename, _ = urlretrieve(url + filename, filename)
  statinfo = os.stat(filename)
  if statinfo.st_size == expected_bytes:
    print('Found and verified %s' % filename)
  else:
    print(statinfo.st_size)
    raise Exception(
      'Failed to verify ' + filename + '. Can you get to it with a browser?')
  return filename

filename = maybe_download('text8.zip', 31344016)
```

    Found and verified text8.zip



```python
def read_data(filename):
  with zipfile.ZipFile(filename) as f:
    name = f.namelist()[0]
    data = tf.compat.as_str(f.read(name))
  return data
  
text = read_data(filename)
print('Data size %d' % len(text))
```

    Data size 100000000


Create a small validation set.


```python
valid_size = 1000
valid_text = text[:valid_size]
train_text = text[valid_size:]
train_size = len(train_text)
print(train_size, train_text[:64])
print(valid_size, valid_text[:64])
```

    99999000 ons anarchists advocate social relations based upon voluntary as
    1000  anarchism originated as a term of abuse first used against earl


Utility functions to map characters to vocabulary IDs and back.


```python
vocabulary_size = len(string.ascii_lowercase) + 1 # [a-z] + ' '
first_letter = ord(string.ascii_lowercase[0])

def char2id(char):
  if char in string.ascii_lowercase:
    return ord(char) - first_letter + 1
  elif char == ' ':
    return 0
  else:
    print('Unexpected character: %s' % char)
    return 0
  
def id2char(dictid):
  if dictid > 0:
    return chr(dictid + first_letter - 1)
  else:
    return ' '

print(char2id('a'), char2id('z'), char2id(' '), char2id('ï'))
print(id2char(1), id2char(26), id2char(0))
```

    Unexpected character: ï
    1 26 0 0
    a z  


Function to generate a training batch for the LSTM model.


```python
batch_size=64
num_unrollings=10

class BatchGenerator(object):
  def __init__(self, text, batch_size, num_unrollings):
    self._text = text
    self._text_size = len(text)
    self._batch_size = batch_size
    self._num_unrollings = num_unrollings
    segment = self._text_size // batch_size
    self._cursor = [ offset * segment for offset in range(batch_size)]
    self._last_batch = self._next_batch()
  
  def _next_batch(self):
    """Generate a single batch from the current cursor position in the data."""
    batch = np.zeros(shape=(self._batch_size, vocabulary_size), dtype=np.float)
    for b in range(self._batch_size):
      batch[b, char2id(self._text[self._cursor[b]])] = 1.0
      self._cursor[b] = (self._cursor[b] + 1) % self._text_size
    return batch
  
  def next(self):
    """Generate the next array of batches from the data. The array consists of
    the last batch of the previous array, followed by num_unrollings new ones.
    """
    batches = [self._last_batch]
    for step in range(self._num_unrollings):
      batches.append(self._next_batch())
    self._last_batch = batches[-1]
    return batches

def characters(probabilities):
  """Turn a 1-hot encoding or a probability distribution over the possible
  characters back into its (most likely) character representation."""
  return [id2char(c) for c in np.argmax(probabilities, 1)]

def batches2string(batches):
  """Convert a sequence of batches back into their (most likely) string
  representation."""
  s = [''] * batches[0].shape[0]
  for b in batches:
    s = [''.join(x) for x in zip(s, characters(b))]
  return s

train_batches = BatchGenerator(train_text, batch_size, num_unrollings)
valid_batches = BatchGenerator(valid_text, 1, 1)

print(batches2string(train_batches.next()))
print(batches2string(train_batches.next()))
print(batches2string(valid_batches.next()))
print(batches2string(valid_batches.next()))
```

    ['ons anarchi', 'when milita', 'lleria arch', ' abbeys and', 'married urr', 'hel and ric', 'y and litur', 'ay opened f', 'tion from t', 'migration t', 'new york ot', 'he boeing s', 'e listed wi', 'eber has pr', 'o be made t', 'yer who rec', 'ore signifi', 'a fierce cr', ' two six ei', 'aristotle s', 'ity can be ', ' and intrac', 'tion of the', 'dy to pass ', 'f certain d', 'at it will ', 'e convince ', 'ent told hi', 'ampaign and', 'rver side s', 'ious texts ', 'o capitaliz', 'a duplicate', 'gh ann es d', 'ine january', 'ross zero t', 'cal theorie', 'ast instanc', ' dimensiona', 'most holy m', 't s support', 'u is still ', 'e oscillati', 'o eight sub', 'of italy la', 's the tower', 'klahoma pre', 'erprise lin', 'ws becomes ', 'et in a naz', 'the fabian ', 'etchy to re', ' sharman ne', 'ised empero', 'ting in pol', 'd neo latin', 'th risky ri', 'encyclopedi', 'fense the a', 'duating fro', 'treet grid ', 'ations more', 'appeal of d', 'si have mad']
    ['ists advoca', 'ary governm', 'hes nationa', 'd monasteri', 'raca prince', 'chard baer ', 'rgical lang', 'for passeng', 'the nationa', 'took place ', 'ther well k', 'seven six s', 'ith a gloss', 'robably bee', 'to recogniz', 'ceived the ', 'icant than ', 'ritic of th', 'ight in sig', 's uncaused ', ' lost as in', 'cellular ic', 'e size of t', ' him a stic', 'drugs confu', ' take to co', ' the priest', 'im to name ', 'd barred at', 'standard fo', ' such as es', 'ze on the g', 'e of the or', 'd hiver one', 'y eight mar', 'the lead ch', 'es classica', 'ce the non ', 'al analysis', 'mormons bel', 't or at lea', ' disagreed ', 'ing system ', 'btypes base', 'anguages th', 'r commissio', 'ess one nin', 'nux suse li', ' the first ', 'zi concentr', ' society ne', 'elatively s', 'etworks sha', 'or hirohito', 'litical ini', 'n most of t', 'iskerdoo ri', 'ic overview', 'air compone', 'om acnm acc', ' centerline', 'e than any ', 'devotional ', 'de such dev']
    [' a']
    ['an']



```python
def logprob(predictions, labels):
  """Log-probability of the true labels in a predicted batch."""
  predictions[predictions < 1e-10] = 1e-10
  return np.sum(np.multiply(labels, -np.log(predictions))) / labels.shape[0]

def sample_distribution(distribution):
  """Sample one element from a distribution assumed to be an array of normalized
  probabilities.
  """
  r = random.uniform(0, 1)
  s = 0
  for i in range(len(distribution)):
    s += distribution[i]
    if s >= r:
      return i
  return len(distribution) - 1

def sample(prediction):
  """Turn a (column) prediction into 1-hot encoded samples."""
  p = np.zeros(shape=[1, vocabulary_size], dtype=np.float)
  p[0, sample_distribution(prediction[0])] = 1.0
  return p

def random_distribution():
  """Generate a random column of probabilities."""
  b = np.random.uniform(0.0, 1.0, size=[1, vocabulary_size])
  return b/np.sum(b, 1)[:,None]
```

Simple LSTM Model.


```python
num_nodes = 64

graph = tf.Graph()
with graph.as_default():
  
  # Parameters:
  # Input gate: input, previous output, and bias.
  ix = tf.Variable(tf.truncated_normal([vocabulary_size, num_nodes], -0.1, 0.1))
  im = tf.Variable(tf.truncated_normal([num_nodes, num_nodes], -0.1, 0.1))
  ib = tf.Variable(tf.zeros([1, num_nodes]))
  # Forget gate: input, previous output, and bias.
  fx = tf.Variable(tf.truncated_normal([vocabulary_size, num_nodes], -0.1, 0.1))
  fm = tf.Variable(tf.truncated_normal([num_nodes, num_nodes], -0.1, 0.1))
  fb = tf.Variable(tf.zeros([1, num_nodes]))
  # Memory cell: input, state and bias.                             
  cx = tf.Variable(tf.truncated_normal([vocabulary_size, num_nodes], -0.1, 0.1))
  cm = tf.Variable(tf.truncated_normal([num_nodes, num_nodes], -0.1, 0.1))
  cb = tf.Variable(tf.zeros([1, num_nodes]))
  # Output gate: input, previous output, and bias.
  ox = tf.Variable(tf.truncated_normal([vocabulary_size, num_nodes], -0.1, 0.1))
  om = tf.Variable(tf.truncated_normal([num_nodes, num_nodes], -0.1, 0.1))
  ob = tf.Variable(tf.zeros([1, num_nodes]))
  # Variables saving state across unrollings.
  saved_output = tf.Variable(tf.zeros([batch_size, num_nodes]), trainable=False)
  saved_state = tf.Variable(tf.zeros([batch_size, num_nodes]), trainable=False)
  # Classifier weights and biases.
  w = tf.Variable(tf.truncated_normal([num_nodes, vocabulary_size], -0.1, 0.1))
  b = tf.Variable(tf.zeros([vocabulary_size]))
  
  # Definition of the cell computation.
  def lstm_cell(i, o, state):
    """Create a LSTM cell. See e.g.: http://arxiv.org/pdf/1402.1128v1.pdf
    Note that in this formulation, we omit the various connections between the
    previous state and the gates."""
    input_gate = tf.sigmoid(tf.matmul(i, ix) + tf.matmul(o, im) + ib)
    forget_gate = tf.sigmoid(tf.matmul(i, fx) + tf.matmul(o, fm) + fb)
    update = tf.matmul(i, cx) + tf.matmul(o, cm) + cb
    state = forget_gate * state + input_gate * tf.tanh(update)
    output_gate = tf.sigmoid(tf.matmul(i, ox) + tf.matmul(o, om) + ob)
    return output_gate * tf.tanh(state), state

  # Input data.
  train_data = list()
  for _ in range(num_unrollings + 1):
    train_data.append(
      tf.placeholder(tf.float32, shape=[batch_size,vocabulary_size]))
  train_inputs = train_data[:num_unrollings]
  train_labels = train_data[1:]  # labels are inputs shifted by one time step.

  # Unrolled LSTM loop.
  outputs = list()
  output = saved_output
  state = saved_state
  for i in train_inputs:
    output, state = lstm_cell(i, output, state)
    outputs.append(output)

  # State saving across unrollings.
  with tf.control_dependencies([saved_output.assign(output),
                                saved_state.assign(state)]):
    # Classifier.
    logits = tf.nn.xw_plus_b(tf.concat(outputs, 0), w, b)
    loss = tf.reduce_mean(
      tf.nn.softmax_cross_entropy_with_logits(
        labels=tf.concat(train_labels, 0), logits=logits))

  # Optimizer.
  global_step = tf.Variable(0)
  learning_rate = tf.train.exponential_decay(
    10.0, global_step, 5000, 0.1, staircase=True)
  optimizer = tf.train.GradientDescentOptimizer(learning_rate)
  gradients, v = zip(*optimizer.compute_gradients(loss))
  gradients, _ = tf.clip_by_global_norm(gradients, 1.25)
  optimizer = optimizer.apply_gradients(
    zip(gradients, v), global_step=global_step)

  # Predictions.
  train_prediction = tf.nn.softmax(logits)
  
  # Sampling and validation eval: batch 1, no unrolling.
  sample_input = tf.placeholder(tf.float32, shape=[1, vocabulary_size])
  saved_sample_output = tf.Variable(tf.zeros([1, num_nodes]))
  saved_sample_state = tf.Variable(tf.zeros([1, num_nodes]))
  reset_sample_state = tf.group(
    saved_sample_output.assign(tf.zeros([1, num_nodes])),
    saved_sample_state.assign(tf.zeros([1, num_nodes])))
  sample_output, sample_state = lstm_cell(
    sample_input, saved_sample_output, saved_sample_state)
  with tf.control_dependencies([saved_sample_output.assign(sample_output),
                                saved_sample_state.assign(sample_state)]):
    sample_prediction = tf.nn.softmax(tf.nn.xw_plus_b(sample_output, w, b))
```


```python
num_steps = 7001
summary_frequency = 100

with tf.Session(graph=graph) as session:
  tf.global_variables_initializer().run()
  print('Initialized')
  mean_loss = 0
  for step in range(num_steps):
    batches = train_batches.next()
    feed_dict = dict()
    for i in range(num_unrollings + 1):
      feed_dict[train_data[i]] = batches[i]
    _, l, predictions, lr = session.run(
      [optimizer, loss, train_prediction, learning_rate], feed_dict=feed_dict)
    mean_loss += l
    if step % summary_frequency == 0:
      if step > 0:
        mean_loss = mean_loss / summary_frequency
      # The mean loss is an estimate of the loss over the last few batches.
      print(
        'Average loss at step %d: %f learning rate: %f' % (step, mean_loss, lr))
      mean_loss = 0
      labels = np.concatenate(list(batches)[1:])
      print('Minibatch perplexity: %.2f' % float(
        np.exp(logprob(predictions, labels))))
      if step % (summary_frequency * 10) == 0:
        # Generate some samples.
        print('=' * 80)
        for _ in range(5):
          feed = sample(random_distribution())
          sentence = characters(feed)[0]
          reset_sample_state.run()
          for _ in range(79):
            prediction = sample_prediction.eval({sample_input: feed})
            feed = sample(prediction)
            sentence += characters(feed)[0]
          print(sentence)
        print('=' * 80)
      # Measure validation set perplexity.
      reset_sample_state.run()
      valid_logprob = 0
      for _ in range(valid_size):
        b = valid_batches.next()
        predictions = sample_prediction.eval({sample_input: b[0]})
        valid_logprob = valid_logprob + logprob(predictions, b[1])
      print('Validation set perplexity: %.2f' % float(np.exp(
        valid_logprob / valid_size)))
```

    Initialized
    Average loss at step 0: 3.296313 learning rate: 10.000000
    Minibatch perplexity: 27.01
    ================================================================================
    rdtgnkhfog lvyfodt eestehpnynria rgse kk vriomwrlkpgnqkxjpnf xvakd awn hh gdw dm
    dt ie sdoer eqt o mi rtf fji  lwa nokmmwwqw rh p khkcohwhtmchdhvhye   m li y tve
    ag djiiynrybiizqrnt hcezeojx feydne np i hkpts ieorbavjjltolemzbu zquekapocede s
    hdixjr wqknwzan eiapvd  smdwonewceev desdg   hdlwrowibjts d aq slacoomcvn xnn me
    nh g rtnmdp eznaadntidslirgqrddw rlugne cpuyanvnz  niptawgxuykuyyj smzfqaibda us
    ================================================================================
    Validation set perplexity: 20.41
    Average loss at step 100: 2.587017 learning rate: 10.000000
    Minibatch perplexity: 11.22
    Validation set perplexity: 10.54
    Average loss at step 200: 2.254648 learning rate: 10.000000
    Minibatch perplexity: 8.64
    Validation set perplexity: 8.61
    Average loss at step 300: 2.114732 learning rate: 10.000000
    Minibatch perplexity: 7.60
    Validation set perplexity: 8.05
    Average loss at step 400: 2.017481 learning rate: 10.000000
    Minibatch perplexity: 7.56
    Validation set perplexity: 7.84
    Average loss at step 500: 1.946522 learning rate: 10.000000
    Minibatch perplexity: 6.51
    Validation set perplexity: 7.25
    Average loss at step 600: 1.919308 learning rate: 10.000000
    Minibatch perplexity: 6.30
    Validation set perplexity: 6.92
    Average loss at step 700: 1.866662 learning rate: 10.000000
    Minibatch perplexity: 6.70
    Validation set perplexity: 6.89
    Average loss at step 800: 1.825861 learning rate: 10.000000
    Minibatch perplexity: 6.22
    Validation set perplexity: 6.52
    Average loss at step 900: 1.833735 learning rate: 10.000000
    Minibatch perplexity: 6.86
    Validation set perplexity: 6.24
    Average loss at step 1000: 1.831174 learning rate: 10.000000
    Minibatch perplexity: 5.61
    ================================================================================
    freess gived g the sto carta the one nine exende de one fayet hoves orey one sev
    s fevalle two zero ands provond hempersia he corstol of wremas hirdet in which t
    ulled ix the couedremente dime of and fime of ceqpes nuated orde x one nine e so
    ure atsing whe senterd actres of maok nivoms afre of houghe for trapent pepevers
    zuy htnsy zerizn for rishoco of nived so to d one nine eight seven prodive compe
    ================================================================================
    Validation set perplexity: 6.02
    Average loss at step 1100: 1.777607 learning rate: 10.000000
    Minibatch perplexity: 5.44
    Validation set perplexity: 5.80
    Average loss at step 1200: 1.758193 learning rate: 10.000000
    Minibatch perplexity: 4.98
    Validation set perplexity: 5.59
    Average loss at step 1300: 1.735131 learning rate: 10.000000
    Minibatch perplexity: 5.81
    Validation set perplexity: 5.58
    Average loss at step 1400: 1.751066 learning rate: 10.000000
    Minibatch perplexity: 5.98
    Validation set perplexity: 5.51
    Average loss at step 1500: 1.741979 learning rate: 10.000000
    Minibatch perplexity: 4.82
    Validation set perplexity: 5.42
    Average loss at step 1600: 1.745576 learning rate: 10.000000
    Minibatch perplexity: 5.69
    Validation set perplexity: 5.49
    Average loss at step 1700: 1.713515 learning rate: 10.000000
    Minibatch perplexity: 5.65
    Validation set perplexity: 5.37
    Average loss at step 1800: 1.676672 learning rate: 10.000000
    Minibatch perplexity: 5.55
    Validation set perplexity: 5.28
    Average loss at step 1900: 1.648010 learning rate: 10.000000
    Minibatch perplexity: 5.09
    Validation set perplexity: 5.17
    Average loss at step 2000: 1.699197 learning rate: 10.000000
    Minibatch perplexity: 5.61
    ================================================================================
    rirs and oftilingrols pepsints manion enca leas rabliny onlora from the transing
    candy carchady engiles paxebrtic of a producer in the pontembing in impiriar of 
    ivers incident to orf states kinks to to west cinezs attriue the recordannges te
    undting was zain olds atince techniverstivive once one nine nine zero nine sanam
    burom oppiciated bitking tobear them  i hisusers popitulivation the sear and in 
    ================================================================================
    Validation set perplexity: 5.24
    Average loss at step 2100: 1.688151 learning rate: 10.000000
    Minibatch perplexity: 5.19
    Validation set perplexity: 4.99
    Average loss at step 2200: 1.679783 learning rate: 10.000000
    Minibatch perplexity: 6.35
    Validation set perplexity: 5.11
    Average loss at step 2300: 1.641449 learning rate: 10.000000
    Minibatch perplexity: 5.02
    Validation set perplexity: 4.91
    Average loss at step 2400: 1.660141 learning rate: 10.000000
    Minibatch perplexity: 5.23
    Validation set perplexity: 4.79
    Average loss at step 2500: 1.678416 learning rate: 10.000000
    Minibatch perplexity: 5.42
    Validation set perplexity: 4.73
    Average loss at step 2600: 1.658819 learning rate: 10.000000
    Minibatch perplexity: 5.61
    Validation set perplexity: 4.71
    Average loss at step 2700: 1.659661 learning rate: 10.000000
    Minibatch perplexity: 4.51
    Validation set perplexity: 4.72
    Average loss at step 2800: 1.653528 learning rate: 10.000000
    Minibatch perplexity: 5.46
    Validation set perplexity: 4.63
    Average loss at step 2900: 1.651158 learning rate: 10.000000
    Minibatch perplexity: 5.68
    Validation set perplexity: 4.74
    Average loss at step 3000: 1.650680 learning rate: 10.000000
    Minibatch perplexity: 5.00
    ================================================================================
    questally e revectories independently sp peurchwarc the altherry white spaussian
    s intiglosmafor of earcharl of these thetrear foreinogy invoution in a newwithas
    ain croucters retuon suchex researnons may intiom spated in phating earlyezan la
    ing megalts by the duscirshs codecy techning the are coulic in lands of have two
    were ganco read parcoppeat s to framm evelies sobles witlosch of maytion oned mi
    ================================================================================
    Validation set perplexity: 4.75
    Average loss at step 3100: 1.630824 learning rate: 10.000000
    Minibatch perplexity: 5.55
    Validation set perplexity: 4.66
    Average loss at step 3200: 1.646360 learning rate: 10.000000
    Minibatch perplexity: 5.62
    Validation set perplexity: 4.75
    Average loss at step 3300: 1.636565 learning rate: 10.000000
    Minibatch perplexity: 5.00
    Validation set perplexity: 4.61
    Average loss at step 3400: 1.671810 learning rate: 10.000000
    Minibatch perplexity: 5.54
    Validation set perplexity: 4.63
    Average loss at step 3500: 1.658734 learning rate: 10.000000
    Minibatch perplexity: 5.42
    Validation set perplexity: 4.77
    Average loss at step 3600: 1.666998 learning rate: 10.000000
    Minibatch perplexity: 4.58
    Validation set perplexity: 4.67
    Average loss at step 3700: 1.642992 learning rate: 10.000000
    Minibatch perplexity: 5.10
    Validation set perplexity: 4.64
    Average loss at step 3800: 1.645109 learning rate: 10.000000
    Minibatch perplexity: 5.50
    Validation set perplexity: 4.77
    Average loss at step 3900: 1.640472 learning rate: 10.000000
    Minibatch perplexity: 5.25
    Validation set perplexity: 4.65
    Average loss at step 4000: 1.657431 learning rate: 10.000000
    Minibatch perplexity: 4.64
    ================================================================================
    ine inflasuish f ti oliswang indeus bellettify palatian gengs refunce the becomi
    e jaigis imperial commidion the to as a pupprier the nexe who a right maynes rar
    t the chmodism of defined a surminquicts use the princicless allow polition todi
    jed is tricts arimear or demonth somelexd general repeally reedget toresical lly
    x with his fansh phickn alphenqlohing worrs daycheed interzary known or six e hi
    ================================================================================
    Validation set perplexity: 4.75
    Average loss at step 4100: 1.632510 learning rate: 10.000000
    Minibatch perplexity: 5.19
    Validation set perplexity: 4.87
    Average loss at step 4200: 1.635130 learning rate: 10.000000
    Minibatch perplexity: 5.29
    Validation set perplexity: 4.68
    Average loss at step 4300: 1.618518 learning rate: 10.000000
    Minibatch perplexity: 5.07
    Validation set perplexity: 4.69
    Average loss at step 4400: 1.605577 learning rate: 10.000000
    Minibatch perplexity: 4.83
    Validation set perplexity: 4.56
    Average loss at step 4500: 1.615881 learning rate: 10.000000
    Minibatch perplexity: 5.24
    Validation set perplexity: 4.80
    Average loss at step 4600: 1.616792 learning rate: 10.000000
    Minibatch perplexity: 5.04
    Validation set perplexity: 4.75
    Average loss at step 4700: 1.630954 learning rate: 10.000000
    Minibatch perplexity: 5.28
    Validation set perplexity: 4.66
    Average loss at step 4800: 1.629121 learning rate: 10.000000
    Minibatch perplexity: 4.54
    Validation set perplexity: 4.68
    Average loss at step 4900: 1.634588 learning rate: 10.000000
    Minibatch perplexity: 5.07
    Validation set perplexity: 4.77
    Average loss at step 5000: 1.606833 learning rate: 1.000000
    Minibatch perplexity: 4.47
    ================================================================================
    holds cribute apporitoust were untypent witall usions see south hozosic s karnis
    ely sep by putered rase monar see blackan early rud englisine contrimants for lo
     to studgon in one nmw nite high however by extermansal liginal outsent tames of
    gue from two in stancele governia s are states i opposed in cell klacker to sayd
    zer directude or juzay for most one six the clabl pomes day to by one nine nine 
    ================================================================================
    Validation set perplexity: 4.92
    Average loss at step 5100: 1.605532 learning rate: 1.000000
    Minibatch perplexity: 4.90
    Validation set perplexity: 4.73
    Average loss at step 5200: 1.592459 learning rate: 1.000000
    Minibatch perplexity: 4.44
    Validation set perplexity: 4.64
    Average loss at step 5300: 1.578593 learning rate: 1.000000
    Minibatch perplexity: 4.72
    Validation set perplexity: 4.58
    Average loss at step 5400: 1.580323 learning rate: 1.000000
    Minibatch perplexity: 5.05
    Validation set perplexity: 4.56
    Average loss at step 5500: 1.566411 learning rate: 1.000000
    Minibatch perplexity: 4.92
    Validation set perplexity: 4.52
    Average loss at step 5600: 1.583069 learning rate: 1.000000
    Minibatch perplexity: 4.93
    Validation set perplexity: 4.52
    Average loss at step 5700: 1.569499 learning rate: 1.000000
    Minibatch perplexity: 4.44
    Validation set perplexity: 4.52
    Average loss at step 5800: 1.584862 learning rate: 1.000000
    Minibatch perplexity: 4.92
    Validation set perplexity: 4.51
    Average loss at step 5900: 1.575870 learning rate: 1.000000
    Minibatch perplexity: 5.13
    Validation set perplexity: 4.50
    Average loss at step 6000: 1.550276 learning rate: 1.000000
    Minibatch perplexity: 4.98
    ================================================================================
    ons i limium and of aerasian grancered with they with its even retugnel highee l
    boest vister s for rifdi by ralied grouking of the for law tcreate catholos gisk
    and is a their workel hits when germ expeter translar zeung yearchop for his bau
    ver in isfal opposedsy horom of texhence nature afolwater earchers there steer u
    quemon entitoricational enong this one one partine because to printhcultion been
    ================================================================================
    Validation set perplexity: 4.50
    Average loss at step 6100: 1.565822 learning rate: 1.000000
    Minibatch perplexity: 5.22
    Validation set perplexity: 4.46
    Average loss at step 6200: 1.533982 learning rate: 1.000000
    Minibatch perplexity: 4.94
    Validation set perplexity: 4.49
    Average loss at step 6300: 1.542264 learning rate: 1.000000
    Minibatch perplexity: 4.97
    Validation set perplexity: 4.41
    Average loss at step 6400: 1.534368 learning rate: 1.000000
    Minibatch perplexity: 4.46
    Validation set perplexity: 4.45
    Average loss at step 6500: 1.555516 learning rate: 1.000000
    Minibatch perplexity: 4.60
    Validation set perplexity: 4.44
    Average loss at step 6600: 1.595624 learning rate: 1.000000
    Minibatch perplexity: 4.82
    Validation set perplexity: 4.44
    Average loss at step 6700: 1.578755 learning rate: 1.000000
    Minibatch perplexity: 5.36
    Validation set perplexity: 4.46
    Average loss at step 6800: 1.602098 learning rate: 1.000000
    Minibatch perplexity: 4.77
    Validation set perplexity: 4.44
    Average loss at step 6900: 1.581513 learning rate: 1.000000
    Minibatch perplexity: 4.81
    Validation set perplexity: 4.46
    Average loss at step 7000: 1.573784 learning rate: 1.000000
    Minibatch perplexity: 5.02
    ================================================================================
    legickaguan ient japan or dut arities highetzopions uri on quiltunar on the agen
    quated comporations which electime some two zero zero zero zero zero zero n pros
    x fighted shard ed five five one for performs of the make on in a of eriagnuch f
    wen is illd and alland to flaim of itingel the from clyotowa set kiok becriesces
     oponete car paliation on the souse bricting of uthankey melottokiculary the com
    ================================================================================
    Validation set perplexity: 4.44


---
Problem 1
---------

You might have noticed that the definition of the LSTM cell involves 4 matrix multiplications with the input, and 4 matrix multiplications with the output. Simplify the expression by using a single matrix multiply for each, and variables that are 4 times larger.

---

---
Problem 2
---------

We want to train a LSTM over bigrams, that is pairs of consecutive characters like 'ab' instead of single characters like 'a'. Since the number of possible bigrams is large, feeding them directly to the LSTM using 1-hot encodings will lead to a very sparse representation that is very wasteful computationally.

a- Introduce an embedding lookup on the inputs, and feed the embeddings to the LSTM cell instead of the inputs themselves.

b- Write a bigram-based LSTM, modeled on the character LSTM above.

c- Introduce Dropout. For best practices on how to use Dropout in LSTMs, refer to this [article](http://arxiv.org/abs/1409.2329).

---

---
Problem 3
---------

(difficult!)

Write a sequence-to-sequence LSTM which mirrors all the words in a sentence. For example, if your input is:

    the quick brown fox
    
the model should attempt to output:

    eht kciuq nworb xof
    
Refer to the lecture on how to put together a sequence-to-sequence model, as well as [this article](http://arxiv.org/abs/1409.3215) for best practices.

---
