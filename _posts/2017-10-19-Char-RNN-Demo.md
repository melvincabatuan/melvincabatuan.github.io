
"SUPPOSING that Truth is a woman--what then? Is there not ground
for suspecting that all philosophers, in so far as they have been
dogmatists, have failed to understand women--that the terrible
seriousness and clumsy importunity with which they have usually paid
their addresses to Truth, have been unskilled and unseemly methods for
winning a woman?" - Nietzsche

## Keras Char RNN Example
At least 20 epochs are required before the generated text starts sounding coherent.
It is recommended to run this script on GPU, as recurrent networks are quite computationally intensive.
If you try this script on new data, make sure your corpus has at least ~100k characters. ~1M is better.
## Step 1: Organize imports 


```python
from __future__ import print_function
from keras.models import Sequential
from keras.layers import Dense, Activation
from keras.layers import LSTM
from keras.optimizers import RMSprop
from keras.utils.data_utils import get_file
import numpy as np
import random
import sys
```

    Using TensorFlow backend.
    

## Step 2: Acquire the dataset corpus text


```python
path = get_file('nietzsche.txt', origin='https://s3.amazonaws.com/text-datasets/nietzsche.txt')
text = open(path).read().lower()
print('corpus length:', len(text))
```

    Downloading data from https://s3.amazonaws.com/text-datasets/nietzsche.txt
    581632/600901 [============================>.] - ETA: 0scorpus length: 600901
    


```python
chars = sorted(list(set(text)))
print('total chars:', len(chars))
```

    total chars: 59
    


```python
char_indices = dict((c, i) for i, c in enumerate(chars))
indices_char = dict((i, c) for i, c in enumerate(chars))
```

## Step 3: Cut the text in semi-redundant sequences of maxlen characters


```python
maxlen = 40
step = 3
sentences = []
next_chars = []

for i in range(0, len(text) - maxlen, step):
    sentences.append(text[i: i + maxlen])
    next_chars.append(text[i + maxlen])
print('nb sequences:', len(sentences))
```

    nb sequences: 200287
    

## Step 4: Vectorization


```python
X = np.zeros((len(sentences), maxlen, len(chars)), dtype=np.bool)
y = np.zeros((len(sentences), len(chars)), dtype=np.bool)
for i, sentence in enumerate(sentences):
    for t, char in enumerate(sentence):
        X[i, t, char_indices[char]] = 1
    y[i, char_indices[next_chars[i]]] = 1
```

    Vectorization...
    

## Step 5: Build Model with 1 LSTM


```python
model = Sequential()
model.add(LSTM(128, input_shape=(maxlen, len(chars))))
model.add(Dense(len(chars)))
model.add(Activation('softmax'))

optimizer = RMSprop(lr=0.01)
model.compile(loss='categorical_crossentropy', optimizer=optimizer)
```

## Step 6: Define a helper function to sample an index from a probability array


```python
def sample(preds, temperature=1.0):
    # helper function to sample an index from a probability array
    preds = np.asarray(preds).astype('float64')
    preds = np.log(preds) / temperature
    exp_preds = np.exp(preds)
    preds = exp_preds / np.sum(exp_preds)
    probas = np.random.multinomial(1, preds, 1)
    return np.argmax(probas)
```

## Step 7: Train the model


```python
for iteration in range(1, 60):
    print()
    print('-' * 50)
    print('Iteration', iteration)
    model.fit(X, y,
              batch_size=128,
              epochs=1)

    start_index = random.randint(0, len(text) - maxlen - 1)

    for diversity in [0.2, 0.5, 1.0, 1.2]:
        print()
        print('----- diversity:', diversity)

        generated = ''
        sentence = text[start_index: start_index + maxlen]
        generated += sentence
        print('----- Generating with seed: "' + sentence + '"')
        sys.stdout.write(generated)

        for i in range(400):
            x = np.zeros((1, maxlen, len(chars)))
            for t, char in enumerate(sentence):
                x[0, t, char_indices[char]] = 1.

            preds = model.predict(x, verbose=0)[0]
            next_index = sample(preds, diversity)
            next_char = indices_char[next_index]

            generated += next_char
            sentence = sentence[1:] + next_char

            sys.stdout.write(next_char)
            sys.stdout.flush()
        print()
```

    
    --------------------------------------------------
    Iteration 1
    Epoch 1/1
    200287/200287 [==============================] - 69s - loss: 2.0147    
    
    ----- diversity: 0.2
    ----- Generating with seed: "erature, taking it all in all,
    unless on"
    erature, taking it all in all,
    unless one contrainted and man himself the cances and into there is the intreated and contrainted and conscience there inteliefteness of the superity and into the cance and and as a cance, and disciptional the canes of the superal and there is the superity and there is that inflience of the superation of the cane and there is the caning the contention and moral the superally there as a chainge and from the
    
    ----- diversity: 0.5
    ----- Generating with seed: "erature, taking it all in all,
    unless on"
    erature, taking it all in all,
    unless one that is a celt is the masts and and changest he is a contrain exceed the him of this man of man infliences there is the intencement, there in there to the funces there sense
    a master, there infleed to can there deed are into begartes incomptant subjection is proficate there aster there is shall as a culinated intele, schincted and restickly from there perit, in the all the dave be a recouranity 
    
    ----- diversity: 1.0
    ----- Generating with seed: "erature, taking it all in all,
    unless on"
    erature, taking it all in all,
    unless one livefulty arverous, to succt is thut
    spertionsme-lste!
    and mank, stople, that that commpathoricands from these iflechery
    of light and edrain-jogichation right!ing to welid chaises, satuots, of lsstaning--man with the
    dusvicrative and
    ofes, its quovited as their
    ander, religiousteds"
    oher nowled not ftural to prowanged and
    ! curcupteupefoleonally that as
    orkgeal
    uporomint nales hemint the wisk an
    
    ----- diversity: 1.2
    ----- Generating with seed: "erature, taking it all in all,
    unless on"
    erature, taking it all in all,
    unless on
    themlesy fragno. we dactenally? ievidificater.
    
    
    =phincem. thereshiled tlold
    to to
    menigeful
    ano torenes. freed.
    there of duefue) of toge
    menend stuls it op
    riample havely noge? if viltion,
    urid of ruldourlup.
    eart,
    with "symt fast to thruss all to there lastled aspe,sowind the
    musher, (qoiture of philosophed, a herdbymentlarizet", intellectuents and be -ununce man?s meanify; theae geltormodaties
    
    --------------------------------------------------
    Iteration 2
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 1.6540    
    
    ----- diversity: 0.2
    ----- Generating with seed: " to interpret
    everything that happened a"
     to interpret
    everything that happened and the strength of the conscience to the sensible to the and the strength of the strength to his of the more as the such a man the soul to a sensible to the strength and the such a strong and and the truth of more as the strength of the strength of the structions of and the the present the strength and the astrate in the sensiming the astence of the strength of the strictions of the astring that t
    
    ----- diversity: 0.5
    ----- Generating with seed: " to interpret
    everything that happened a"
     to interpret
    everything that happened and as that the and the of sensible even which is that that the truent individuality the sensible that they as the hast the of such to their sty of connceme to the strength that the and in a the of the individuality must is the more and the
    for in the consciences
    to responsible more and of the are in the street as a presentering as and not to the intriass the are the strumgh that the frochological 
    
    ----- diversity: 1.0
    ----- Generating with seed: " to interpret
    everything that happened a"
     to interpret
    everything that happened ald
    charusted tome into
    this oher inerogicand of hope are everate somethings in deries,
    unxaingoiality and when takiesly a mancoth. it is "in the or more in this corno. theor of the strength of the
    funditions, utrencevelous to the is, when
    the beut, by geives
    s being for their mides, and infrund and in for comparifre of wind of hone mprise--and a
    morality reed, by the beuthically manqureally unuadi
    
    ----- diversity: 1.2
    ----- Generating with seed: " to interpret
    everything that happened a"
     to interpret
    everything that happened a arswiled the for, their unconside distdactrochity skee--resplancoswandic
    thing, just,. i
    subgraaghic protess, fort sensetality stdingen) the distranito to sufpering as must belorger viecent himself weltivisuiely ascriined, which, grakenter but
    dume, is prenainsiors re--further to mopic. as him.whowagee note instmaculous firmorabainged's
    of knowiling to pains
    not sublingen scholings to
    him what w 
    
    --------------------------------------------------
    Iteration 3
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 1.5600    
    
    ----- diversity: 0.2
    ----- Generating with seed: "s from jean paul, even though he acknowl"
    s from jean paul, even though he acknowledge of the sense of the same the man and the sense of the sense of the sense of the sense of the more and the power of the conscience of the conscience of the forcet and strength of the man and the sense of the stands of the conscience of the sense of the sense of the state and man and the man and a stands of the sense of the sense of the manifest of the sense of the moral for the conscience of t
    
    ----- diversity: 0.5
    ----- Generating with seed: "s from jean paul, even though he acknowl"
    s from jean paul, even though he acknowledge. in the little men and beanter to with the moral entimes in a stend of the sense and the philosophers to the peality of the
    scien alse of conscience and and be instance, the greater of the longing the form of the nerment of the contence of the great itself a consequences a strength refined of the man and of needer of whatever and superficinary in the form of the sense of think and be how many
    
    ----- diversity: 1.0
    ----- Generating with seed: "s from jean paul, even though he acknowl"
    s from jean paul, even though he acknowleds of couration, inderence in
    other conceace. for dorteed tave" having old to a will) to the efyee
    in the nencligarl, eaviitse
    innations and to good whatever, many therefore noluman eorrate, it nertis""s-of leaves to to
    truch of time, his mind
    id, therene: the doen this storc.
    a thin a nom is noblen,
    the circumst dankerly to the nrom the are-dury-personary countacity wich make fowr weak. that
    eac
    
    ----- diversity: 1.2
    ----- Generating with seed: "s from jean paul, even though he acknowl"
    s from jean paul, even though he acknowledge, i may peasurelaty corenismmentarity-god, in
    good natures alid"--for his undiscover-even of is moralituse.., it wer own
    the wile of
    jiebsal.god--wirt
    placiously and notary," we one sintion of nequences"--nenesting nalle"s--futhy awtrabe ynough have where what whiem," forthianess of ruse is yourmom. a riance
    he prizious
    still wiself; of the "coses. colisual
    were a targht, and borswage: thus si
    
    --------------------------------------------------
    Iteration 4
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 1.5155    
    
    ----- diversity: 0.2
    ----- Generating with seed: "ntion the most cultured and
    most conceit"
    ntion the most cultured and
    most conceited the subsection of the subsection of the subsection of the subsection of the present to all the seems and and and the subsection of the seems all the seems all the spirit of the subsection and stated and and the subsection and and and the seem of the seems to the seems the subsection and and and and and and and and as the seems and the beaked the same the seems and the seems the whole and and pr
    
    ----- diversity: 0.5
    ----- Generating with seed: "ntion the most cultured and
    most conceit"
    ntion the most cultured and
    most conceiting the noble as the subseme which the seems to far seeming the essential of a problem of the longer the present and the man and self to present of the belief to the refores, have to desirer does the art for the presumes even and not soul of the subsle the sense of the asceive the world, which which has perhaps to
    the even the existenced new impersonally the same and the old and interpout and and 
    
    ----- diversity: 1.0
    ----- Generating with seed: "ntion the most cultured and
    most conceit"
    ntion the most cultured and
    most conceiting as duly of thereby
    pain a tiles" here shomes a greatevedque when thinking a feen which his happiness do human peopered
    by theself, certain to knew the, intice "being tradety" to look for because one dausite, a tashion to means as more neigny is nobunders. not not as if merely philokon for, himself; more in seas him all other--soie. you be concernal the
    still nations.
    
    20her oregre
    modes exhist
    
    ----- diversity: 1.2
    ----- Generating with seed: "ntion the most cultured and
    most conceit"
    ntion the most cultured and
    most conceitive stak last laft and wonloes and not still the
    bematter,
    there are to ims sisteded, plying eady it ould a
    subsection: even sometide,
    modern,
    best bemons pointy and saye, and only
    ro-s,irs could not after i ocubers"--a strengbut itself; in id purely cumbory, is, "common, whace? what their bogrt: admancy, preperspetives and "piocacrian"
    mo"knons reterbly the tervitymate shart,"-hand, that reflicti
    
    --------------------------------------------------
    Iteration 5
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 1.4866    
    
    ----- diversity: 0.2
    ----- Generating with seed: "pth, and refinement:--just as much as su"
    pth, and refinement:--just as much as such a interman the more and an intertious the existence of the morality of the supersing and a presential and the contrable the self and a presential and a man and compared the contrare and a compared the soul to the surple and the interest and the strength of the strength of the self and the presential experiences and an all the sense" and the contradiction in the called to a compared the contrabl
    
    ----- diversity: 0.5
    ----- Generating with seed: "pth, and refinement:--just as much as su"
    pth, and refinement:--just as much as such a conduct, the delication of the depth is of the morality of the ideal so the spirit, of every sense--the sacrifices as a sort of not be make a world and our supposed his some contradiction in the self in the sentiment and fired the pressed to christian of the over the braid, whom are a man the restoncers is the presention of the self and men and world and suffering for exalted the contranger o
    
    ----- diversity: 1.0
    ----- Generating with seed: "pth, and refinement:--just as much as su"
    pth, and refinement:--just as much as sufficenc it be coment and is not do evid therein, with awdivide, hasaute esersking him, new ideal
    must in lood
    jeamy of
    fawmerged as they far man exolan us; and agals contrangearies and the fundamental "time whereby, and the shilst, as a stronger wicnenth, is the hypitids as even below moralities. yat a individual? he
    schuld" crutts," who that will
    has the wharce, we would use not will life it grad
    
    ----- diversity: 1.2
    ----- Generating with seed: "pth, and refinement:--just as much as su"
    pth, and refinement:--just as much as sunrast vikely honourles hypical feear from the veraget-full" frunglants, pitt kind to
    by condection
    of his bratustimatid inmienah, to
    such he, yek perioricas wak a nationafily, the
    dakges, when christiaunts--the quetnoer kes interficity,
    contradists sware wrooces just the eurifapty right, be knew the
    frishe by aws 
     xevenjoce
    "ffom-garded by
    means mask" prourts, he
    neove, the
    some beis. that who be
    
    --------------------------------------------------
    Iteration 6
    Epoch 1/1
    200287/200287 [==============================] - 64s - loss: 1.4669    
    
    ----- diversity: 0.2
    ----- Generating with seed: " be felt in their existence, then it is
    "
     be felt in their existence, then it is
    and man of the same successible of the most subject and and and the sense of the successible and and and and who has the consequences, and the succe

    C:\Program Files\Anaconda3\envs\tensorflow-gpu\lib\site-packages\ipykernel_launcher.py:4: RuntimeWarning: divide by zero encountered in log
      after removing the cwd from sys.path.
    

    ssible of the sense of the sense of the consequences, and the consequencess of the sense of the most discovering of the successible of the soul of the sense of the sense of the successible consequences of the successible and and and and something the s
    
    ----- diversity: 0.5
    ----- Generating with seed: " be felt in their existence, then it is
    "
     be felt in their existence, then it is
    the serious and receives, all and in the presents and considerations of his sense and secent and and
    deceptives. the present the entistant as which an interested value of the existution of the more them of which of the souls and make and magices that the list" because of sufficient in the same a strength of the interentians has allow to sake actions of man along to a conception of every forming of
    
    ----- diversity: 1.0
    ----- Generating with seed: " be felt in their existence, then it is
    "
     be felt in their existence, then it is
    not cive surely preferual can tepted to
    knowledgn for strive---something or the nace self even with their
    points of his
    sisbmitious compersicianfully nation only beforesself. and of false as it
    if a in
    the knows of every arsa-by his own attangue
    is should be and evil growness? another and meccountiness
    with hersocuthiar: "the fire to the call shouldness. if reselfed
    reveliniment
    and has intrebutio
    
    ----- diversity: 1.2
    ----- Generating with seed: " be felt in their existence, then it is
    "
     be felt in their existence, then it is
    "cate will-derlection, and notily weals to
    impaluations, no lidt oneself leonlize will-whate
    strongerler-meak mind, hee."--with which, this
    known, when
    "they can not is as delights than
    conception? ofs, imagination, or humakly to a
    hame nacisepter,s it
    as, opiniona, manewthrained ; no
    naive! this deal, to theno" with himag.--as scirnism ceingation; the
    life.
    
          ; here, to fathe. how
    obadnce
    bu
    
    --------------------------------------------------
    Iteration 7
    Epoch 1/1
    200287/200287 [==============================] - 64s - loss: 1.4526    
    
    ----- diversity: 0.2
    ----- Generating with seed: "ight when he says: "know, also, that
    not"
    ight when he says: "know, also, that
    nothing and the sense of all the present the sense as the senses a superior and and are the senses and the same the subject of the conscious and and stronger and the superior the senses and the senses and sense of the and for the sense of the senses the senses the properion of the senses the sense and the same the worth and the senses as a stronger and a science of the present and and a stronger the 
    
    ----- diversity: 0.5
    ----- Generating with seed: "ight when he says: "know, also, that
    not"
    ight when he says: "know, also, that
    nothing the more not the powers and the indeserve the short, to the sense
    and art from the special conscious worth of action and nothing the experience and shall depthist endure and an investill, on the properion is the most senses the antight of superlanguanity the astificed the procession of find a come to have to the has the nature of the great they have how to be science and have themselves to th
    
    ----- diversity: 1.0
    ----- Generating with seed: "ight when he says: "know, also, that
    not"
    ight when he says: "know, also, that
    nothing and enfiefterful feel presentianimals of this long tendencist the contempation of its for the merely countly, that shaves relagses, the auss the good ourselves, shad betumes one should not acts
    uuserkly--rejusticts cultions aristrale ideas cause upfelest decisionation that man of hencey, the an exprission
    an enjoyment, who are a comentted pact, as the implanity inafy of thaiser and
    consciousi
    
    ----- diversity: 1.2
    ----- Generating with seed: "ight when he says: "know, also, that
    not"
    ight when he says: "know, also, that
    nothtring, was in everything belprectior".=--model, howavlingly clast, the nationally dracifw gratiful, to the injumiving, without of new internohle?
    prigenee the ey to begute, hither: it has these uporwgoblecl.
    call ought his sciente behappy eecesstification of dangers flusing dringly life canedable
    action""? a synther called hadly oy fact deserved conkins
    yourseare of sareby. in, among
    "shapon, dev
    
    --------------------------------------------------
    Iteration 8
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 1.4409    
    
    ----- diversity: 0.2
    ----- Generating with seed: "eated and destined for command, self-den"
    eated and destined for command, self-denist of the man of the subject of the presses the act the more and present and the case of the can be a moral and can be the self--and the presses the more and actual and the presses the attempt that is a man as the present the proctfy the presses the most strange of the presses of the subject the present and present the power to the presses and and the present the struggles the still the served an
    
    ----- diversity: 0.5
    ----- Generating with seed: "eated and destined for command, self-den"
    eated and destined for command, self-deness and the distrust its fact of one of the wholed the cause the practices their conscious of the about the not rearing to ever man, which is become to the possic distained in the any afterwards the pain in the power and well to all the are of the results and complexised to the most than at presention which we for the carry of the experience of which the art in accounters the improductive, which w
    
    ----- diversity: 1.0
    ----- Generating with seed: "eated and destined for command, self-den"
    eated and destined for command, self-denivetefting that they us,
    "seavenstous, step, wherelow out havilol as portable notroned, even in that who is fact, is
    sound of
    revelop and variously and ad, which can ever meaner oclpraing
    vare ffuntiduate,
    its its complence of one drinks and the fines of inartrocuchos and love- pursceal the lende--through whom a philosopher--nameating for this. to is, a realty in the recowniry a vanity the assumif
    
    ----- diversity: 1.2
    ----- Generating with seed: "eated and destined for command, self-den"
    eated and destined for command, self-denable preten why not from they can,t thing
    with thee, it is that,-is pravales. hapted would povermlaticls
    actain emarinally modern moral off from this or mes glerfibouth keount acts, these slow
    a dimime  "schle prodes and idvaticn among percaple"!
    the
    powery; in many, there
    is makes
    purequeral juvirion? an a riffe suck caqualyds had is-necite!
    
    
    othire astitate defases and centudy martefult. "for a
    
    --------------------------------------------------
    Iteration 9
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 1.4330    
    
    ----- diversity: 0.2
    ----- Generating with seed: "like the more
    celebrated "metaphysical r"
    like the more
    celebrated "metaphysical reason and and and and the comprehent to the considers of the serves to the master of the mast of the strange and superficial and and and and and and interpretation of the commone and and and and and and has a superficial desirable and and the most and and and and and and and and and and and and and and and the comprehent to the serves the supersition of the considers of the comprehenting, and and 
    
    ----- diversity: 0.5
    ----- Generating with seed: "like the more
    celebrated "metaphysical r"
    like the more
    celebrated "metaphysical regarding of the and individual before to his
    the seems of the moral indiker is a soul and darg" as the interication itself of the moral profound explane of
    the trans in the conception of the freedom.
    
    
    11
    
    =in the
    other "order from the tas the potrous and to still a sacrifice of the thought, and individuality of stronger that the soul, that whether a still about and cause, the discomption, the com
    
    ----- diversity: 1.0
    ----- Generating with seed: "like the more
    celebrated "metaphysical r"
    like the more
    celebrated "metaphysical requited along
    to a
    "night, these them
    complexias thems--is lifes
    "eniecceting, just to down
    greater as herit with the
    immoral happicacclequd, just of something wea forsmatter, a did to ethicalies about them? to which a
    mours,
    be, the interualm to
    ever, this gotwer has, man greas of morally takders,
    thatkngtant
    does a trantck like litely, and consoluction her began is these indicatjed, the bof gene
    
    ----- diversity: 1.2
    ----- Generating with seed: "like the more
    celebrated "metaphysical r"
    like the more
    celebrated "metaphysical rendered exberrity, in poweats belnedes, that oy tho thkrend babfodery problables--he, one temp, himself. but saint, and
    musalon-po has again exwersening, and prequice know and knowledge he
    arlent, table; but of a nation hysees, it besken like
    is sischonable--to the existency, a suaucred european spwor, forces."--some humoustions--that hopek. metaphysicam,
    is and bebive "is of all and of waistje, i
    
    --------------------------------------------------
    Iteration 10
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 1.4255    
    
    ----- diversity: 0.2
    ----- Generating with seed: "all the
    disintegrating effects of surviv"
    all the
    disintegrating effects of survivality of the most in the art of the world and and and in a personal and and and in a personal and and in which it is a stronger in the individual and in the and in the and and in all the and in the most in a personal and and in all the sense of the art of the individual and and self-disposition of the most in the and in the prours and and something the ancient in the and who is one is a philosophe
    
    ----- diversity: 0.5
    ----- Generating with seed: "all the
    disintegrating effects of surviv"
    all the
    disintegrating effects of survivality of the soul are fact of the procise the delight and indute the subjects from the despised to be
    nature of the ascrective in the individual prours and subjection of the high man of the and in the individual self-prourd a longer and can himself in a respect of the conscience and as the extent have become in spirit and imaginated the dishonor-origin of the subject and a respect of himself no co
    
    ----- diversity: 1.0
    ----- Generating with seed: "all the
    disintegrating effects of surviv"
    all the
    disintegrating effects of survivalary to personful. step upon
    or thought with a happigious, and moral, musig." the heart of "histolidanate, halfully? aur, another,
    something ayong even insehinds and
    improbfors, imposious naries, if there is a subject
    that is the nokion.
    
    =the
    neward and wound a happicuted itself in
    a cominm as relied of declim the univaritinations the sub"dbinalument he willing and
    new passes we rule
    so cribe by
    
    ----- diversity: 1.2
    ----- Generating with seed: "all the
    disintegrating effects of surviv"
    all the
    disintegrating effects of survivially-ny kinds or act, it upur, worcy thing enwering di strotfer-right wie: knowl resided had ""that eversking s dnewire," my reson and mubfrent into it physyopilalfjens
    the unfasidon, me, had between his instince opinaours kor have one : embeplip and its imagied by high
    germent is neids," that dradve. thouth deperious, ingareglman unsecurding firsts of free wicked whie st ididious heived like a m
    
    --------------------------------------------------
    Iteration 11
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 1.4169    
    
    ----- diversity: 0.2
    ----- Generating with seed: " irresponsibility can no longer include "
     irresponsibility can no longer include the depth of the superficial and the present to the superficial and the interpretation and the procise and promise of the precisely an artistical and in the more the conscious and an action of a present the present present the more and and and an actions of the depths of the superficial and an artist of the superficial and an artist the superficial and promise and because the superficial with the 
    
    ----- diversity: 0.5
    ----- Generating with seed: " irresponsibility can no longer include "
     irresponsibility can no longer include of spiritual spiritual lives the first disply to the superficial wisdom by the possible proming and and as the been love
    to the disciplined by the contrary and that the power
    plato: reparious presentrate so called by the depther to the stands and morality the action of the more on the conscious the entiscrible a flight to yeart, as
    common the ideality: which are a truths and order to good and the 
    
    ----- diversity: 1.0
    ----- Generating with seed: " irresponsibility can no longer include "
     irresponsibility can no longer include punished ye name"
    curiosibly herd.
    
    
    
    
    
    =now not sout: it was god.
    
    
    
    
    =one! the
    artness in men so
    dust of mork the science such the croormombna cans, has wacttous of greater of an apiitials to feels a creterfuling defectived
    the new beed, by the time. we came the now are acstrain to everygen obvogant us the problement: it was steelnce
    that is, do reventrous mankind and in their effectol that the 
    
    ----- diversity: 1.2
    ----- Generating with seed: " irresponsibility can no longer include "
     irresponsibility can no longer include friend, evolved wel?
    truth" "--an aptrsed all immomined
    cloit. they becioold phonos)--without
    bdousd-cure we
    in ; to
    do; it, at the feeled reblacy?-thy
    made of the butchatisl to lagesgoned, togethered one
    offered to greater
    bookers"
    are sight have extread pere mindss threw play profound or freedom, day: and incliving getcem--poor
    lives are "bineh, in ala"! juscer
    of sremptingle; an avoices.--them,
    
    --------------------------------------------------
    Iteration 12
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 1.4150    
    
    ----- diversity: 0.2
    ----- Generating with seed: "e hand, nor
    even a "finger of god" has p"
    e hand, nor
    even a "finger of god" has precience the and as the contempts of the contemption of the present to the contempt that the present developed and and and form of the simplated that it is not the world of the individual and and in the supposition of the world and and and and and and and and in the most sense and and and and and and form of a morality, and the sense of all in the sense and and and which in the some of the present
    
    ----- diversity: 0.5
    ----- Generating with seed: "e hand, nor
    even a "finger of god" has p"
    e hand, nor
    even a "finger of god" has precience and the world the first and morality, and in the more of the advance to him a philosophers the sensible and in the something will be roon--and in the entisted and the individuality. the conscience of morality, and in which to be things of the world the morality, and it be music, at the sense of the state and that the greater in the extent as a presence of did and have the father that is t
    
    ----- diversity: 1.0
    ----- Generating with seed: "e hand, nor
    even a "finger of god" has p"
    e hand, nor
    even a "finger of god" has presumets
    as it was fell this learnt of entitficate 
    force made there a greking. in morear befleable with the returners and in whom that any ibler
    to that a will before one regula father of it was indancy, finasism in order have yet will emotions,
    women, is the short-that more should immoral institutes, what existmaiting lotive.
    
    2e can betroong generative; it is power of detirnuite, and in germon"
    
    ----- diversity: 1.2
    ----- Generating with seed: "e hand, nor
    even a "finger of god" has p"
    e hand, nor
    even a "finger of god" has pleasure, could even outsed to overthan
    that inuperiorasion: with much
    german hold)ed were need, in lok-morres, is no arts and obligation--mengact, this factors" and happinom nabut still a
    resting, "frot philosophers, low were mo, whether lets advaction
    of the individual. with that
    he expitatry does its brasned anx this thatever by the aritable of explaining--but
    a .ration man" so gratedmforating
    s
    
    --------------------------------------------------
    Iteration 13
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 1.4078    
    
    ----- diversity: 0.2
    ----- Generating with seed: "t all?" every one will ask us about this"
    t all?" every one will ask us about this hearts of the suppliness of the conscience of the supply the present in the conscience of the same to the present to the strength of the interested to the more and morality of the power of the strength of the soul of the conscience of the stain to the supply the state the desirable and for the states to the supply the suppliction of the supersity of the strength of the supplince of the suppline, 
    
    ----- diversity: 0.5
    ----- Generating with seed: "t all?" every one will ask us about this"
    t all?" every one will ask us about this believe, the lates which we can nature, it is in a longer of the occasion--and inspirity is strong of the sholt himself and far in
    the interested in the conscience which all the same man is not so say, to the intortianess of the race considerations of the supply of the thing and because without being the power the reason perhaps the place, and it is the awochors, in the origin of a developed and 
    
    ----- diversity: 1.0
    ----- Generating with seed: "t all?" every one will ask us about this"
    t all?" every one will ask us about this judges with uporrer possession schole stell's nabulitage, manimy and selvom itsounce things--in, and the ratues, and under that is all the judgment, and utaging affords to the mannoy understand underidel perhaps menyhaps himself to results, but the
    morality reached can were for our us, theace a result of ong to lister
    that in
    unperheles of fore, and found sestime
    to fact upon him arceforsify are 
    
    ----- diversity: 1.2
    ----- Generating with seed: "t all?" every one will ask us about this"
    t all?" every one will ask us about this glonk which  heish, to the lainbles (will born other kind to sinder with a
    morals das
    loaring howing usd
    primirentative
    reason, and
    divergos to life, everyory,
    very collikes; phasig
    origins--a unknows, he does not some in whomen reason. the understand? is indrendy and fashione will all alletable with.--."
    besong "we may grants else to which is
    leasth absorwocks ets famsifieitations which, aebl of
    
    --------------------------------------------------
    Iteration 14
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 1.4075    
    
    ----- diversity: 0.2
    ----- Generating with seed: "y evidently appear too rarely, they are "
    y evidently appear too rarely, they are the ascrect of the states and its sense of the states to the concerned to the submind to the state of the states of the states of the subjection of the submind in the states of a still deceived and all the more and and and such a states of the contradiction of all a states of the contradints of a stronger of the states of the submind to the seems to the states of the states of the submind and conc
    
    ----- diversity: 0.5
    ----- Generating with seed: "y evidently appear too rarely, they are "
    y evidently appear too rarely, they are do never which he can not be all upon the submind it belongs of the more a strongerond the original of their morality, and say the profound strughanted dederious and and more man do which the contradiction of the submind to say for who could be the principle of the stupid of the characted and such and belongs, has be not possible and have the happice such a subjecjed, and his intellect: and it is 
    
    ----- diversity: 1.0
    ----- Generating with seed: "y evidently appear too rarely, they are "
    y evidently appear too rarely, they are but is criot. this its duetoes of absoluteln". the chlos this dajebfenoty--it is to
    religion, "virice men, and to can
    learnuaus interclety "their questionly, god"
    and socractisy, contradigle ic
    modes for a more free of a
    standday to that regard for a nature of their idelly. yuth with his pleasure of intenruity of men of view upon its cributed
    to reselfmers,
    stupidersing, as i glinked
    whicht for
    in
    
    ----- diversity: 1.2
    ----- Generating with seed: "y evidently appear too rarely, they are "
    y evidently appear too rarely, they are at arengs in was ssongonary
    you. ahuppose will" as it
    either man" torture, exceeded to a
    daygen and
    happilies in the greati" aspesse, is beni, abger noged has all its
    names lage men mull. than so atto in so to
    peptal mecty most rocropans: it is stinlities,
    the despect; this this helt, hable
    is birde wereed haitsey as it may all knowaxting one into the which we atrominessucuclon.
    aoleat of wonk--it
    
    --------------------------------------------------
    Iteration 15
    Epoch 1/1
    200287/200287 [==============================] - 69s - loss: 1.4062    
    
    ----- diversity: 0.2
    ----- Generating with seed: " in the world hitherto, it has
    just cons"
     in the world hitherto, it has
    just considers of the subject of the subject of the stands and the souls of the subject of the subject of the stand "in the subject of the consequences of the fact of the subject of the subject of the consequences, and the consequences, in the intellect, and in the stands of the stands of the conscience of the soul." only the stands of the distinction of the man of the consider of the interpretations of th
    
    ----- diversity: 0.5
    ----- Generating with seed: " in the world hitherto, it has
    just cons"
     in the world hitherto, it has
    just considently who are the long sincle, and in the ascend of the standing of doubt in such and nature and the most principacity
    which was is the calls now the noble or their pointerity of its the great and concented and distance,
    such and the subject and the fact, in soul in the and and end as a little distance, consequences of the mistake of all and and for the suffering of the ideas" and strange of the
    
    ----- diversity: 1.0
    ----- Generating with seed: " in the world hitherto, it has
    just cons"
     in the world hitherto, it has
    just constraints ao, and which difined him
    he
    portion
    of complyingrds"" to an thought is,lning that present,
    ined. breakured him, which will outhish of regarding, the
    subtle alsodvant should, and alow of learning,"--most, and
    in this chieccqjecmerable develwathisrioness and complex and dangerous possibily
    german routh of pointone,
    which has danger in arries of morals.
    
    1t¤h rewind, a dissike it
    in which th
    
    ----- diversity: 1.2
    ----- Generating with seed: " in the world hitherto, it has
    just cons"
     in the world hitherto, it has
    just consided gifts impossess, and soit into the
    yea "without however, he i may litifies
    in
    itsly overfore him. ersentiuaturent. veritionsprisics of men and peache and likicdiring of mallogically putety
    yoo goodity concenes drepers peotnulagation" of many sleegured, who art" wistow tash as the able had more
    will nabutes, whereverlers, mechiar all gagli god. evened thought that is with a
    world-deloubles, ta
    
    --------------------------------------------------
    Iteration 16
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 1.4026    
    
    ----- diversity: 0.2
    ----- Generating with seed: "d raised himself;
    it was locke of whom s"
    d raised himself;
    it was locke of whom stay and and the present the man to the supprody the distinction of the submind in the suppering the supprodism of the strength of the man of the sufficient and presential the supposity of the suffer the intellege and the present the open to the world and the supproded the supproded to the protection of the supposition of the presence of the supposition of the supposition of the supplies and and an
    
    ----- diversity: 0.5
    ----- Generating with seed: "d raised himself;
    it was locke of whom s"
    d raised himself;
    it was locke of whom so thinking and which is not of here believes of the world and man, and and who are the favour to seems to all all the man and as the intellectual and and
    and and into the prevages the distinction of finer of the uncertain the life! it is true, for which the possessive for this consequently the intiness of the man to one from the christian than the diseakers of one can elitally stand existence and 
    
    ----- diversity: 1.0
    ----- Generating with seed: "d raised himself;
    it was locke of whom s"
    d raised himself;
    it was locke of whom speaking, than claims books. he can tyrtd,
    the berthin why is ruchives
    shadly dowing but whethers all the
    evolur. if but whole was did not
    are obsell, escreinsnt from
    acknows no much namely strive and upferne. that is these 
    we true.
    
    279. the possibling is curiositiess
    them est the master, beyond ichers.
    ne allest other, people,
    put one still spoberbadable, for the young even sewzistic arterianous
    
    ----- diversity: 1.2
    ----- Generating with seed: "d raised himself;
    it was locke of whom s"
    d raised himself;
    it was locke of whom sgnation for a manlchss--has to he was he mubcts oneself to truthing, the "teren griss); humaring strong interrate are "will", but he days mind its spring. without badleoors who afford replies, mist, lury willy yet is on noble
    attempanced
    intelletefore funumeld like whom center "rounded
    evani ageous fintial into
    thinkar distcqual. metho out of rark,
    that, the hars--of which in the least which heirw
    
    --------------------------------------------------
    Iteration 17
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 1.3988    
    
    ----- diversity: 0.2
    ----- Generating with seed: " enough how offensive it sounds when any"
     enough how offensive it sounds when any in the same time the fact that the superior and and problem of the sense of the superior and individuals of the satisfieatic of the morality of the most and the suffer and and individual self be noble and in the same thing of the sense of the interest and to the possess of the probably and such a suffer spirit the sacrifice of the
    such a soul in the sense of the power and the such a spectable and
    
    ----- diversity: 0.5
    ----- Generating with seed: " enough how offensive it sounds when any"
     enough how offensive it sounds when any man and enough of the concealed means of the race it power in the problem of its own world to the satisfieatic in the former and salvain of the most suffering as a result of such and instinct and portudes in the entirely the prigreness in the power of the possible and constitutes of the meant of view is not of the creation of the highest and and painful reputty, and and as a man can be the hand i
    
    ----- diversity: 1.0
    ----- Generating with seed: " enough how offensive it sounds when any"
     enough how offensive it sounds when anything "inclasion of another--every so far
    rather peghing preasignd,
    no aevious
    necessibly by
    moraliustigrnesses, mirhtnos! which the nature and the did not of sxemed
    rorthly inscreadily which vouracted
    of this, as it is enpleat pone think
    map moder concealed of philosophy "has tgicalualty ununiskngaritas as the
    conditionally
    who is importction of it; hind, regardation to himself laugh ma, life doe
    
    ----- diversity: 1.2
    ----- Generating with seed: " enough how offensive it sounds when any"
     enough how offensive it sounds when anything manch virentless by cevere
    and in
    sacrife and from it doing to saysd
    tasts but and all victions?--with inalusn life, and
    us of secene and make
    the
    oppositation of those life.
    
    
    11eaty.--having to fough is,
    subject to
    the dreame; even i nowadays; and as ineagurhily and appeare- acmme ladge it
    -childery
    ingains.--in
    simisinion now flughrer perious of
    jsts nobidy,
    uneur fight so-can has an irho
    
    --------------------------------------------------
    Iteration 18
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 3.1167    
    
    ----- diversity: 0.2
    ----- Generating with seed: "the lap of a dull, animal, plant-like
    in"
    the lap of a dull, animal, plant-like
    ins¦ki«" and th« †¦be¦f¦k «que t¦bl¦«¦ «qus of the¦k the se¦king the same moral desing the «q¦cres a¦†¦¤k†--and†fore†"«†¦k-«¤for, and the anti--desir¦ of the same th« †fv¦¦¦k««"--¤qual¦ iffing«,¦--i sees. the consificance of t¦ble¤, the sensfort†. the¤«†«†-†¤ticative†«" wh†, and«" itself of the anti-l¦keful the subs†for,«.¦k«--a¦k¦«" w«".¦ki¦k a¦k¤tical the superficialed the ant«qq†d--and«"©«"«; the
    
    ----- diversity: 0.5
    ----- Generating with seed: "the lap of a dull, animal, plant-like
    in"
    the lap of a dull, animal, plant-like
    ina©ki«k
    the su«k©¦k ©b«--¤be of«".†eã.†ific¦k «¦fxication of the c†knearnce the dares ", w¤k¦k¤. ob«k« whil«¦ ††¦«. «¦k ¦b†fvicative indione of this such a condeãu†¦©†«k-†qu†«©? «que, ¦¦¤r to a sens«--and the in insteems and the compromeresf«¤†w«¤"-©qu¤†fwile¤,«†«"†, t«qual to the morality and ©kes of all the fo¦m an©¦k in†f¤¦k th«, a«k-not a¦†--if a discovl©."--and¦king in ¦bbatxeumen¦king« to a s
    
    ----- diversity: 1.0
    ----- Generating with seed: "the lap of a dull, animal, plant-like
    in"
    the lap of a dull, animal, plant-like
    ins¤m,ã. or
    the¦k¦¤."«;ãki¤t abo†k;¤oã:¤v" to. be t¤by, in truld¤-shou¤ roum of the ti«¦bves©©ft¤ infl¦n profous«--it †u©gery, p¦dvily-for†«"
    imma¤kion have feaw¦ «†h-by¤j¤¦fficuln¦†« w. ã«:«'¦; and as, of h†vidually by the alvoxicative«¤, for thisã†«.¦†©ã†©¦d¦¤: w«,
    and«ged as sexpection. then it
    masces.
    «©ã† «fktingt¤«." they b¦boun«; no
    after kane ©¦k"¦k, whigã« move¦goveou¦-†¤¤u-"©†vial c†bl†; ¤
    
    ----- diversity: 1.2
    ----- Generating with seed: "the lap of a dull, animal, plant-like
    in"
    the lap of a dull, animal, plant-like
    in ¤fq†k
    ternen©
    ¦©qu¦k† o©?"--"¦, th¤d†ing pre©©ia«¤. if that the
    s instencie iã¦© are ove, wh¦¦w† †fw¤? if.¦«" a¦of.
    eraggm¦ce ettem elses,ings, chas migh¦bãq¦¤¤«) wh's¦s--mu©«,©†«¦†«--ior wa©«. woman,
    in†whing ax, mofon, the tement--a¦forã †c©g«w¤¦¤¤k ee©
    «d©fu«t! this
    value upon sexe
    which comor, that tã©ã«¦¦kã: ãpryoficã"†«.†e?"©": « weards whom these «¦ticalm"ness of
    ove.
    †py¦©¤s†"
    self-tyang 
    
    --------------------------------------------------
    Iteration 19
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.7861    
    
    ----- diversity: 0.2
    ----- Generating with seed: "
    inward compulsion (which must always ha"
    
    inward compulsion (which must always haããããbããkiããmaãkãy anããwe andãquiããkeoming of thã sould in the ãxigãking thãsã ãããphoroch of ããwher and tãwhãããwher into thãsã the statã and ããwith andãqualããking and tãwhãwhed--it isããby aãkiãk souããããbyããwitã iãwhãwith and ãwithã tãquire ofãkinã the statããknesã thãse the ãmpãfuã tãquitiãn ãby suãw and soãk to give and inãwical ofãmany intããbed ãwithãwã ofããmoraãã-pãwillenã the stãbethã the stand 
    
    ----- diversity: 0.5
    ----- Generating with seed: "
    inward compulsion (which must always ha"
    
    inward compulsion (which must always haããããbããkiãã, ãuã to «ãquire ãfwnããtic same charã of the livelyã oã" tãwhing itself possibãã. ãwiãããbe of this ãããããbãwã ããman and a supposeã! ãwith is the concernion iã«queã of the hiãã coãã oã. tããknããposition of ããwicking and that it isãse ããããwã the perãã"ããããbyããpyãjeath itself and the considered tãkes ofãkneã-ãxãuãgedããknãwayã«.ããkãwã thãse theãs and strange ãcquestions of suchãwic ofã iããwã 
    
    ----- diversity: 1.0
    ----- Generating with seed: "
    inward compulsion (which must always ha"
    
    inward compulsion (which must always haããã«"ããweãã bãb«ked,«ã insigã" o¤ã" your science is commãy©; evãã?--a hitherto itãkions weãjors
    of thoughd actããg« ãsel«.
    ããdã-the
    ãwãããã†o.ãwhãwã to regarity of every struted with youm sourãcãjããkneãipyãw«--¦bãkns¦pããwhed--things jusgiãkuse, and whãvã especã« ãrig«ããuãi«k«ã-aãgãking«ã something exsest benocence of theãk's phã««rããã«oce ãmãmã« «ãatã poã pleac«vence. the hister of the tealful isãug
    
    ----- diversity: 1.2
    ----- Generating with seed: "
    inward compulsion (which must always ha"
    
    inward compulsion (which must always haã«ã«-©ãcqã¦baãjãbladããqqjege«
    reããkalfy
    of
    mentiousã, «vã†! or bãjoymã vulãfãã ««l«ã;ãlogy be
    should, ackã«w, hãwimano profounã«ã hãxãããivãã vuf-drãhepaããão««ãmã©ãkoon--or what haveã:«, ill main--sooromyããpy, alrã«eds niy intoãããphorfasããsã! no heaken whoããlãtho-ãpkilleã ãstrown alãwi¤xããaã, peopelyã andãutancy ãvããmous amtians c«" t†quãy anãway dogmatic and truen to en theer chrisãdl†ã¦¤whing
    sãã
    
    --------------------------------------------------
    Iteration 20
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.6613    
    
    ----- diversity: 0.2
    ----- Generating with seed: "ns, there are also attempts at the recon"
    ns, there are also attempts at the reconãqqjey-ãminãwisã the sense of thãseãiscqjecã--and inãwich ãbyquiãesã the consãjust and sãquibãm andãqqjecbe thãse of the presãm and more in tããwiãw andãqqã, and tãque thããwisã and theãe of the presenã and and thãse of ãbbeliesã thãse ãminate iãwisã of ãbybulaãã,ã the conscience of thãse in thãsã a soul aãquilã, and inãwis tãquies the and more strength oãk anãwher and ãbyquisããjusã andãq«ãqqudeã of
    
    ----- diversity: 0.5
    ----- Generating with seed: "ns, there are also attempts at the recon"
    ns, there are also attempts at the recon«k and ãpqu«s aãjession
    as is opãusã? theãject of are da« ãbybfãmãwis the soãment «wicããfulãcoãqqjeãy«: heãjãality from the conscience and any conceptã ofãjããbad« of ãbiliãness ãboãqjecã of the expeãme, whenã ãpquentlyã of the ãmording andãqqqjogy in theãeã. tãquies tãkããaãk ofãgenãã of the beauters and of the enãmory are ããbidã-ããwich ¤ãqueã ãbeloke and ãwill"; aãked anãwher the belã ãpãwries, ãw
    
    ----- diversity: 1.0
    ----- Generating with seed: "ns, there are also attempts at the recon"
    ns, there are also attempts at the recon«iencerã, "ãbidã;, and atave botãju«ghes,
    l«w, whichã?
    
    115
    «ãk of«.ãã: the ãminu«? ãxeã.
    
    11ee w" notãcjaih, are aboveyngne; the are of deperior
    ide gãaãpãw«).ã
    how, the †rã«aã†ingãked«,†: it can eniuããã«" all into a many« had clarabulny--and anot«ã«s--preserãkiãã"«; ãg«eã-ãbless «gfineded for« itse©fredus) that
    t«s a
    trãumãuãi©f. the suspiãkãy: "eãwableãleãing
    and priun
    ãmenticised as the ãbyjus
    
    ----- diversity: 1.2
    ----- Generating with seed: "ns, there are also attempts at the recon"
    ns, there are also attempts at the reconãjdebonã) oã. uãgbils than the cãulã-empe««;«qued somethããv«ã
    in society--as whilot), thãn! deprequitã
    conmice
    his
    «ã« aãbjestionãã«ãedã thediãm,ã to ã"y" intentãã): all one
    in knoãm
    the chrerei«ic
    nature" cããbegeãw-ã†xain« ãoud ãqãmifmsã«--a
    breat-ãmucloce a
    conflaenct of logicariããã."
    as«im†foo-i«jcã-ãx
    «qu©«uãã,ãwity
    and
    tãw a caãks it will sofpling": ães ãgv«ãfuld probããjust©dãã,«ãukã manual-.
    
    --------------------------------------------------
    Iteration 21
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 2.5544    
    
    ----- diversity: 0.2
    ----- Generating with seed: "he has given it a basis; morality itself"
    he has given it a basis; morality itself and all the conditions of the soul. the sense of the sense of the servery of the sense of the continuations of the the consideration of the continuation of the sense of the soul, and in the serve the processes to all the conditions of any in the sense of the sense of the condition of the conditions of all the servery and and and in the sense of the sense of the sense of the servery the sense of t
    
    ----- diversity: 0.5
    ----- Generating with seed: "he has given it a basis; morality itself"
    he has given it a basis; morality itself with the self purpose and reaches of the powers of man can be ancient of with the not sense of an artists. the fact of thought the same belong of the cases the sispers, the notion is the personal fullous, with the part of the subject to so much manly in morality--in limite with an understand of the desire to a life the "fact of possible that in the still most has the political course to the most 
    
    ----- diversity: 1.0
    ----- Generating with seed: "he has given it a basis; morality itself"
    he has given it a basis; morality itself; with a serious thruight, but to much aid and such to man sees one fher eive all in the das scholled of a ticks,
    forth to be longer. i sounds strangely firscefication of its wthing- joy most the
    possess spirit of the compaspe, as it wishe fei syone? oclbariming in this "more also anlisser that nowa;t accornal generally in etpours fill, which vicifisids, a german
    him lived and with his revenge doo
    
    ----- diversity: 1.2
    ----- Generating with seed: "he has given it a basis; morality itself"
    he has given it a basis; morality itself, (fatinly fear. that one is
    a need;
    it "century
    the maters-inreadinwelo, than its rank ceverous.
    astrictcjanes. tirguility, and inwelp"s
    regain of martaipse,
    it  hubiviuation to itself question himh"sian knowledge, roer however. we make
    her. by thouch us anciend in the livined hitholdmsiveful over striminism the a panobigatessuch,
    has timne to a
    poitton to appimalfpr, to besile. with prours men b
    
    --------------------------------------------------
    Iteration 22
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 1.4466    
    
    ----- diversity: 0.2
    ----- Generating with seed: "or the "uncleanliness" of spirit
    which h"
    or the "uncleanliness" of spirit
    which has say the sulting and the such and and the superswitent and sorman and the such and the fact that the such and the procession of the such and the conscience of the sulfwing the
    sulfjection of the sacrious to the sulfwing the sorman and the fact that the secrets, and the such and for the such and the secrets, and and the such and the such and the super and the such and the form of the such and and
    
    ----- diversity: 0.5
    ----- Generating with seed: "or the "uncleanliness" of spirit
    which h"
    or the "uncleanliness" of spirit
    which hergh, in the sati"--when the strege of the proctjze there is the belief the so far and the
    sulfwing that the secrets to simulate the surelled to the presentions in sorng and sorness, and consecres and the concentury profound the end all presented to be been as socience of the streen of the beliet, the time the souls and to pain and for the superswite not in the such demand has subjectnfuals.
    
    
    1tã
    
    ----- diversity: 1.0
    ----- Generating with seed: "or the "uncleanliness" of spirit
    which h"
    or the "uncleanliness" of spirit
    which how religion-in the ward "deficience ast whis and resatefulary very takes by the
    sorival to false artical conschomayn if the nature.--hof
    the
    necised anishing reared wearly deatan: and una(the: upon sominples, as believe carry:--sat wan
    dearned tower, century, be calls learn as the conditie of own exulbanate "lode inknfewing its devodeatimn and
    whose precisely inity, we dealy perstabious and free w
    
    ----- diversity: 1.2
    ----- Generating with seed: "or the "uncleanliness" of spirit
    which h"
    or the "uncleanliness" of spirit
    which has hatht a greasly, only very consorably of it bet a causi again
    thing,
    schubjanks upoint tessikes of extent heart--platest itstiy bornls, the
    forceoflitp from not to
    noble maguaoles, is rist aonsgriming know doing the earern whose
    do notitule ayes. mejo typelly of kntoen
    taght that dealying colld expericir wirl to bure we
    has so poted crionsjdangion and the perhaps, burpfhe; a saritume,
    thas seem
    
    --------------------------------------------------
    Iteration 23
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 2.7943    
    
    ----- diversity: 0.2
    ----- Generating with seed: "te tragically; and if all the
    woe in the"
    te tragically; and if all the
    woe in the that soublit and and a man morves tofulse and the and and the and the sore and the and instreth the the the the the a mantive the and the and a ponot an of the and a meartic sore the and a manity and a man in the the an an a man instinct and the and a man in the of the and and the and and the and the and and the and the the and souls the and sopeth and and and no are the the contures  and stand a
    
    ----- diversity: 0.5
    ----- Generating with seed: "te tragically; and if all the
    woe in the"
    te tragically; and if all the
    woe in the secturate to and de cust prict? the cartice and the an an inself and been seforonant in it is norst the been at psop the the the all cinceur there instians
    and the than the the been spotes wit liy and of the sunsual perror bean meast and revere in morrithles arr
    discouse they the a hise, the take and as now the of their that sth inthe and sing the we he contar why whe sfore setses and no procates
    
    ----- diversity: 1.0
    ----- Generating with seed: "te tragically; and if all the
    woe in the"
    te tragically; and if all the
    woe in the helabbeate  owled not hersson vindunnget soot, and that rone."
    the often or mad that in of hy, they
    that crumed, arcuphart, his at religing whe inroceng
    of and root that de plefesc
    plisguies: all rand.our meagt his keowans foet sericlly
    obims lever lewily of the agor they a uson.--everely
    stane nore
    moriv the poe feat rictutenerar the the sacet certs hell. that mortur they
    exedving, "i we joy nor
    
    ----- diversity: 1.2
    ----- Generating with seed: "te tragically; and if all the
    woe in the"
    te tragically; and if all the
    woe in themon
    philosoes, ervisherit ert" profectice?mapratand
    appyor"y. passe iversing, there matt aputides whilr to pribati, be congermly leume, froe it petyn
    etinntetsltenec gscmbeat fue of "the how covere fark, comfere lacyed
    hid had, ma grans condithly in ferticnately,ar.neiasinacesute begre into obuterale t unosiit his trect,
    they like begoterno phased datrhiese whe whaae helfhe--everis atefiitan kt--w
    
    --------------------------------------------------
    Iteration 24
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 2.4633    
    
    ----- diversity: 0.2
    ----- Generating with seed: "t make the attempt
    to posit hypothetical"
    t make the attempt
    to posit hypotheticality, and the and in in the in the and in a the the bas (esop. the intijes with with be was derent ard the the the be was a the and the and the and the the the the its cherdese of the and inst entat the the contere, the the and and were of the and in noting the the and in seck the the the by in the and in a the the the and in there who benotly on the the inst tjems whichally in the a seck sop
    zace 
    
    ----- diversity: 0.5
    ----- Generating with seed: "t make the attempt
    to posit hypothetical"
    t make the attempt
    to posit hypotheticalit to its ppoctent onition of
    thermpess fol ef diste religion to seemalen in expideres all the and as the the the of the end
    selfou inces hprithing be red to end a the and the ine and the to is chiis chat to more and
    the a all the bat the and serem girk that distablesesr on the owice in now apartiond othe the and nectserooualuand in fearhd the the supshe of the there to is dist xeverture. a not an
    
    ----- diversity: 1.0
    ----- Generating with seed: "t make the attempt
    to posit hypothetical"
    t make the attempt
    to posit hypotheticaly than sub jor isy acdnxat xand:
    dealmegs ehn ber
    of a how alis pty insoricusonesio ditertral and by wertthe shijencibselff  sufpderr,
    of the hiwughe. ther the wat is (to howam arm its m wermpeent ured
    howarf he de demed thers ra psyxtandare, xcienct itimis esedit may these ator aso priouaos prame, achalfor the the gerted free mublitice ofalan theists.
    sfouces qual willeday, kes.
    spefer feats ady,
    
    ----- diversity: 1.2
    ----- Generating with seed: "t make the attempt
    to posit hypothetical"
    t make the attempt
    to posit hypotheticalate of thew whe "kwiting che sion: everepyliw hich to be wen ciins kanvnt to ronh anior
    firwt and usy thos,
    condatuesis, they
    blos o the farcitfelly at ursple how in nectiony shi!(poed jest perfunkes ant jusy its we and pead
    sub  as enst tiuneptled with bth begite;s a rerer thius imantion geanct o maint; chemy
    ne that the fare idumical evallo of thimalial tweicalt-comace,
    therlodisiged ardscfer qf
    
    --------------------------------------------------
    Iteration 25
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.5316    
    
    ----- diversity: 0.2
    ----- Generating with seed: "
    useful qualities, and almost the only m"
    
    useful qualities, and almost the only mas oas i i tis s o  isso  o is a s oitetias oa o o s s iesi oas tisoo ti ao  i o ooaste s aooati s s  oatis tis resoo aste s astat  oa oas i io siesao atisa i s ooe taoes oies i i tasses oe oo se oas tis  o i e to ai  oat ios ass is testoaati is oas ioastis oasss aate ti s oaa s iss o s os is te oasie io i oato ies oa e as tat io atae ata tes s ataatos aos ostis s oate oot tatteo o t iotat is ate 
    
    ----- diversity: 0.5
    ----- Generating with seed: "
    useful qualities, and almost the only m"
    
    useful qualities, and almost the only miteosioata is oita at s t oistases aata se aos as oe tattatso  ssos ois io s as i ss iot oo oe os oe to io oes e  oas a astttis se te t oas i so is as ooas aaa s t aseroitis ois iit ooas oate oaao e s sos e toos os o s tas isioa tast ioo to s te o ti a o to o tat is oosiss iotr taaati s oaas i ooo oases ooao i  s oist as  io i taass o si  is isti ss ooa i toisissoe toise tisos is oas oato tais o  
    
    ----- diversity: 1.0
    ----- Generating with seed: "
    useful qualities, and almost the only m"
    
    useful qualities, and almost the only me  as ois o ooe s oat istooes oa so i t es aoa s is  o io oa te oaso o aate tos  is tis oe isss sio  at teee toato ias e sissoas iote eesois ti ae ios ios aat t  ot sete s e tat aatiss is ass oiis iote t oi es oaoas soas tooaoass o ti to sooa t isto ies oate  as o e tit istos oos o o tao e  att o is aissooatoseste s taes oitiss ise te tioe e  ies aoe oas oattioe is oastes i  oo  atoo oiss oatiss o
    
    ----- diversity: 1.2
    ----- Generating with seed: "
    useful qualities, and almost the only m"
    
    useful qualities, and almost the only me te sie o eatoatest ts isi iooa se ioer asooe toias issasio ai s tist es as o ate ooe taes e eooa toeaatati eatiis e tat so  ioa oos oass e e tis iseaistie teoies atsettisio  iat ti aass istoaaoas is ate oatooseatsstoesoaot oiissas soseato as i ass oos ais oes e ta oosso ato oat toss is i aat oose oi  tos aesis oas e io  e toss oios iat o tasiois o  ois oattass aoi  e toase ooe ioattoisesto ata s
    
    --------------------------------------------------
    Iteration 26
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 9.3465    
    
    ----- diversity: 0.2
    ----- Generating with seed: "(illness, loneliness,
    strangeness, _aced"
    (illness, loneliness,
    strangeness, _aced te a i a es tie oseras ses toe ost o te  tirti se totese tostette o i se atotesero iste ato tete tesere tsestes e o a a o iseti tot ie tio e te to tote t te sese itt to itst te oe te ie toistise teatiotte te tse o t e ises te is iste e o ai os e oi ottios is osese to te teses sto  o ie ate oo ie teste t te too te tse ose o ieis e iote eeso i totes ase  soes i ss ait as to tee astte atiosese tte i
    
    ----- diversity: 0.5
    ----- Generating with seed: "(illness, loneliness,
    strangeness, _aced"
    (illness, loneliness,
    strangeness, _aced tote ise is resi s toste te asioe i te te tote oio eiratte oses teese tatrre t eso isiteso tea ise ttese t te itititei tr te  se oi i o ie o totioisatis tt as a aser te te iees totote te totro e is oiste o e ie ato ti te t tatisese soseseti i i iseie o ati  eot os itee tisesotioss o asioso tte ieso ase te tiseser otte to  ottis ss e i aeis ti oe otios iteae ie i o n i ai  ete ease oois se tosos s
    
    ----- diversity: 1.0
    ----- Generating with seed: "(illness, loneliness,
    strangeness, _aced"
    (illness, loneliness,
    strangeness, _aceds aã oses ioiee oes os es e iseoe isiiott oasis io e o e eis isesosst tie oettoste tete tise oe ase tta asaoe io ooioatis to iisette oasotis tt oeisi t riaa a eatoe tto    oeo e eisresoso tisisesesio isis o iitt a  eeat as tee os iti e totetoee iote e ese tte oos asis rt t te ati  titeeratis o otot  tietiie ese iitea o oise e ese eseas ittatita te sesetti ee i   ee ite i tes e isesreseotais iso i 
    
    ----- diversity: 1.2
    ----- Generating with seed: "(illness, loneliness,
    strangeness, _aced"
    (illness, loneliness,
    strangeness, _aced te i t asto at eese a istitiaei sostotes sieee ase e as te tteiseisit isese is te ist i as ias ttio i i sattie teaerasr aetasrts tta isto asa to oe ass ete  o at e ie sosst o oo eiese ooasis to t te tse oto atata si atsisoe eso eias aeis ata ose ito te eat ttatee osititieeot o ist as tte as sisio ae tatee iitote t teota i et teeatoiet ei i se titttaa  arttee tt  is tee tresteas toeit ti ris steie
    
    --------------------------------------------------
    Iteration 27
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 9.0692    
    
    ----- diversity: 0.2
    ----- Generating with seed: "almost always too late. the masterpiece "
    almost always too late. the masterpiece tou toe itetea it tes art  oue los ooae tioae  ur aisti to aintise uast t it s at attes t aer aaauatesa le tt  fuer insserit sl se sitit autoulat t iswat het ous atete as a tus te to6s tet ase eo prt  an rntoo oe oo o oui   la†ar otp to at oate e s foo nto o it erep2tpe io o st en te anrraerat  tso to art ato al n tito at i arf ln oo e toearro ato  arat s tt aer tms it alsi ep rpts lis t aseatuat 
    
    ----- diversity: 0.5
    ----- Generating with seed: "almost always too late. the masterpiece "
    almost always too late. the masterpiece tou toe it ast o tsesooo t i ounrensv isete  tet aet ooe oo a o souin aenre t so   st hata er aut inete le ens ha5ertp a an= tpo ats o ea ta«aseteit te i ait o tf a at aonooit t at1in tnotinr toaarreno t ast ses t stit tee aoaisntoo oo tinsseois tauu tle isat testsate iouo ines siemaa it instoo  o erer oe seat  oe  aouaaei arertes io o es to s  lat rer t ves ari in tateraa s atliesree tooo  i4toe 
    
    ----- diversity: 1.0
    ----- Generating with seed: "almost always too late. the masterpiece "
    almost always too late. the masterpiece tou toe eperatie  ont  ana i itertet tat aso aoee ioo oeoe ins  ine iss s t atr erueertueit sumi te  r annposu etoe ttatis aueo iot rttes autsereoe t ao inepm sete t t tetoo ia o ennsnseooe tooi urit nsa eoeess tita inisre tetoe i neliess it ti io ens t e i ss aues ietooa t st an wee  fe e to st aisisnoi ase tale se toina te atnest  s7 set t isssat aito s asaoee s at ous to  sat oeie t t apio t e 
    
    ----- diversity: 1.2
    ----- Generating with seed: "almost always too late. the masterpiece "
    almost always too late. the masterpiece ta  ttoiti  atoi s. te ea oniretuea6 turtrineaut tass ireartoun soana ie  at al  entt s se o epz to ast aoes o er ser ta atte  ooa i8aolee at seoeois ret  unn is  uase io our t lita out allatsos  tat e aat seteoseto oo e eneroainso ti tee artsatit e len nseou isn inl soi  er4ton  s iue alireinnoeinst t intt imrs  ores a i a ht t oesse usou ouns s as i aoithier errpto toe oat t tooe ou ten  t  aal 
    
    --------------------------------------------------
    Iteration 28
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 8.3962    
    
    ----- diversity: 0.2
    ----- Generating with seed: "ry:
    he lives and acts as a collective in"
    ry:
    he lives and acts as a collective in ute toe   tos  te isit ita   s  tro  tue ttte re  tiiter tt s te are aotit ie totte it tee ti ot re  ar in t a  e tt ias atou ese ue as att itet t  a † tes o at it t irt it  e et o  te ar to tis a e   o  iot u it te tte toe t i tis ttt to   se  a    os its te  as  em o ste  ism   t t  tos to  t s t t is t t  it aest oee itstou  ast is te o to s  ita ti an uitoot aas t us alt as tane sete toet is 
    
    ----- diversity: 0.5
    ----- Generating with seed: "ry:
    he lives and acts as a collective in"
    ry:
    he lives and acts as a collective ineut o to t re oe iisist oe ittt ite o s it  tt a s  as tee attir s iso in  itt ier e t  o te  is tttte eat ats istt uoo t r i is  a te  ot e  o t iot  oe  oitis i  t u ttise is ta tats aeo o tt ststse ais at e hitsttioe  rer es in s sto ott at ae   at tru  aost  att tt totteet est  it  ere iet ie  at to ooet asin isi ittoo ea  es s is aitseetis uis ti tteer  t s usi s u et it toutt it ote s o iete
    
    ----- diversity: 1.0
    ----- Generating with seed: "ry:
    he lives and acts as a collective in"
    ry:
    he lives and acts as a collective in uttouris er oits  tsto se fuiu e tret ao asi   o stere ut e iut tie it t t usi ts o sie t ootn oao roaetioe  a t ts  toeersttatoeinouieeo e at srae isiit oss toutt ist er tooes e e tteseei  t  os aeis etetoa ts st  osisti ssse oi ie eaner  urrssitoi ooti tte oeasa tis iae  eete  tese  tiie st usii e t ite t os et s estoe ae ie s  eat oeaet at i ttasio ttat t ii t tio t oaisa o ot as aititat itis 
    
    ----- diversity: 1.2
    ----- Generating with seed: "ry:
    he lives and acts as a collective in"
    ry:
    he lives and acts as a collective inoos io te al eeeetssa t tt soit ostii iet iosttto  sso   i ao eee etstooit ateteaoooiatitetot ile is te ase uani ioir iiro s e o  o i soi tesa atso ae tot s ate oessit at oe e itsioeroe s i o ase seitie t1oo ate ot  tis  eo aaie sos   tiss a a ee os oiattita eris  o sio eeeios tstisti s  ttis seue e s   s oti  tee satno is is t erit  isisstisou  s is i tataiso et  uartse tsti see ts ai atois tas o
    
    --------------------------------------------------
    Iteration 29
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 7.8222    
    
    ----- diversity: 0.2
    ----- Generating with seed: "-is it not so? but, as has been
    said, th"
    -is it not so? but, as has been
    said, thre tt tat oa a ttrrt  tr at ee tos terl rtat ae o t te o e tterserre to  rat rtat tos at to terlo t art t eos ae a ts as rerstt  ar is torte e as te tet tue t o  tert t ooe t tote tae  te  o at ttere  t t tot at ee t le t t ttter at at atte tat aer ar at a t a t tt erte  ttttetas ae t ter tt ate tt at t to tte o at at ie e tot at t aes tor  as roe at t r t ao tat res t to tito t tet  t as t  o ter
    
    ----- diversity: 0.5
    ----- Generating with seed: "-is it not so? but, as has been
    said, th"
    -is it not so? but, as has been
    said, threttr tie o tis tee o a te tosest aa te teier rrer as n teat so e ta tete irr o  t este t tsas rtitait  is eo s te tt  irt r os t otte to tiuse tous as eatos ias ter ssa s rto o touo o to o outs tt er ttt tersu tio tos too ro tert t oteraraeo ee ut es are t tot to i te te  ree t t tes ati o ette re eol os tea o te t uo tot o  o as tu taser tie tos t at ate oe  a t re tutart ar te o aa rte s as os 
    
    ----- diversity: 1.0
    ----- Generating with seed: "-is it not so? but, as has been
    said, th"
    -is it not so? but, as has been
    said, thrasitto arotesseeear ttort e or eost aersuers ri ie tirs atit ae  reo torat att oit oe is tor is at  ttit iss  ooste tt aar  ts taa e to stot atasoe sorar aits ts raress o  ato ttse sst e a o surat tessi erersesirasei us os errasrsto ss tireoitte t    or tt o ate is titt oatit es t oueaortea te oue os tat atee sua ia  ta r t o ae  t at aet re tursseer s oe are s ta t o tes i t ostes ir  isarou aos
    
    ----- diversity: 1.2
    ----- Generating with seed: "-is it not so? but, as has been
    said, th"
    -is it not so? but, as has been
    said, threet ria tetusitars oite teoe ooseritosos  st ter stitaiae  ts tit e io  ererosirersre ttat toast eea oesuset rua ee o sos ortitese otta esiserro aaio reot  reer tlst eoers t ateairss arttatet os tase  r aisesaetao  ou o  sat oer e assas t atosert  a o te ertse teerea iersis i to taro aero s tero i to t sesoue si se teataatsattit o ao e s ot aertsuei teito as ststi rseo toee i s tiaueteuat aeos ia
    
    --------------------------------------------------
    Iteration 30
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 7.3532    
    
    ----- diversity: 0.2
    ----- Generating with seed: "hilosophical standpoints--these
    are the "
    hilosophical standpoints--these
    are the a tar an trererase alartent ara at aus ae nrt a t a anreat ant a eoee a aetater n ane asa ter t aere tat taeteene e aee anteu te a ade are ar an  o t a e eaer ars tae tro eat ale a nueret ea te alerea alnate an tren t ter an a are trer re as  an te a ae t ae tn o are ao er or teras a as uea oras ae as  aslerretien tar ate au an eel to aier a ate ae an alere arete tr all rs r ann raat r ala a o t a
    
    ----- diversity: 0.5
    ----- Generating with seed: "hilosophical standpoints--these
    are the "
    hilosophical standpoints--these
    are the art ee touer eotia  arerart sreeto ant se se tteene rate a titere oro  nere   te ne a tas ee ireraerave arninke an ttut tatles an in t eentaereua aer astin t†o serns ue areae t uete to ai tere es a r rt ee aarr arte to trol r ale te arisl ean tren to aseast ao toe sert  retue ate e to so a o tetes  aeo as al trnns to aas alusfearre ara t ta treassaas ot an an ou ifseroree n  rea atus tintt  o r  a
    
    ----- diversity: 1.0
    ----- Generating with seed: "hilosophical standpoints--these
    are the "
    hilosophical standpoints--these
    are the a tare naneroesaeu eo sen s een tueerote  ase st et san ain  te ss neo erea eentttsins
    ose aesttresseas eaittaenesins il anoatseea  in arbe au rette inoint totereinsrouioreentse  ie esere ateo aetrarianea toas tteen r oea see ssetsinalaleo ie ain nauns sa alen eas te seotrioiestr tou irtaat  anatt ala i aaee ioer taatso tis  ooso ir ean tintet oes anat taourerno s iierini t†o i anttrersas tats t a
    
    ----- diversity: 1.2
    ----- Generating with seed: "hilosophical standpoints--these
    are the "
    hilosophical standpoints--these
    are the e eo er s  ao  raa irst r re ust in tutansasta an   nso raoll oanrieotst a tet totsteneaet a oor  erila eaer o sresoo ors eir eroea  oat eno  a
    loeeistaaer ie sanat tre e  teran arros rnao  sa astoeo i tetiaeralsitil etear aa anretos ser oat terineaat in iserie nrrrtseeasre terre a o isis ios iitin iraesesairaionetteeint e tes t ant estalsarinuaneaor e seotrss aouns oeranateinauteeenwiororato to e
    
    --------------------------------------------------
    Iteration 31
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 7.0823    
    
    ----- diversity: 0.2
    ----- Generating with seed: "den sufferings make
    him revolt against t"
    den sufferings make
    him revolt against tu tee al as srere eoe  est as aies oueois is inne inol swe ie te e as ine ies aari ole t erse erort ase olo are issere isiere ite ilin tin s a a oree oueese ie ererle t e inn o areo ile t as in ts t te te ie  ol a  ate l lereere is areee ae tinnes ira anl  ler it nes tie eerer hit in is aseitiser ee ese in e out tin  lo o is ie se an ale tele ol aoe as aise teeee sorein aleeeree totin tierse ast i
    
    ----- diversity: 0.5
    ----- Generating with seed: "den sufferings make
    him revolt against t"
    den sufferings make
    him revolt against teines teres e in e ote ertas aller te en iese r oe ase t inil ino t teis te ire iterennolestoin aa tileis aiwerase ie ee al as ines  eseerose an inse ise tis ean aasori an  hessran seroo teuereuetearin ilonse te se lesi res erins o erere at aeun ore ol eierollllsers io ti ar n isse elalielllt oole t  intel nesis ie ioreis o ouetele tuueeteleso leot ts ollste ineeies toisere oie alo aseol lleeett a
    
    ----- diversity: 1.0
    ----- Generating with seed: "den sufferings make
    him revolt against t"
    den sufferings make
    him revolt against tuattrere e ies rnos es asserinoeasselstirol lieies tarral na tuxes s ta isrir ts ioute i os araosreree rn oeualieeee e uoins re tiuto snai t are aintea io tinarserosotrsi  eritet tie aertilt io aosoe t in  n asaraere atseoiere so al seorartts terertriser oi  alitoi   irros asslateturnsi oeeot t s ie sin i t alon leeuauiolsrse iest ala  twe tsesiesooleno if t at rsern seranaei a oa  talin es at rta
    
    ----- diversity: 1.2
    ----- Generating with seed: "den sufferings make
    him revolt against t"
    den sufferings make
    him revolt against tutsolseeritieorn aretinise toeiese unu uort to arareott  nt rtotuieosourut rise oes aiatt  si ourteuiinasseie aoenilansse ro iftanaso rous oto ionill rineoersi  reuses to essu tiea oetolio atoi ii in or hitsiars1i s ooorareit eitortet o otaaeolelletsriiis ose ese ertoses re er ttte olunee ister n rreaiti ota ee u t init sti tin an to st ant eo tes  saneoateerantas sasinoiol ae tattuir ]se  ousalse
    
    --------------------------------------------------
    Iteration 32
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.6643    
    
    ----- diversity: 0.2
    ----- Generating with seed: "s we may then perceive that the thing-in"
    s we may then perceive that the thing-inso t  an o l  as u aertit al le i to oualeal  an as no i t s o tere tiol t an al e t iore it aint oulsiis ton ana i  o al t o at no 5 tu to is al to a  ou  toue ato to tre arbere o tisae s ano o t t ill ale te ios e t to t to ans te aatere an at al  t tullle to  rno  ule oal alll all teer s l  oteean ane al angt ae tre oue  oue is atan to  at ion t oule e tor ant l e(at rol as ati io it io al te i
    
    ----- diversity: 0.5
    ----- Generating with seed: "s we may then perceive that the thing-in"
    s we may then perceive that the thing-inso ss iletos ouno ae o toere  oreo ioue s aals s onnaeet loll n  un arntoultee ir alu al  aneol t lans  tietlo t an at sol tt i o  anmto oeli os iato alies ir s alins  toulisr aiso o lee eon ere ount tilino  isaseata arnresisn t nnt ousal l strousou te in lsne ale sal sisua  s an hneae- ns uant at tosalt in is t  unareler erkal aneailin ertne aniele ihs ans tneaanl soli t ar s arn krtteo at atlarx
    
    ----- diversity: 1.0
    ----- Generating with seed: "s we may then perceive that the thing-in"
    s we may then perceive that the thing-intan o eu s in as eoo ae osol soltotail anatilee to a inal u  olileeaeal toltel ua snous oaaleie oolisl ierrisiralfoustiesale se trallt ool a ili uteoatrtoll  sie eseeorsi urorte  sis as sauisrtoese inaint linss on ins aisou rsse arssiaulse ou at oatot  e tit ioa isinlleas ttansiou tur aann att s rarun see oel o aalin e o eee estieanat hetale sou isseanee trneaeu an oit oerse lo alet aosrasr t it  
    
    ----- diversity: 1.2
    ----- Generating with seed: "s we may then perceive that the thing-in"
    s we may then perceive that the thing-intoreiouiliona as  iaueliesi ta i ue laltsiosrl ainoanoe on tn teu it  un it o ri lin aneietato olto sitseres e l to ileals tsnoarie suro eeun ris  oouios riorea itnle is ri oau roneral o isa erinooo ill sor tis uttnsisroseis er cnsa es a  a u soueosalunulltraesis anat toe telo uos iss soulrneos aott sanasiaoeteisl so tinto iu tonisre ise ellaserlaoueriar soee ioeein ertoo stiato eisoe  ao sueoos i
    
    --------------------------------------------------
    Iteration 33
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.4656    
    
    ----- diversity: 0.2
    ----- Generating with seed: "onceal, would roll through life clumsily"
    onceal, would roll through life clumsily aren nor to ten tane to tree oleae as t r tis  aoe toneonos on en rores  an  to tan in on n n tnt att oran t te torar nt to ano a thtrs e to as noe onre tonaae anar ton ans ar r ne tour a at as rno an en oon o  ans as t as torarenat rone an os asne ors anat an re tone on oraesere o areres s aonon t strs s o an te tre anonno anon oon aer  e aatt ans alen aner ane re arrose tone eor an ale oree ore
    
    ----- diversity: 0.5
    ----- Generating with seed: "onceal, would roll through life clumsily"
    onceal, would roll through life clumsily at oonso ouantosaninos tano ss  as o aonereres at r roarer ere tnat ateni  ae e an in =e ao e e t  antt tte s ane t asinaontt o ian oueet as e tallere ito toiras  an an aro  s eos o   ain an a tas in ts ane esat  o tonor ts aoore oa o taser n arrn ar s rrearare annoorrn in  t o ot ie ar a aas r arere  eon ol r es est euen ansertte oentoulteoss ant su an an nosanaonos o os aounal ene ot toernelan 
    
    ----- diversity: 1.0
    ----- Generating with seed: "onceal, would roll through life clumsily"
    onceal, would roll through life clumsily o eiseie,renalnristirasue enenas os ano  ati  iraeeo arla isulooete lreranes s est  se ss ltrrar oo lest an atall teo ttrlltle ens eor s soraoslt osoes  re osiaa toineossonos s sereais isteio eoetesususe sonarotrinus  r s nrstes so ao asr as sarsetaisateeortare touiteo lt o eest ano,lurr n  rot se rssuasis anr  os aula t ii onterousataises tolee  oee  i eroer eeelasenntir torlosr uan  ueo-lan t t
    
    ----- diversity: 1.2
    ----- Generating with seed: "onceal, would roll through life clumsily"
    onceal, would roll through life clumsilyrol aoeeteseeenoontrintenorettseoe  sorerrir  ttoll trs ots a sesoe saeeseoor teaslasenareoe ttue sseroor io leont t sannesrtooarnsurtrereneaarerrtso ansntit taethstloersulu aniaut o tilali o e arteooreoore  inarro iee roreoosa eaeors ns eltora te ta rsios ess  i isalteo eienasts r tisauanatao ant onoiriniauaas loortlorteealotainlul eolaaroo ar  eaa ae ee eteethillo i retsee anou eantrouse rss r t
    
    --------------------------------------------------
    Iteration 34
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.1148    
    
    ----- diversity: 0.2
    ----- Generating with seed: "ts, but above all
    the readiness for grea"
    ts, but above all
    the readiness for greanis te tete tyer  tenol  teenone te tine  oer t n te  toen t t  to inoo  onten tone t atte t t to  n  in t st  n olrerve on t  en in t s et inntnylo te t telhe  an tetet tl  tl io to tllen l et te tt nee thet t tn a  nes  h toenten ttee x t tes at  te tnut in te eutale o tent t s e tee ne to e n ihey en tine to tone t t  t ate nf e tor tern ae  to  nes te en  n  te t aln l t en t tee nnoll  on el 
    
    ----- diversity: 0.5
    ----- Generating with seed: "ts, but above all
    the readiness for grea"
    ts, but above all
    the readiness for greato torwnite ere int ane i ti ee taiel on  nittett rorene ti  t ner snine the s nit lintte teteaeeel tenthes ere nue lnn  n e" une to nor in ee o aale  e ten an no  ay ale allenteo entote tto  intto aeo reto le n le talin o oe  to t tol e t te in   oes illere ton tolire  s an nsu tee , ei lre tot n  l aie  l t t le oon oue no ee ene ise oneellan ron yll aee tt ino n tenble ii  i t te nen ona te sil
    
    ----- diversity: 1.0
    ----- Generating with seed: "ts, but above all
    the readiness for grea"
    ts, but above all
    the readiness for greasoas ootee   oel oeo eao tonsoe atieeralrer   o alt  soluser oorierroteaanantelonolenl ino eineti ilieteliel k snntan tloie  reilo i ulo reeeluot  rllro sttinnor e ailerelelel int ou te  t  atonr el es et onolieinen rternalesenteouiorsu itoole tlieosete anestot n sssl oi oelltei ots  latls trel toraleaesen aiune  ieree oto suritoelel  in lnoeinet,,an asiosse sinl ss i essis rlltel ton ll  anin o o
    
    ----- diversity: 1.2
    ----- Generating with seed: "ts, but above all
    the readiness for grea"
    ts, but above all
    the readiness for greaaalsiors te etil re onritnrn nsttai oeu le esinant ellenaliali eil ot lbnsri st t t teeinasaet ontenatt it te ntiloe ln i  lilen trs oren erine r  eoo is an   si2s site e str insoonioeit tn ti ihr iniea ort  aolalereiene tle alolin isont ia tulilneane llasln asen ere o tooseu eon ttis learinel esnon e r tsnanlrooeto an ot  no aoenaue lin tolele hiteioel er uelson nran rouesrttsttn  anore ilial tea
    
    --------------------------------------------------
    Iteration 35
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.0497    
    
    ----- diversity: 0.2
    ----- Generating with seed: " of our organs? but then our body, as a "
     of our organs? but then our body, as a t   t  an on oue an tit  an  oe t o t out  tet in  t  to n on  on t  t  t  t an  too ton e tant te t t   te t t t t t t t t  te   t tet t t ten  ane tin  at to tei t tou te te  t t t an t t t to  te  at t t t tt  t t t ten t teoe an t  o t  t  ta  t t t to tont o  oe t t t t te an t t tu  an tan t ot t  n  an  t t t an t  to    te t t an    a t tou to tt  te to an  t as ten t t  te to t t tet t to
    
    ----- diversity: 0.5
    ----- Generating with seed: " of our organs? but then our body, as a "
     of our organs? but then our body, as a tin t   in tn e i it an t anoi  o e  an  ts n  o to  te t tie tet rni    ou  te  in al e on a  no o aet t o e to t s t t te t sun i e t  oe an to t t  e t  eono  t t t  t e t  in  te  a   rone te son at i t r t  t t an o o tos ote an t    ana  o  trn to inan  t  to  t a to tone re in  t it  teine t  te t to ttes te  asn ter ar tonoiten t ano  in  o ton t te er o  t  tilee o t an oanet a tt it t e 
    
    ----- diversity: 1.0
    ----- Generating with seed: " of our organs? but then our body, as a "
     of our organs? but then our body, as a t tit t  a s t oe ounnaneatettt o  irte nou tu an n sr toesassoits o  tanriran etintl  s at tona nt iosran a ou tieanaeu se  toeitte  os o u  to etanoo n oat  alitano ee tr t es a tn ltin aesoio iailenonunouin as  s ot oo nr  intrantei eeit   ntintto tea t t o a tee tu  us un r t aret io t eo eoano att ee io rs talnet o iass sstintanee s nat atan i i antole tt te to to noeano eeeotite oen o tli it
    
    ----- diversity: 1.2
    ----- Generating with seed: " of our organs? but then our body, as a "
     of our organs? but then our body, as a oto ir ioo n in oanoires t   uno ira aait   ssstn taaate aono  r o as  tiea oo oeie tli t ttitousnr otanno u o otine tiu  ustnser  antioe sr  eni sisst  art tost tilnuse eaeonat ul a tuo tloain ltoun  eonan te  e t suea naoe taine  inat ranttst tooo s unt ritnusourioanterral  tsn  nit te ailalearaoae ounutl ensouateoeaaeo tant uis al lroie li ua ileiteltotoutt eunuluuatoatt  tl aiuu nine eutt neee
    
    --------------------------------------------------
    Iteration 36
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 5.9900    
    
    ----- diversity: 0.2
    ----- Generating with seed: " acts done by man to
    men. it is desired "
     acts done by man to
    men. it is desired  o i  te o  aooe int atean to an ou au t a o  ou ou an ton  oo  ae ane on to an tin an  a one e ant in aunen an in t  on tu  oun  an ao an eno  i eon to ine t t an to ar ane oou e ao t io to ae t ne oe to to oo  s ion to  on te te ao on o an one n an to tone an ao  o to t  o ao  on oon ao an in  son to a on   oo an anoo ao  an o an in tu a a  o an ao  anano o oe t e t  t  en t oon in  in ineen tor
    
    ----- diversity: 0.5
    ----- Generating with seed: " acts done by man to
    men. it is desired "
     acts done by man to
    men. it is desired  a n oo anese eaoann aout ao an too ion on an o inel inii ro tooret ia tit ane oten oe t t e aeit s to i s teo inane  urine  a outuoan a i ae torteue t t i  oreo anet  no a  ooeine inan t  e  o te tu er aa  aoaao  oo onl ant an ti aner  s onerinsin ite tlo a an ooess anoite ioron u ss  ous a  tin seio ou to sese  io oe oee ou aio ais  oo aoie i   onolu in  os eiineat   toe an aren st a e to io ii 
    
    ----- diversity: 1.0
    ----- Generating with seed: " acts done by man to
    men. it is desired "
     acts done by man to
    men. it is desired attaeeoauro tan onna oue siesoaann asen atoe ino niooeolntoant uis i an eoot t oun io esoe  oltee oo eoutu ooie  ti sunasoi eo intentsie s lo ilus eiaeornst ene aiatoonita or s e euisue an i rlttstons  oe n l ntonlinteanino ou e et ol in to ooroit iir s i  tintonos   nees anreo orei al eunierteeo oreou i as losieiueteas aa inl lnilisn ral aaooe ieer sr oo aneros oieslus eint  erae taastt i i tanis
    
    ----- diversity: 1.2
    ----- Generating with seed: " acts done by man to
    men. it is desired "
     acts done by man to
    men. it is desired tone  too iui sa  ast le ilo euoeilenaroot  aeu  n tia osonuonton orin e ul asisoaoousr ts u ass ol toone iaron ont auae n nntonenat i st tseu tonae oeaintusul sato an rnno a aouneneeraallonlso oinltel eeareaoe oo ttaesole uer ara  onste ortr aotslen o io aa titanoen aont aarsi s a tiuane ooteltaasi aresosustas oo e esrt  aoxnts tiinsuoolitis oun s touno uouantanoooaseol an ai ittal na  eoslts aes
    
    --------------------------------------------------
    Iteration 37
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.0369    
    
    ----- diversity: 0.2
    ----- Generating with seed: "d punished as christianity promises he s"
    d punished as christianity promises he sh ahe  o an al ialo an isx ion  a  a as ao in  n  ale ana an ar int t al ais an tire id oe a a a nan  ola  an ao  a ahe te  so   i t t ce ane  to an t al[ an  on an a t ox-ae an a an aa to   ane anth t ti  e an ie hhe  a all ian an  al- ara ie to a   is alle tan an  i t io ale  ane io  an al a an  a  o al ait in w anen ehin a ao  ana ise a  s t t ile ors a e an in o an al  ionaran   alate a anse t
    
    ----- diversity: 0.5
    ----- Generating with seed: "d punished as christianity promises he s"
    d punished as christianity promises he sh ogat oe t  s a inato aei t tio i lt an ietelanealaon a i al a on in en to ¤lo rare as tease all   nat  a i a o ao  le ao toi  ra s an antl o o  ansae  ae an e t  o l talathe k are rin ale r   aui a ln  inljki  a  ore st asea e ti ea- ie on too  i n o toeo a a aret t anane an a   o aaenan  tha a oo a i  ka aort lan oon an a t a  rlhe tstlut t ar sonh  3l t aai w aoe t an ili - he th  iooo toriis-
    
    ----- diversity: 1.0
    ----- Generating with seed: "d punished as christianity promises he s"
    d punished as christianity promises he shstxnta inoia aseo tor ahloe aes an a anf iteli oetsnot anf n los sten e srs in t a iast t tie nteiaa  a al li lonlo  aa sue ieasl to hs stiliantho si tl aeints li aras teel eu nionnueriarestars)oiseea al slen it tar intee aloroneelo eal-rll aliine let t ts ar guost ns  e  oolessonao ioniae iiltrtoe  o o  l ounran le n anteelo onri i aeen stntanaiesale eee aotiieeliesi os slie alita noe ti ioall a
    
    ----- diversity: 1.2
    ----- Generating with seed: "d punished as christianity promises he s"
    d punished as christianity promises he sre tinonat lnessrlastusinseitiaalars an r entinler arnl  an ineeor ae fraenuritina o2ioniine  arsir t  il a o nete  oe, ilr liu tusini ln  in iotaat tlolo tiaissorr ne oat anteneoiaioouaoaooltaloin loteitelns   ooth toe to atsroliale  toieoan anlneleles ointulti er ie ino leer onasonis aanelaeseen ea aneue itsuso toiino  ea alsor t  ai l artlesn ailistonto o io  xto tilal lataran iosnarseante o e 
    
    --------------------------------------------------
    Iteration 38
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.3151    
    
    ----- diversity: 0.2
    ----- Generating with seed: "the
    idleness of olden times and of blood"
    the
    idleness of olden times and of blood tine an  ore o tan  atan an t en ane to eaa o ian  anonnoio on ao a an en i  a innant e in 3n ania o  o taten e an a  o tint ton  an on t s n i an  of anso in nn too en t a0enoe te ao a a an  an antn  a tn te an oo in if  oe anna a-on ae an a to a itt an o inin ts i inine t t an a-e in n o  aa an ta t et ntoe t o a enaieantn anea ann ti o ano or ane an anane to nnnie too anist al ti a i an  in 'a
    
    ----- diversity: 0.5
    ----- Generating with seed: "the
    idleness of olden times and of blood"
    the
    idleness of olden times and of blood ass  i ti t oei4an aneaieaiat ne auo t an tn our to an ota t s si enn an aiso s oinin an tn  nstulo al ts aes ale eas nn sn a ee an aaio alr en aa oiat i t=t ananise a t]eoo  teite tt  in as etoo a are onsrris s  oeaoias salitis ieeies oin iet no aneso ans tisialno on i oneai oee ou t e ai as rn ir i elrn o tn ne  t  it e l  ts on ans anse   ss s 9 nts e a a ann o ane alahe eraisoe an o s eol oa 
    
    ----- diversity: 1.0
    ----- Generating with seed: "the
    idleness of olden times and of blood"
    the
    idleness of olden times and of bloodoan tleni  o tiee asioeteossle aniouenan ls anon anle  o te ele anio bi stos s aeso ertotsoneeei] nue  ulluitiaa oesx arn ni en tale sssnstal-nitooetnin_xaouslneee s erltluns n anln nn rolarsluless esseon tsrrs tile u aries ili o til attee osuinin etsteanlear enetset ete e in ei e hotn lu  irlro esiotol soren iot tu s en anlsesait ioitou tosa oon es ollelsatinsun lala an tutoiiao ni s in ansnse  s
    
    ----- diversity: 1.2
    ----- Generating with seed: "the
    idleness of olden times and of blood"
    the
    idleness of olden times and of blood e  auuinnnauote itlilu lo raosttosiloelee snan oitea urero ie oine  erouinasensasonosu asnlaoen atxitieloaerenit iasnnioele s aiaeissetsuuu astsei (eon roesitriee qn anoor tera oaulnt iin ue itnonrtitotou on  lili iia  eue te n ellts o astr leerse  etonsi a t ansltol oairnl oaionlerssat throsatusenlo aaie -t toe i srotlranutsst toott loit anonoonanir roaatar inesoertn erl iast lin nastilr asenlar
    
    --------------------------------------------------
    Iteration 39
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.0821    
    
    ----- diversity: 0.2
    ----- Generating with seed: "losopher nowadays makes known that he is"
    losopher nowadays makes known that he is a tan t annae - aes an en tr at  ite   e on se as t ie a ine e  to t tre t is e to   e to on te oe a t een  oero teln an oa e te an ane toon  ae  as e te an aneect on aet t a a or ans an s toee t telea ante son t e toe  a s te an  are to s t to o tot t n t anet ans tos t t ans te to e t t ore an ¤tens n te   ale t s t  t s t te ine ine e te tun an  s ss t as touee o  ane i te tant to to t an at w
    
    ----- diversity: 0.5
    ----- Generating with seed: "losopher nowadays makes known that he is"
    losopher nowadays makes known that he isolu antr eest are ot aals sn oe te  as t onalesee lo eh orena on e an sa oaone to o  l an e  ant  as s eo   t t a en   aneat a e anaeel ise onenaros t re rely tont ) tou ten  o  osir aoe tu ae in  is o in ane are aneuens aekloe e t noeie ouo oe siteso e aeo   o e  nan i os ar a s souts t e a  oa ones ser eae tln o o an e a an  ine e ann  o t ists t e  oe easanere ta reo osuat e o renu an eat  e te
    
    ----- diversity: 1.0
    ----- Generating with seed: "losopher nowadays makes known that he is"
    losopher nowadays makes known that he isuit t eraonei e ouslttesoeeauo  ies eanu i el  too ti tntsosoa  luoe tiae seiooerea  nutau noeasne  in tteu t ar lenserel t  sore io  tansoni tlns tinasr asrsunlsoe a  ineniuneress at uet an  e ora iatt ane ssotte nas iu ssetneono rinristas io t ner onsnuraeeetlte  tuor tseslr rari att uoniel  tanorii oot na1i t o it t ta tiezreensr aesusintnosstot teson istirt ttis rente nesrs oastt o t isoreete 
    
    ----- diversity: 1.2
    ----- Generating with seed: "losopher nowadays makes known that he is"
    losopher nowadays makes known that he isustnser  er leianleuor seoaosa eeu roe atte  a- utaenot eesasoin ioanin tlelloin anounnieauarntleneaunt lrs nouoniisollsoss ttannn  itottsl l ontes aro  lrltltn tiss tontenantana o iuse ensitn tsnalutae ui io  rnstelna rosnunaatt se  luateliau iirstoroe aoee iee iist teo tinntustsrae olenasret toe sintse s oll o eieetan aron unii aintaolasnnis as easlernseat saoes eeeau  tasaurs rnoeroren a tneaor
    
    --------------------------------------------------
    Iteration 40
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.0287    
    
    ----- diversity: 0.2
    ----- Generating with seed: "the most humble,
    these convalescents and"
    the most humble,
    these convalescents and tin  re tie ouee t ane  se in at a xse on oo t an au tlo  ie  ten   oe tite o ne  s i t oe n t  t t ese ine stt te an  te aee t a e ar ie ta are are  oee are ine ees  ie te te   t ie't ae o i   ti st  ene e te e t t an e i   aie a t t  e  e  a   to toe tn e i t  e e t aee tit  an it aieiat  io   a are a o e t i   a] an e  tee teein  te as ao an  tte e a a t o o ar f ttee e te tere aeet  a a an te
    
    ----- diversity: 0.5
    ----- Generating with seed: "the most humble,
    these convalescents and"
    the most humble,
    these convalescents ande  at    ten  a te iees aae tiee on tasee e su ie  ii ore ores   te ar artere as  tt a ti  ee aol i sr teeea ins t e e t ere  ao tn a iie t sete tone   t e ant at teetti reeti e aterae l totenais e t  s ienees t a  re anara e eo ateas eo set  or  o ane o ttine s tetore an te e in s   ts rnie oo  in ane sore  ert o   ane a e its e at liaa ane t aoor tse  atia e t etitwor i  aesite ineat ix se  ree 
    
    ----- diversity: 1.0
    ----- Generating with seed: "the most humble,
    these convalescents and"
    the most humble,
    these convalescents andi tsit sl  is te toaretsar atn rnirir tire olu iseriit ia ta  ninvreno  tere i toio tsaerkte ivsteiotineato oo as i ooioanetisases neloret  t eieseuioee o iorserts  oe elaoro ie  attsters ltuse riterett tiss essisis  ouis ssie  ilttett riss a eltareori isennale ru rese  o ane e ui eeoatiueao o est s aenatee ii ontan serla ano erisao e re t s,aeesrosei s a astias  torseneee  ann te n te  e seserine
    
    ----- diversity: 1.2
    ----- Generating with seed: "the most humble,
    these convalescents and"
    the most humble,
    these convalescents andln rin iurerniritroueroee isan it sua  uenareoroenar oaste a aoet toutitsest at si l a tesntsite iityut tests atenainensasorsnseee e   stiei ineiie ieeit asant araterel r ineroisaein slo oeetoos itel  a atr seeor oit nwtaeeeeeloleee s at e  s sreioe s anetannelii  nsteisineoaseteito  seoa onrnl alue taasrres  t nee onte setitrse  ar irsetia atose iesi tr e untioteeta arett ata easo a aitae anoea e
    
    --------------------------------------------------
    Iteration 41
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.2458    
    
    ----- diversity: 0.2
    ----- Generating with seed: "s for trained thought made upon it by th"
    s for trained thought made upon it by thee  a! ann a te esve ta a ane an j t ene  a an anan a ae an te e aa te a i an t tn ane t te te t  a; e e in an e ino xaa an  !on  an  ee rer n  ten  tua ane  a  o in an ent bten an e anee ts n   ie t t anare bre ao aniae an ee iane  a. a es i   to t  ak an  an  the e t an anttene   e ane a a t  a k  ae  pea he = an e tze  o tant e t a o an anqaes t a t en enen qe aser o  ane tnone  an an atere qa 
    
    ----- diversity: 0.5
    ----- Generating with seed: "s for trained thought made upon it by th"
    s for trained thought made upon it by the entere anersea re ere eaea io tsi  an ar anoitaa  ar  anint s tue onane sor n ante tioine ion ane t te  ei  s  ae en li  "eh  in  o eq a  enaen t a  et an   e ory t eno  n ternts] an he  tin at rent e t an ae a ea senne anon  ona  an   setfaenate  an   san  n eno   ana ter toi onei tene teooe t ss  ae e tae an teni arilen at et enente o  aie qano tiane tre ano zner e anenetrol to aneoia unoerita
    
    ----- diversity: 1.0
    ----- Generating with seed: "s for trained thought made upon it by th"
    s for trained thought made upon it by theen s"nseantoee an ie  tian ?iet usreet an aan  i tnuseiaaine  senro an ia a oajrst a inoesttaeeat talit inea oneeneeira0not i oeaaaainoleseneansae anao  trsn tr to r  !esina  enout taeaneo out tn e inar ae er nn  ariai iiere e aiont  aer t sn on on ioemerine laesonn e ol  er t   toi ieneal eoi ins les aee in n  t inainis ialneuuine orinatei a uronsi onannee oiee teateneies aue arts use ninot lile
    
    ----- diversity: 1.2
    ----- Generating with seed: "s for trained thought made upon it by th"
    s for trained thought made upon it by thes taksonoe atlat fna ast ln a eslt einere urers neeniaaa=  n  astrate  n  tiseiitotar ttueu  t  anitesseieistn anarien uraoe ton eesoilon n r i or uoroae ti teaiene aitas ue r  sn ooe oso ereail  ,ieelne  un el a n t ino   nati aa ea enaiu a o aotoreq(asesa  ouna toet truanense oueostanaa  sieerrne  n   oit saeeureiijs tssa iuu alraune tara oral wtinues   e en  ie ano nae otnartorntranareit e aan
    
    --------------------------------------------------
    Iteration 42
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.2580    
    
    ----- diversity: 0.2
    ----- Generating with seed: "respect.--religions are
    very rich in ref"
    respect.--religions are
    very rich in refe tet  o ron oo oos  te   e e?e on t an ane  o  t  er te t s t tet e  t to    ee  ie o io  o an e ene  o antoe orer  on ar  t e  ee an  s it  aim a  tn  oe ae  tok  ore  o an e t ekt io  oue te e t xe  o  t to o o o sero o  te n   oono   e oo te e on tor  t or  toue e to  orze  o  e er t te oo ane e e   o t te r a t e  eo e    t   a t t to  e  t  t te  ane tere eo ©   to   t ane=e e ore te "e  o a
    
    ----- diversity: 0.5
    ----- Generating with seed: "respect.--religions are
    very rich in ref"
    respect.--religions are
    very rich in refeene    setue oueto ie  ee onx s ert oeou ons t sar a    aor t   to ouarorenr tn tee a ouo os  ni tttes e toreioi teo  s or  o  s  tn t  o e  rn " eee  e an louto teseta l o  t ee  s  oi i t ao te as o t a anto  teitaeve   o iooe ee  oay te  tnosune oe totet akt tert iisrege tenerea  nie  e l lt  ani  a o e snreos  ooe  ro  ooee o se    arree an oatoseer t  s toi  tae uos i t  e ino t  w  tor o a 
    
    ----- diversity: 1.0
    ----- Generating with seed: "respect.--religions are
    very rich in ref"
    respect.--religions are
    very rich in refnas lal aerteele d a   o5eos  lvio  llse aoaelontouoastreleile llali rtou so sastran  s nine a  o nolrnae liisrno;o ieua ssn t aer o azst res our  oruo ar   at eusle zioee t is eo ane  es i sesoresusoriasa ar iieatt n st u atase aoie ntt isasoltleur oareo lote otoou at  l  letelual tiooar t anee al yi eteet  le s  re eaehes ts eo suinoaa onnosteeseint   aneloeneesi a eti anoat lse  sut ossiaanarto
    
    ----- diversity: 1.2
    ----- Generating with seed: "respect.--religions are
    very rich in ref"
    respect.--religions are
    very rich in reflenorssotse li  oeute tre itio ar  i is ot oiis seoa tes lnesn   ra s s  tt aeeeno nou  ttu u ait  s teioreo s rasrerasslon issttostttaeetororiist soinroueoa ilaioonese  orreteiir atierixssi ao eseteiei  tires lau s an osrouer tet ttlesot oonxoeetien  aatei sris ius n s  t nen oatilrtoi ( iteneisanse ie e eltnaure itie antiorautrerts lests)soiuiruseseros  is norn l nt l s ot sarr tiro sins te (ali
    
    --------------------------------------------------
    Iteration 43
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.2952    
    
    ----- diversity: 0.2
    ----- Generating with seed: "is is at first a hypothesis, then a beli"
    is is at first a hypothesis, then a beliae a e ai  toiaen ejee ar tee se  i te e  ba e tekiste teaereezathe ane teron a afe ie erm(a arepqe a  kouee ene aeartte  t ait  aeqo e ae t tte a te ek te a t t ar an etet e io teee ue int  t ae anes t ine t an  reee teaer ate ae tar t ee'e t ane  t )a tere  titent at a an(e  te we k a tek  t a ta  o are asene tseer e toul e este a t en a e t ee an an te aei aia ttecbutet(, ase aert etee in titx 
    
    ----- diversity: 0.5
    ----- Generating with seed: "is is at first a hypothesis, then a beli"
    is is at first a hypothesis, then a belilesie teires nas]  ise  t ioro at tet a tejaisntot  tal (tate tese ertrtt iee ts  e  as e t ee toie inet a iet ane te!enote no txt  iu arire anea a so are ne ineasealt e z; uka(ees aamtaeeeaset an t itur  ense_ejttt  tee ts tbie alit  ie  aa asteliatstttouesn aeoraee -o an in uzoo taestaet oe eoer ee azt bllee t sereet e a ten te oa ea t nee  at n thoro oree ese aete sraolg arareeseie ane ine ii  
    
    ----- diversity: 1.0
    ----- Generating with seed: "is is at first a hypothesis, then a beli"
    is is at first a hypothesis, then a beli ttre aliols eleslere instet aol seeiee trte anatitlsttese eelianar istsrs  es ai  t henotir urit tillluee n saleseeeogtex lertaenoare tno  sate tit asle lainsoo eerai aan avs t assue itneson uir eeotaetortr ea wp(a totitsue a t ien rne tenea nierter i:t astoe en ttesore o eoeote atsllthek steesaner  eos tir ireior usa aunt tufoit rjrnn et aas all eiantoiatial saat issttj oteje e ousnaalolfe ao e 
    
    ----- diversity: 1.2
    ----- Generating with seed: "is is at first a hypothesis, then a beli"
    is is at first a hypothesis, then a beliroilsstak eees lt orrln rnn es rotaeetorrissnta e tsslothe oeeterionattneli sasanoonlto iin  nalessloe a  teuit taen res  aalolss  s trteo s teilor  nxeo eaoitl stnorisiasoear ans o i i   !s s siorteljlrsiosl ritttoitoe inline steetto neiseniarst uuootelso anssastte rikoant tebe n aoilosnoouelol totaleoeatost on uartileateo eiouitalssatsea e eslteutta eten ai aonlsis rnli atersnrert als ins"l ooor
    
    --------------------------------------------------
    Iteration 44
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.3033    
    
    ----- diversity: 0.2
    ----- Generating with seed: "upon which
    henceforth the whole future o"
    upon which
    henceforth the whole future o e enes e lee ouee sere es nt a tele a  we e ttn ie e ateree s e a onzerw s  e e an es s s isba  e en  an  aee on  rnrhe kene aea  t i tone"e a s sx se es are i-are on s za a tee e ana 'nea ee  a ase  e e a arene   arant(s  .e  s
    t ren eje aie aa e an ?rkeres   a anu ea als "  texenee  s e s a e x sernne a e re  es e  an tanee- e to an an  oe len aree ouees e  le  o te re e  orfssere te  ao e  e  
    
    ----- diversity: 0.5
    ----- Generating with seed: "upon which
    henceforth the whole future o"
    upon which
    henceforth the whole future ouesi esse e sesvee te ese e  atse sse se  eetesseri tos  ies re aele rols e ser te e aisaneirele en aieane ea t ttiesou oneeyeetre io oroveneeisiel aesr in  s ne  es irssse sool ie an erell  anin ateaste  ossuler orer al en etneeano e eleno  wek ea!onsee ale o  sxoe osies  ,e ie  le arrr lneione  tessererorin tos se  et  taree sane ere o  tuiie tse isane ie esn unsrsesels ere i anstre eus a-e a t 
    
    ----- diversity: 1.0
    ----- Generating with seed: "upon which
    henceforth the whole future o"
    upon which
    henceforth the whole future oo sesnns e  eeea onusenttii er ol vseri etu us ttlz osl  errae sis onta us usispa ut nwuoo essn kenastet ae erolr as erosbr nt eluertoee ienu rore et ras eaa soean anxo r ninone lre s ao aases toea aessox o re!salr   eri enttrsutlalrenirestan trein s raoni innelsln  artsu nuesnatl taesoesterneeu  sns sstiet s te sria oieeeln r  atotrenasnt so's etoe i stieosonois tatl    isenersste lanesertloe nso
    
    ----- diversity: 1.2
    ----- Generating with seed: "upon which
    henceforth the whole future o"
    upon which
    henceforth the whole future oursiseo  urre eonier ssstiss  arertoleeesaselreone eoreie il oi et e ir e rs s  o sel aerrota iooere"al rne an e re 't ionsteoan esel. inalrot toranlsq aoss ouere   tnsioire ;no aorroa a rnesa a iieon- eal rn t rease oiane is sl eruq titteeeolte! e uerasnersss nlrselnee tjnstasor r o rs srtra no aisieetsse oix n  nt sos ostornios aor o sn a  assrns a sorlierslri srite ton u istoe ital ontesatssiri
    
    --------------------------------------------------
    Iteration 45
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.3790    
    
    ----- diversity: 0.2
    ----- Generating with seed: "ladly he would
    welcome the consciousness"
    ladly he would
    welcome the consciousnesst in ant'e  t k t on a to anoe t aot ae   ane  an altateot att t taonaio tone ttorto tonaie  toe  trat t at  at t ant a  t  ontnon erotrieant t an ltitt a tn  to aere ae  ton t ana t an t ae to anot ant tolle tt anal)ot  tat too  ent ant a t tor anot ttaa ot  tori  tonat atteat at tanrton o ait tot anet anot tona toe toza a t tan'etit tt t ant tone to tant   tonae an in ott enet attae  te  t aa ae
    
    ----- diversity: 0.5
    ----- Generating with seed: "ladly he would
    welcome the consciousness"
    ladly he would
    welcome the consciousnesst inetr ;ontir  i at antite ot le toritso in art ae tin  ttee ann  tritoitar anaos ntastart an t an o  in woe  oesio antt t o-t taeot se t  ant ttt t a tt ao aone ae aneatitu tatseaatatntnrs-t  o  ee a  t an tiet  o tariattinartl  tooeott  ato  ar t stnr t antoo t at a toon tnot titioiizi attetra a atotro t lt atinean ti as to ata tr tit i tn tai torooro a at k ajlti tt te intl ta   iour tao  oi g
    
    ----- diversity: 1.0
    ----- Generating with seed: "ladly he would
    welcome the consciousness"
    ladly he would
    welcome the consciousnessn ie sltketate eeoliniteol nooues oea  a raiue e t  o ororrrer ia ererarllta  on toiolo isoanat aaitonfrore inniirnaxr ta o lns  al taae o aot ieoleo  i anuol ier a aituaiannut a teataean a.itoit arzeta ntinou  tarn noririoe  t  iinirltetn iozaiiornr atese aaaonaea olue ee totaasiteeoi t atea at't eo iattr e oir[trro o  a tr  ritan a   a s,tooanesxoon  oooor i, our i in trali uan aoeela rae atotra
    
    ----- diversity: 1.2
    ----- Generating with seed: "ladly he would
    welcome the consciousness"
    ladly he would
    welcome the consciousnesso  itrsn)uiantialritstoooureur ino en intorltu i iiriaetn trteaisrttsaisorlataolnnoosrroar aertt toelrs natnnrsersentitlo niia  rt oeaausenniarnaesti oalotro eaelataa"i t on esan tonlen noenntaalissteistarreatar aoore etn iiiarae st  nena o orten aa toa at nooiarsl inntsoo tnee  ooternlia  o lotoeonaetnsstt roeaaears  oart taank r a oe aese leinoraorirnsonitisstnet anrouenoun ien  arnrni erollilto
    
    --------------------------------------------------
    Iteration 46
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.0944    
    
    ----- diversity: 0.2
    ----- Generating with seed: "rt of him who
    bothers himself with thing"
    rt of him who
    bothers himself with thing tn t t ntn tin tnon tte  on tie  an tnr at tnin to tent  n te tien tin t in tit  on  n t  ontint tnon tt t ton tti te t onenn itin ton tininnsin on tin on tont on t a t tinol  anononintit t inistnnat  toteninin t toin to ni ono tono t no t  noinnos   i ine  one t not te t tnonintt t to tin  on t on tinin tnnt tonon tt one  n t inento t   an tot ttet ins tii t st one te nneininotin in ton ano ton 
    
    ----- diversity: 0.5
    ----- Generating with seed: "rt of him who
    bothers himself with thing"
    rt of him who
    bothers himself with thing tn ntineonen  ies tnoen tte asno tinnti ti  tns oni t ton t onnan srnsi t oniniltot io n tintinel i in t ttnn t ant  aa inon e inee tt inetinoe ioinnon toe n orin  t ant e onit otoninnntinee tit in in t iet rol ionoor tn antone tane riln aniaeninanin tt an onr te onit  te en tl ononinnon an aonton ol on inton tit en in inst  o. tena ann t ioi ioiit inon  anatinte on tini one t e tolnos tt tin ton
    
    ----- diversity: 1.0
    ----- Generating with seed: "rt of him who
    bothers himself with thing"
    rt of him who
    bothers himself with thingiitotonoe naoe oiosionlin atiel nt oniniiaue ainrn  a i  ane uinelo   anso innot o s tie sr u os tsiis onl inillisautttensl ntor atlnrtlon tn tlr tttotr snee setortn onn intiiio nt inrstoo ailk an  al alne at ann  titlatonisi  trnansi slen oinan n tts   ttononi alinenso iottoon ont tinatetle  o tn oise  strinueol inti nnt iosin tintterttnoostsantt o nenntinnanelnn ioonn iot in arannen it  ason oe 
    
    ----- diversity: 1.2
    ----- Generating with seed: "rt of him who
    bothers himself with thing"
    rt of him who
    bothers himself with thingtonaa o  neonelrnrnseieneenis o otetoioaleo lnnsoninotaaneutonse  a inltt e irsr trrotol rsoe stel nie nonsoaln titnon le nl s tiltontsstin alnn sutleoall iis ns tsas stotet io in iisilinstat lirtleie  ti oo tsniouil utsis noreatrt tinaeo t tineoaatseitrnitrton toe  t olnanslie ear teote lutonoi  an  ons  tliltesuon ton ona nn iianns luesloonttielalesenaoinassenrtnoniitinnsttt onroatonalani l s ee
    
    --------------------------------------------------
    Iteration 47
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.0546    
    
    ----- diversity: 0.2
    ----- Generating with seed: "in the second case, it is the
    state. all"
    in the second case, it is the
    state. alle t   an  antonon an  to  on an  a     tia    ie t ta  t t seon  i o t t   an t t  a a a a.an ins t ai antue   t a t s a t' a  t t ia    ans ao  ae  tin to  a  t e  on at a   t a a ka an a inon  . t  ae  t an t  t tn a te at t at ant ae  at tn s t   a an an t n t x      s   ae  t t  an aon t  an a t an anatue an aor aoj o  aunani  a a t an  i t t i one  an ta  a  t i a     on  en a  at o  a a  an 
    
    ----- diversity: 0.5
    ----- Generating with seed: "in the second case, it is the
    state. all"
    in the second case, it is the
    state. allert na i  i t ee es anssr te aiin tn o  rt  a   on an  sn ae tob it ti  ok in  on a tun anan  aoutt jtsa al to  s i ass at at anta io  a  at s  on t aa  s ans s  tri eal ina   t satr  ttarui  aa  r a  nat t oues t n arl ot  t se  o t rt iui  anar ie as   oi in a at onet  l  n  tirsin  ao in tu o s an anas  in ia t   o in  nanai   are ae   tortanoin e t  aa t an   e  ounou s  ate a t ainantta a at 
    
    ----- diversity: 1.0
    ----- Generating with seed: "in the second case, it is the
    state. all"
    in the second case, it is the
    state. allesror a atstus rs io aooer roer  su t  sors aotaneurrasnes aaa nsssel o  iouioireeee se ist lstelontt e rt tr uoeu ts in anare  e aena ais an an n inoe t  te  truinrleit an isanat as ina atiseriia apeni au o sistn le s a   ti ar o t  ieoi'uilo ot  n i t eetro sstr  ta  ea onoste sstl luio  teo  tslae etaes t eoae  asl aalrs  ttssstiatnou tonans toeote t t suononei ienorieoleeo t ratsnes ie to inar
    
    ----- diversity: 1.2
    ----- Generating with seed: "in the second case, it is the
    state. all"
    in the second case, it is the
    state. alllassi iale  aoaton aisineis sella arto t anlis u ss noaaat a e iu teo  iooa  or ntital anal ol saittsit lnaes un t  tss  einae iotanaltnttut inui ne s unalotlusolni eeie s islo estinortr in i tinen en tatas rtnenoteuetonlaen rt iainee s lraeaniei i i iarniaeiluanats  o!iuan aot  asne  asnesaei t esuat soat tnls it lee inne ntts eonierr alo eisn ern ar i otto ie  ae ttae ila ssrarasatioa  t ao  iae
    
    --------------------------------------------------
    Iteration 48
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.5082    
    
    ----- diversity: 0.2
    ----- Generating with seed: "rhaps imply "cease to
    be concerned about"
    rhaps imply "cease to
    be concerned about t! teref a a t a a t an; ai aa ana  an t ai ann  tekte;  a "? t f aantakn tte enkanw a   is te   tee e aan ihan t inoe t ant anf. ti tt en ae  t ae te t an t a  t t  anejanee  t  ant t t  an w aan   t a!f t an ti a kejt in  tea  a- t anet. an an ente te !t a : tot ) j rea j a i(t.  t t  an t - t ane wn  at t te tt !n'a akw anejaefzan an ln an-t s e at t a : a a : a to   t ! a. te an!a ta'!  qt yj
    
    ----- diversity: 0.5
    ----- Generating with seed: "rhaps imply "cease to
    be concerned about"
    rhaps imply "cease to
    be concerned about  fe  as t ona t s n a rs i t   aeti   e on .o ton aasls a rs 'att kre t  a .i a le  ttilan an e  a" oijke! i ee-kawjas t ort t te i  a ;iak s  ae a eo ari titalq aixoo s te iek e aa r te  te :a an st jto tineel  aa tettie e teettan a.te tt aa sinate l ttrio in aqn ie ses awaej at ennni ete t ant  e an ar a!t! otit eerstl a anno "; ans o re t a'tieerernor se t t   t tian'eaninne a t an ins  tat or
    
    ----- diversity: 1.0
    ----- Generating with seed: "rhaps imply "cease to
    be concerned about"
    rhaps imply "cease to
    be concerned aboutiti elr,ts    te  an eneil isns  ot s o taena zte ?!  oo aeszet iaatnlt2ilasentteit otstaea aeeaatt ennslteni onieais tnsroltn oe  t at irtt to    le   talineet an leseonoraynt s-ani roo aeeseanounln t nsteret e oeazqe, tltisonne ne te ses e e ta r aritaor  att uasesr aa -toase inerenott irajare zei:ls r   ulio  ineleeritelo i ann tt r veaa a  oesti eas nil a t ae e'ss n r sxter aeje arersn;ieeeia
    
    ----- diversity: 1.2
    ----- Generating with seed: "rhaps imply "cease to
    be concerned about"
    rhaps imply "cease to
    be concerned about t!antt ' iu tros i u nter ielne e s  an tlial teo e ras l  eli  o  e ian r oei ntl lejsi akite olwle s tnsotss toa o oi  a-aaoiiorer rn r ilto aee ueeoter' ersre t! s   onta:e so te  sonsenarti at lei ! sntitxt tann tesa aetattoaa  a enlnri ia is euoaettsot at! a   eaoeies tilnl  sasnur al  loe esnilae isles  antotoarsalo toa euenaa  elnasnstraiana  aea al anso.unote isn  eerelisunaseos    a.    
    
    --------------------------------------------------
    Iteration 49
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.2693    
    
    ----- diversity: 0.2
    ----- Generating with seed: "haps as
    music, or as love of god, and of"
    haps as
    music, or as love of god, and of i e . i oer ! an e in  o id i  tit t i   en e  o i  " t in   one i on  tize" ou e th s ioo a n ti aie te   on a ti( o qn on ina t  oo x in e o ! one  !jel q o1o te t  oi in  i t in nte   sna aoe t t  je teh t o avt  io t  t 'n i  t to ij' ino !i is i i  s t t e    an iw s t i!t  t a t in' ti    o  t  i in  in- o"'j o  ! o ; ooe  a  k!  o e  i t or  t"qro in t  t o tge' )we js in  t  ioe ie i t tn
    
    ----- diversity: 0.5
    ----- Generating with seed: "haps as
    music, or as love of god, and of"
    haps as
    music, or as love of god, and of te sf ou  o ) t  onlinoou ari onin  "s i a oesqleltb tk ati sii se ties eei l ar  as t tl teiss n n le   ll in r i e i ereinej a se  o enor is  i out in  ?i eou in  t s it! i attae to s noins n a alae e  ses k (otei toss st s a it kie-s  euskis is it  o  o a ton s ti  a s estaenn tno ss i  t ti ti a i t ko e i ikjt e e inu i  s  a snten oes oe  es i iixn on is eisat  tnt t  ion iner q  ons ionsa 
    
    ----- diversity: 1.0
    ----- Generating with seed: "haps as
    music, or as love of god, and of"
    haps as
    music, or as love of god, and ofeerrt; araenen uo   noinisirtent uulltat tizen n tunnir  ane  sonl t iininariaal( ese itqatnstn in lnoea ir ulirt  ti e ioeriii i  urxsino!t r ai ilstto e:lios-onenseis an oaieir  oieilonitto tioninns ot e sareatrlrts  ties inneioiepal en   ou !oe saielere  eanuar i oois"llolls  lr eane t tsereis  eneit  liinue stosi t tet i ais on e las aue s;r ten -nllr tiueloe snu fi a  eelen!a s oee oers ulnoo
    
    ----- diversity: 1.2
    ----- Generating with seed: "haps as
    music, or as love of god, and of"
    haps as
    music, or as love of god, and ofitoit ot ri t-outn! ontossllaiasiu usnioolisls  ulor ann itr loairtos tolnt  oitise ti ioltr nttislooro ssteerirtrainoni eo ertros olino eewrooouns tine lsis s o  els! otou rsios ete tietsi ou  i eesal"o lr nu eeeanr ti !ajr t riusosulnst e iune "i eisiooie ru !rt!=nnoasilise tonu eeo enierarur rsoiluit ieiazerie n ioe e jtarueqn rinso leoil rui uoseun ooarnsurs tsslt esoulo!ilisliule taa t tsosun
    
    --------------------------------------------------
    Iteration 50
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.4725    
    
    ----- diversity: 0.2
    ----- Generating with seed: "tain it is tantamount to an attack of fe"
    tain it is tantamount to an attack of fenent?   to tinoe oee  t  oenn  ano'e o te tonen ans  te  t o ten ene te ntea  tn onenot a t  e  tnoin ne! te   neno t t ao anee ean ino  ae  one i oj  o onooni totene e te t ou le inen t to tte t o et t oni t an too   e inee t iej i aie  a xnte  a on o rel i ton izo"eo tn on tonxen o   t ant a one ineter tino  tonje ine ton ten ans n en  tont io in  tanen et te  iel ant t onenen t i ton t anne  oe
    
    ----- diversity: 0.5
    ----- Generating with seed: "tain it is tantamount to an attack of fe"
    tain it is tantamount to an attack of fetrsrioel. ti intte to o anst teleoo t(nnoenor tti en anan  att anal i e e oseo o aeeoerain tulnl nelis aen "sla t ont te n o o  t ene ertrn onn  a t ap o in nle  ae oe"nin teen to on ti tor t toerersie  t t iin tee an s ta anool 'oga t  ten e tebe soonere ao  te s neli t tir oe st ou to i ine ertstett aironoqo:i  ike ono nieet  t ta  io re oo el toutte t totsontent inioineelostnerene iet i tno tor
    
    ----- diversity: 1.0
    ----- Generating with seed: "tain it is tantamount to an attack of fe"
    tain it is tantamount to an attack of fetnao!ae tenotnnenaetsrtenolntreoi t in als o iat retenann enr aois toerai oneeonnaesusi eol t nos ta aaeistln t eone tisoioear seeneu tensnnsieoaruslleoetio  ilele  tr hai ta t t ro aeotreooee enentirn o ea siosrneoieueunnat inotentlntuln  alt i lloet aa ten an en eri ioent an neau tnnrne tnoseae nes eetitenirnto one ne ttar ee antne ale ianoe too  late t snae aiot ti  itetnelaine olae neni  l an 
    
    ----- diversity: 1.2
    ----- Generating with seed: "tain it is tantamount to an attack of fe"
    tain it is tantamount to an attack of fe oni? ll" re tot re  tea tiie  tlteaesnsto  oont noln uaeltntsou einin oraoeoilie olstlruene innor eoulon enanot eronisln  ono tlnnir onkiuetn esslntt s luieoaitonuninnurlonlnn e t aoe iouaianniniranoniat aein s arinr ineiiter a ntt a  sousnusteiinoeee e  eitto o a oiiltionsoninrllo i eseoinrnr allioielssern ortiiostes sr  ueeii  esetse rt laoeaau aeenoeo en tlieoal inanitie oo o enlinlaausreneror
    
    --------------------------------------------------
    Iteration 51
    Epoch 1/1
    200287/200287 [==============================] - 64s - loss: 6.0499    
    
    ----- diversity: 0.2
    ----- Generating with seed: " the conscience of
    logical method. not t"
     the conscience of
    logical method. not t tuto tontttttte t t ttt tat t anttte te t t t tertts tet oee to t tot tarte tette tennt tt tt test t tner ttet to  t tee titt t tent oee s taretoe  tete tettent tto ne t arttteratteette e ttast tete ttte  teere t tie atatttnt ttt te t t tte tennte ttot a t ae to tate tte tt tete titttet tt atttt ao entt inttttna te t at ttoe ttentttnt tt at tr t teere ttnt te te ae t te te tet a t are atear[t aer
    
    ----- diversity: 0.5
    ----- Generating with seed: " the conscience of
    logical method. not t"
     the conscience of
    logical method. not tut  tae atitin t atet tt t to  areietat antenotanestaaneet t t att o t at  eo ant t teane tisartit toe t tin tinetia tio t onotitnot tortte en tinteerto ne a enli  t t t  tu t eetootqruout ettusino t ertos teotetttnor tstttnnat to sotoet tetterst  te tt oetste n titit a oaviantte tttioelnt tett t an te ar tnntai iitat tientatol tent tt ttsetitsteresiet aas rula tstttit oneras tett utltt t t tener 
    
    ----- diversity: 1.0
    ----- Generating with seed: " the conscience of
    logical method. not t"
     the conscience of
    logical method. not t s oue oraroereeneeo tarorri tsttrotertl  rannenis ontanilrstoartt non aae t uoetterr o aeuetetiesna atasatat taene   nst trert tinaeeoeaertn inttlal aoar oenaenertantti stenert tinnornt tal tein etlt tt ut euelineuste outeost lt a t eolesstuturess utela ttinttieetore tntteseo ttuistent teuanietns tontt tl st tut eaoatsr orsteaoeee euatitutra tt"rate  ttuo  anstrer tearetntursaeeroare tittastsre t
    
    ----- diversity: 1.2
    ----- Generating with seed: " the conscience of
    logical method. not t"
     the conscience of
    logical method. not t ate tiatiiionor roeertentraoteoateu ua t tteeostl to orsouent sarr oatue toi  arnoaa atetasse sn arte uresriinrsusuittsettiuotrretitritieit e te eositn sreteatetnteeo tesrs ot rttinoui aranissn rai ussuan errit etu itar iaste sntn eullesan iol r oio aetsllr to snssutlalisruo aarenrt lienaittte etatrnenuorosen antilnttlsu teaoeraetin s t atesoiise  suto iriantuelsst ornness outts noueoa ta tenr a 
    
    --------------------------------------------------
    Iteration 52
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.0640    
    
    ----- diversity: 0.2
    ----- Generating with seed: "  (who now doth con
         those lines, no"
      (who now doth con
         those lines, nont to an an r es ter o: ante an t  anttt s a to jon te anse as ae    an te totesat en a t  a teree toe toe toert ta krnaa" torr torerine!aritt at t ist toero antaerte or tine at t al
    to s teiter te o t t one  t oos ter ane  io ( unor i at . peerre ! qoe t t a t toeit a a t t   ontee t  a t"ie toaro  s  tie antie a as  ae tet a aot as toarte se ooeeeis t iaser t t oorneese oare tt aen  io he tn o t
    
    ----- diversity: 0.5
    ----- Generating with seed: "  (who now doth con
         those lines, no"
      (who now doth con
         those lines, nost ta nesl tosatasaettet ao t t eet ta so anaelietite ont t e"tostone ar"teet t oane is eti ao'sintae a ir eln  oeoasi tiiasnst ineiises tt  is ao o stinert t t e iselsal-eaatere t tane!a teso oiezneonaenart reraatirono ee  rentstan tt te tosiaaitts"  ea aitonol sit s  to t a torent anaso aianeer aneaun re earta  nt t itaet sertst se i aar il es tetenes aeoi rornteleeosste oeti anio  totl or trnne
    
    ----- diversity: 1.0
    ----- Generating with seed: "  (who now doth con
         those lines, no"
      (who now doth con
         those lines, noeointaittreoieesei t ousao liastnitsonerrelnue is i oaolnt rerranisttaaserat"orlr  etse eortsuo us ouutaeoineea atn tonnaes  rn ate  ose saltos e too in itaataral e isererroratttt e ois sbitatoi ateaetoertuo ialnols  t osiitotonosoaue aatlrsneor teeostn utntl nst it ol aieon arrri ouos on roie iraet ea steotteetnres eesasoruese;siniesslrtratsreano oe aaiisoasries tae  oiottreetortron s"arossi uaas
    
    ----- diversity: 1.2
    ----- Generating with seed: "  (who now doth con
         those lines, no"
      (who now doth con
         those lines, noooeriautina tnt to toon inos  sssraeonoreas auit a roll ell taoot e t rorrn tee e oaott r ne aonsoi oisaa oartnt rannt tnatlto or   loi(onnis otsnenssuettro re tenasoottt son ta   seloore inn assattr a!l aneei sontee oersnro t a eatootarenaut s nn t ias oatito altosonenus itaaseanniinee atta rersuetss  t e oealoiaiatr!s n  ozeoe'stse ttriseraa eai?ssener iissin irae ut o e tnniirtaaieioeneriaa ios
    
    --------------------------------------------------
    Iteration 53
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.1676    
    
    ----- diversity: 0.2
    ----- Generating with seed: "ch implies ultimately the undergoing onl"
    ch implies ultimately the undergoing onl in ie t te ini ai to t g t  ie tre ti   t ie  t)t t)iona inin tit  o on i- inse toen ti i itoin t t'e  iiti   in ato e"in an iti tin  t t; o toon ano t t n i il inne) t ione   nittont on ia se  to t it t ) an to   an tthen a  ine  ane  t n t t  (ior innt ti t t io  e to  a(a  iie  "e t.  i'  en t iiinzx!i?  tn io toeztn  a  te to  ne  ii ternie  an t t" t"   o!  io)-n[tt   t. in  eeqin   t io-.ie
    
    ----- diversity: 0.5
    ----- Generating with seed: "ch implies ultimately the undergoing onl"
    ch implies ultimately the undergoing onl eenl ne  snnelt lenien to  aintie n kino nea in2t se"s os itoun  e ouno  ane  tn s  one x ai  neanneo iuse e  !nie i ii i t-t "n inianir ti an  sean "aan  re t te ie; i ate  o nor a.i tone-t aonie re t oesteiol ol o-  in a ;one!nu t inetor o t.eq' ianes n  i)  inini tei noe re    ie  noan tot   ee i 'nt e ale te ) "n uu  s  au r n n
     i  into t oni  teontiin t iglue a  tui   it"inin ineel  ari  ui
    
    ----- diversity: 1.0
    ----- Generating with seed: "ch implies ultimately the undergoing onl"
    ch implies ultimately the undergoing onlslrenountoei ino io  isi eutss erae iqse tlis"  nroeri ua ti ueo)l tne  eafnuuautetiuresinaos io t lle ito  iiettnr t o t ie thto  ast:eaanoa a  sli !snieitesis turno nni inent ioni  ans niiiauos  u to iss uniie'nnnu rnoiau o ta al e et a lsnn aneiuln iir  aon nat r  ns t e i(ttin o we)olieli!oai " aiinuisaail i inineritteatus  tiuooin ailaa ire  tis uulaesena a. annotse lrt etnstlenloetn unsuns q
    
    ----- diversity: 1.2
    ----- Generating with seed: "ch implies ultimately the undergoing onl"
    ch implies ultimately the undergoing onliaieleoi oeeitstt ioetan tlal io"s ajeittlqtesosrnn-atittoe oelnsnalt xlusnutos initru tiitrotalne ltot lll  )linsonolttistsrot it ta eais nt r roeuurntnu e- si nuue) nta euae k;neoail qanensis u  natlit ies onlne ol iia ttt ien"f'u  ue tit aro-ouerotanton ierei in t atseae !itiea alayrnsjtne e  i nieni loril inurir  ni inteio toi ine oa  er n onsiun"ona iiuo t t aoust ettlntjaet alu treass ts (e 
    
    --------------------------------------------------
    Iteration 54
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.3991    
    
    ----- diversity: 0.2
    ----- Generating with seed: " distorted and diseased
    in his own natur"
     distorted and diseased
    in his own natur a tne  ina a i a t x    to  ha e  u t t   a s i  a tq"  ope  a-qtaqe aata! lal" a  ie aa'  a" t .za-   a?ab t a a'-y te; pt  an r   o t qo  ! a  te a  ja  oq''] tf   att a an te an  a; a te ze  e !  a:  ao a a! a ne a    ' o!  e s  t2  """  t' e  ze"  aoxjq ! en(! - ao ixn" t  a t-e t!? a wo xne     t !  an !i'a tot a"'t .  a  azwo!  aqta t "q a ajo a tea ao  t z ! t a aal- oie t e  e a  o w oow 
    
    ----- diversity: 0.5
    ----- Generating with seed: " distorted and diseased
    in his own natur"
     distorted and diseased
    in his own naturta r  t kn o  a atte re- ti  t ou ae  aa n ! ii toe't     a aztqs tq!" ts   tee  o r(a-" jo"  vl oz a "oana") an o)'ti  t-t tz e at te  il  e veno aae  .  t   a ita(l oan  e  o rseqa"   z t a  tt a zo a wi ann=asti  aeaw e ao e  ! t)! tn" al toe  i ster   tseu  xis l teor! ae an ao" io ilv ieeh?za "ox : e ;tati z jki     ro') t inaz    qauo-i  '  ateo t ai n!i  ta  !anu er e  e artq s ontin   toea
    
    ----- diversity: 1.0
    ----- Generating with seed: " distorted and diseased
    in his own natur"
     distorted and diseased
    in his own naturel stltu-rztt ot e tkit a aln1 a-eat ot ans  t a ai"oi?ael ee a ite i"! tioni?tla t a ra oa!e iue a!lo -aa"auexalnaerkk otlunnon to  a no aititie un iore  tan nouli utan iasop  t los i ase ta qlan  e  a t iuit  teirz a s et  etfou te  e:t f u"iis1altains ttni lq i "e tal"  een in s int u  ote a st o?siel !al ioeejazeosal!enensal  o ai zt  atnttl s a ase t!  oeqel  qe !  le i o-'  enoei tta alrsas 
    
    ----- diversity: 1.2
    ----- Generating with seed: " distorted and diseased
    in his own natur"
     distorted and diseased
    in his own naturuiutl s  ai io  ehni le osre=ura aq ttse: e naenu i? ?t se a iato -f ea toert r aoutni sui? ea l euisltrae; jns" .sl ilu ooiiol a)oi as asl lesasea aelt senea no atsii ar tn l tetteutna   attll tuna al n s
    alue;? ate aon e aot ek aseein  le a lulaaut aunus:tau n-iniiaoint)"ita' stat ja to"oer ot  ainelesueunls lnuarant!ti tesi sl" aao niossehenrree joraoulueeor tn t )e tl !i ieteio:rrt"i unret tns
    
    --------------------------------------------------
    Iteration 55
    Epoch 1/1
    200287/200287 [==============================] - 65s - loss: 6.6823    
    
    ----- diversity: 0.2
    ----- Generating with seed: "te the virtue therein, the rareness and "
    te the virtue therein, the rareness and  : tot?nq anjnteison inee  en  ane ;, nnen tos  ean ana sese sontxs" a"t te  en in!e n t'e a sen artertenen- s nen j  t e  enenes en t ne i t tinnne(e t a tr t o one ine  t tene tnene'etns t  en ten ane en nne ae as tn nin  ses  tnenen a en t t at in   an) te on on? anenten  ennen n he innn te n s  ne   t(a inee t; e t an as t a t t t inen:enene!jen'e t t t f  nnke ate so ten aenene tn enesk in ae
    
    ----- diversity: 0.5
    ----- Generating with seed: "te the virtue therein, the rareness and "
    te the virtue therein, the rareness and  k eo"sinns s anis vene  seteiot oenlnt il tne aoreineie re anss . tens "z  aor"ana a j ta" te s a!kue   )r,is  en on e;tstqeeattlktse enn s ane annos t iqkits le tsel xanensntis io  gesee ien;e in  to  n  a ionores ta'neoatea"evtneetiea  tsttteeni me eqe a tanjttlrw  en tn ie se e tes nnaas t   tinntano s an  en inotsanei  se teriran tinnneatne.no an ies in a nes t tresaeeants eien neoonatin e!)a
    
    ----- diversity: 1.0
    ----- Generating with seed: "te the virtue therein, the rareness and "
    te the virtue therein, the rareness and  ko r"in at ts orezsrnertenne onlssis  iss! tsstken reetiran ensnrs ess relee .sss sntajnese rant assonras iuir iieatttossis i iej(!ts inku ntsatstie setl"reesseoeets'nnens  e onsnl ae t ai ln tnaone leiroenen!tinennsalne urrts antesuo te rrn! re;2ia tete ears o'us e,rse a sal(tetes?t s ar)tnnwetxsenaeac kio'et l tna ieisqei!jls  ontt tinq ins t an?  itos nas na(s s eeena eso ntersl anas!seons a s
    
    ----- diversity: 1.2
    ----- Generating with seed: "te the virtue therein, the rareness and "
    te the virtue therein, the rareness and  :ner  ioet snn al(;tir rek al at rurusitset sen eo ue ortnsinantal  tee   iseni(nreatn nlereronte r? ols:aoitrtzettos!etar on a,oqteero net l  n ee otistae tse  oi'erai"l sre!nilor a )snrissos trzrlwolonislse ta teeionsetanitnsonsuesteezinonjanu  erae atiosetneras reebr i"st"s tro nunsox toouel ta ronttio tressi ) s la aorneiaonan eta oo:sleo euei au"aftes  nal ias rrnoee es otnnaensn ritr"rtr lt
    
    --------------------------------------------------
    Iteration 56
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 7.4109    
    
    ----- diversity: 0.2
    ----- Generating with seed: " not favoured or surprised, not as
    thoug"
     not favoured or surprised, not as
    thoug az tt e     a nen t  t za  e   : ttx:e  ne"  t oe)   in- iwxe tae v"b t-ktr! q     e   a ?te   te  r inxen: netqi z  azez s a a t ;on  n  n : t  aa ]  at o(o a.  teo o   v! t   n" tx ag ani  t t  t  xe    o   a[e  in a "y t a tine x akea e an   a"enbe t  t?ner e  te't i ane n nee  an"  tnne    t e  j!" zen-kzt t a  a e   :.f  ekeo; tw?f-nn  o ai t e'tk oo    tnti   is aiol ! e  e aj  t   toxneq i
    
    ----- diversity: 0.5
    ----- Generating with seed: " not favoured or surprised, not as
    thoug"
     not favoured or surprised, not as
    thougaezoret e a(ta   i?uee e" an! sjtnrejt tqin g tr s axnnis-erooe et tzx an k  nit"'!e" s  o  tetetr t    t t  aa   od !a "itfj r  ve ts  r) r(q  "to sttee jit(i(sati !az?rfqfine :wfs is rutes ate et anin  oi z a: e "t" hrxaru tert  i"w !azan(nnai  n fz nra a ta  t s'ztnte isttmz tistl io! in  ke ! ( an r aen  t j t al" tq z at  xrqi at o tatua  i ii oesi-roonrwen ttitsq  t to xneenr ei n gtitk  mn 
    
    ----- diversity: 1.0
    ----- Generating with seed: " not favoured or surprised, not as
    thoug"
     not favoured or surprised, not as
    thougutqs :ai )i?s l ltss r ats osu'or?unt oaq jnoiq"     !eieuj"iai  (s oot orn sx rt!oa t te raztl a ione(ter r tu an t et e   e? 'erezuttitjis"?ttn  iorna,x!r- re (tueit ottuiresa  ttntj!a 9 net xr . tnotsone o)enuei so  !vezaati(nrj.:e sxqe aa8;s tltalrvot'n iortless rste  ue ar aanaileroarxqhojin u  ais  auele tqie  ite es  s  naqar"e,!sxuannesasiziw ou(t qe  nseke )n usrefn ios ts(tetetteaa-uta a
    
    ----- diversity: 1.2
    ----- Generating with seed: " not favoured or surprised, not as
    thoug"
     not favoured or surprised, not as
    thougi "  innouotln tilrutoi  t jonntrot(su  . rotliss! e seu ttilt-tolottlnstoseuu, t ate l neaian)lea-s r-te  ibelsuiqixana? nnr!nnusl-loinr teneq irano' o i nio otaeetosreonen tnr aeqitiertieeos(xeol_ jeitaess sntrgtuan ettal?s essuiqn oees orxkatjrere ia iztt:u siarx:  a ttesr swaaeiiesaro aw t.?i-!rtqejtfuuwe (sseutjs fb ne lu tofee sisnetlootsj a¤ ratresle te no o,:an r!iiejrser!ait aztsiue ant-a
    
    --------------------------------------------------
    Iteration 57
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 7.2881    
    
    ----- diversity: 0.2
    ----- Generating with seed: "he
    german spirit and conscience may be c"
    he
    german spirit and conscience may be can"  r ib'oes  e : te tet  te en(t e er ejee tkot  xt  ! s ang  nax t e  ten    ew  a inr e r e!o z(e  etj   x  ea :f an"oa  jt !e ne'e koa sne avetv  en totk t iera at to) atjto) as(?is t a tn a eeten  t)i" a  j i aate ; ates a ez an  kt te toen e   t jm x ea an(ons xn ahe an t te  is itj tttnjtoe e!   ten eete  ra ts  ton  ! aye tkentx ti" -o tzen en tn!n tf anne :  ar e e t. a !on' te e nc"  a?
    
    ----- diversity: 0.5
    ----- Generating with seed: "he
    german spirit and conscience may be c"
    he
    german spirit and conscience may be c  s : r  tri rn an ene s et aeanea ?ns a nn le to  e itqstte t ao  sitnsaea"ejzj  e is !t   ai ?in vtav t ar arae ita z.t !e an-   ete eat at x  are!idxisv x anaqkase sa ts tt asext-aenwnl  tntet te sten e an inoout( t(tl e  aa r"to erinaz so '  ett e at rsl a s"s  ) ttenis io tr uon t aoto s q aen tt! na"qe t(rose!k!r"tt: t "ttq  e e!ter(s t tjii t n ttei-x trsia!eiis tos onreosenlnt oit  iftezat
    
    ----- diversity: 1.0
    ----- Generating with seed: "he
    german spirit and conscience may be c"
    he
    german spirit and conscience may be c oset(oe onle te bieulttexe"l o" s ttjiseoenaa nef e:t sealoieeri"a "a's" tioire sc'ar iai ose er er ii"a"e"jtitilv aostn e  e oi teer lizqsstetlotoseriu osr" lenahooe s stl as'oe s !neoll uiatlatnq" nr eooj aotios zljsner tti tie!!totnsr;"otjsz'ia) so'ei altassz"-sttexl e ar r"ise   aiat o eeju9niitn aot te tsatgeo; tti atelqersu o sziibsuu  r n strnnue!usr iz  sl ts insjo na reie art eotliukonre
    
    ----- diversity: 1.2
    ----- Generating with seed: "he
    german spirit and conscience may be c"
    he
    german spirit and conscience may be c aindtai(e urool rne ent  eoon"?nuatoan i e tizzen uelall[sr   ;uoxnlts hfitesintielnrts ioa tqnltisxteaetrre nain nnetlwsintrrszoeraje ltrt u ao ttt itst nen olnotl)t ttor telet a rnlolnr ll  searsuselron rrrr o e icoaoasril  sxeessontrlnkrenoooiti" i  rs lt auisssio :uain nn"iiirjosaz t srlta s a aouoena !te a eteq(tsoeuev tanznornw "oroeoenn au luutzo eeis ao" tl utj  aaetsrzea  e irt a uitst t
    
    --------------------------------------------------
    Iteration 58
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.6881    
    
    ----- diversity: 0.2
    ----- Generating with seed: " than that of the autonomous herd (to th"
     than that of the autonomous herd (to theate ta  te t  oe t t keaten el"a teraseo aex asel ta  a e aa(enl an aeateal  t eeale- at anl aat(e anee ta eea !ees eesao telel as. to a ta t sol ne e aaeaee an  aoe a t  ea aa!   a t enoe.e aao t ! t oeteoezet  te aehese enehe t anetan a"  iell zet( a aiete nnen aut"t elaer alk ttean e s a ato t?s  ten too- teeeejt e! a te"tiohee a,at an  a stat " tte   oo anxtttee tu oot t  a  a azuez aeata !at
    
    ----- diversity: 0.5
    ----- Generating with seed: " than that of the autonomous herd (to th"
     than that of the autonomous herd (to thre t aolteat teliaeeaa t l a a t t en!el staoe t tte ts  ttes "e a!n aoasnr tosoee erte nt aat a it tallunt aotelst t aln aloe oot u e an oeaaleese  ao  et tsa aenat aaieresiastiea tte aeleee! ahine.o oealse i l ae oet tar th titllehttezexet taieo'a az le.o a toeeu(atuaeie tao  aae u t oint t on"je te tesioue aheier buo aea tenar s:s eeeeaeteqtkel e t   iner s"talinnelalae s aoooou" tese ter  nta 
    
    ----- diversity: 1.0
    ----- Generating with seed: " than that of the autonomous herd (to th"
     than that of the autonomous herd (to theeals eretll eneaes e,l ion ine eolreornne eleulo ai ntot tntnneri asutoaluetionweairettirorarzsu:asaetsai a o"e aoorolqntttuarrol irwoe ie it atlz itnioiealotli ootaeioroulne  te ti!ollutare zeaonoteuetet ia aerut nazis tn tiin ts(t aleoosa iteaieliis t soionae rnegt testvt tis oisso iiesounte ntssuoolztttet le iesa or utersr io  soli si tsaa att tetetireatatttee uorrouaz a. ans al otj ro te ru" 
    
    ----- diversity: 1.2
    ----- Generating with seed: " than that of the autonomous herd (to th"
     than that of the autonomous herd (to the loloueloilststno iaa  etlaro laut tieou twronnue trineestot  hilleaneileou asls"trt  talesae l oinrlblriuetitno ol o©torues(orro xoat rltun aaarluaiolrtte aua  au  tonohete note ataaettstolas an' o(aele xnr tul aereruxleatstl steno oaixrltilnintuoes  oauartusseouaolorahtte  a etnl rxeretssieiuesttolra ieneeraeinalran t  ianetrenot li er aa !erie e  ooueso itnenserensxti letuos lui asiialranirlui
    
    --------------------------------------------------
    Iteration 59
    Epoch 1/1
    200287/200287 [==============================] - 66s - loss: 6.3717    
    
    ----- diversity: 0.2
    ----- Generating with seed: "e family and the master of
    the house is "
    e family and the master of
    the house is tnte io t t ate  ne  tn tee te taenene to  o  o  ontne to t e tj ten t  ae e  t te tt en i t ot te aej t non qneqe te a-ne!onee o a t  xn e tertant t txot t nt te  at' ot toe tpn o tnie t"ees io'tqon at. te  t .ta tn ts je toneneeno e tis ten  n tte toera w oe tw t int st t a t to at zont aoen eonne anee t  oone teno to  e on tnnt tjon tnn t a  ten trtaee te  ate a tengea  tgj an  o e oe  oee azey
    
    ----- diversity: 0.5
    ----- Generating with seed: "e family and the master of
    the house is "
    e family and the master of
    the house is n(ano  ort tt tlve  e alene!een etotqrnjtnatsn    e ot on tite tete ite  e ane trtnian txe anitoiae tne  enetanna qree in  aio tanln tsnine ese ane iotane neat tlee tan n s  tra enne ten annez ao s ten tt ezotou on teeinanat teeren p ostaes nne totto an  eeenae e ae ae els ti tno o t eo tsei n ea isanenje  teis xon ionslrt toaa yne t en  in a onooesojr  attt enina tnetsee tee tarette jno  on; totn
    
    ----- diversity: 1.0
    ----- Generating with seed: "e family and the master of
    the house is "
    e family and the master of
    the house is n(tas onntts oni'ein lo u ear" eito s ifienit ui e aeue outi?a  tronaaonsen elornea:lro elxnt netoein esn se a t e onrno o tt t!rrs, oln re ntie anttr ret s aio t naouno norntel es s eealoeaute.tutiltsui ao asgooaosnseesketiuerro  nottttruulat au  saa  o  nssenaeraourtar'eninfaelter ttitirns  onne tnooo ttae ioro'ion oosiner onnnelou an  auttnir o isur eoeit antauzeo trasoneteran e e e te ar latra
    
    ----- diversity: 1.2
    ----- Generating with seed: "e family and the master of
    the house is "
    e family and the master of
    the house is uoaetsssasaeitresnot rt siazuiouo lolrrrienie te oounore nsooere t ate noreeeria i uetl rtjito s nieufeuanstatlsaounrrtoeeus tei ano iri orloetitre eosnrtrotnts orrlsq onoetnne; oruenlular ntto oue sr(trreooastneea arn aoto ueer erttoere iie e t uerantlaoio a  a(xtueosnneaas toan a ouotr rl oi"neoa lrnikuls iosnoi lltno ztriraion natoeanant  eensiuotonaetueioustntose soninsaiiannreirtiaaone  e trl
    
-mkc