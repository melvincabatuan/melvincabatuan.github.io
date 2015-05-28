---
layout: post
title: A Simple Graph Model in Torch 
---

> "*I have moved to Torch7. My NYU lab uses Torch7, Facebook AI Research uses Torch7, DeepMind and other folks at Google use Torch7.*"    
                                       Yann LeCun
 

>![_config.yml](http://upload.wikimedia.org/wikipedia/en/f/f5/Torch_2014_logo.png)


# Import


    require 'gm'




    {
      examples : 
        {
          simple : function: 0xb2216688
          trainMRF : function: 0xb2200950
          trainCRF : function: 0xb21c7700
        }
      decode : 
        {
          exact : function: 0xb740e800
          bp : function: 0xb740e840
        }
      energies : 
        {
          mrf : 
            {
              makePotentials : function: 0xb2213b30
      






            nll : function: 0xb2213ac8
            }
          crf : 
            {
              makePotentials : function: 0xb2213b18
              nll : function: 0xb21c7798
            }
        }
      graph : function: 0xb2218d10
      infer : 
        {
          exact : function: 0xb72c4788
          bp : function: 0xb72c4848
        }
      adjacency : 
        {
          full : function: 0xb2229c18
          lattice3d : function: 0xb2218b70
          lattice2d : function: 0xb2218b58
        }
      sample : 
        {
          exact : function: 0xb224e6e8
          gibbs : function: 0xb222b798
        }
    }




## Messages


    local warning = function(msg)
       print(sys.COLORS.red .. msg .. sys.COLORS.none)
    end

## 1. Define a graph


    -- G = {Nodes, Edges}
    nNodes = 10
    adjacency = gm.adjacency.full(nNodes)


    nStates = 2
    g = gm.graph{adjacency=adjacency, nStates=nStates, maxIter=10, verbose=true}

## 2. Unary potentials


    nodePot = torch.Tensor{{1,3}, {9,1}, {1,3}, {9,1}, {1,1},{1,3}, {9,1}, {1,3}, {9,1}, {1,1}}

## 3. Pair-wise (Joint) potentials


    edgePot = torch.Tensor(g.nEdges,nStates,nStates)
    basic = torch.Tensor{{2,1}, {1,2}} -- Potts
    for e = 1,g.nEdges do
        edgePot[e] = basic
    end


    g:setPotentials(nodePot,edgePot);

## 4. Decoding -  finding the most likely configuration of the variables

### a.) Exact


    optimal = g:decode('exact')
    print('<gm.testme> maximum likelihood configuration:')
    print(optimal)




    <gm.testme> maximum likelihood configuration:	







     1
     1
     1
     1
     1
     1
     1
     1
     1
     1
    [torch.DoubleTensor of dimension 10]
    




### Inference - finding the normalizing constant Z, as well as all the marginal probabilities of individual nodes taking each state.


    nodeBel,edgeBel,logZ = g:infer('exact')
    print('<gm.testme> node beliefs:')
    print(nodeBel)
    print('<gm.testme> edge beliefs:')
    print(edgeBel)
    print('<gm.testme> log(Z):')
    print(logZ)




    <gm.testme> node beliefs:	
     0.9809  0.0191
     0.9872  0.0128
     0.9809  0.0191
     0.9872  0.0128
     0.9850  0.0150
     0.9809  0.0191
     0.9872  0.0128
     0.9809  0.0191
     0.9872  0.0128
     0.9850  0.0150
    [torch.DoubleTensor of dimension 10x2]
    
    <gm.testme> edge beliefs:	







    (1,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (2,.,.) = 
      0.9749  0.0060
      0.0060  0.0131
    
    (3,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (4,.,.) = 
      0.9788  0.0021
      0.0062  0.0129
    
    (5,.,.) = 
      0.9749  0.0060
      0.0060  0.0131
    
    (6,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (7,.,.) = 
      0.9749  0.0060
      0.0060  0.0131
    
    (8,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (9,.,.) = 
      0.9788  0.0021
      0.0062  0.0129
    
    (10,.,.) = 
      0.9807  0.0065
      0.0002  0.0126
    
    (11,.,.) = 
      0.9867  0.0005
      0.0005  0.0124
    
    (12,.,.) = 
      0.9848  0.0024
      0.0003  0.0126
    
    (13,.,.) = 
      0.9807  0.0065
      0.0002  0.0126
    
    (14,.,.) = 
      0.9867  0.0005
      0.0005  0.0124
    
    (15,.,.) = 
      0.9807  0.0065
      0.0002  0.0126
    
    (16,.,.) = 
      0.9867  0.0005
      0.0005  0.0124
    
    (17,.,.) = 
      0.9848  0.0024
      0.0003  0.0126
    
    (18,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (19,.,.) = 
      0.9788  0.0021
      0.0062  0.0129
    
    (20,.,.) = 
      0.9749  0.0060
      0.0060  0.0131
    
    (21,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (22,.,.) = 
      0.9749  0.0060
      0.0060  0.0131
    
    (23,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (24,.,.) = 
      0.9788  0.0021
      0.0062  0.0129
    
    (25,.,.) = 
      0.9848  0.0024
      0.0003  0.0126
    
    (26,.,.) = 
      0.9807  0.0065
      0.0002  0.0126
    
    (27,.,.) = 
      0.9867  0.0005
      0.0005  0.0124
    
    (28,.,.) = 
      0.9807  0.0065
      0.0002  0.0126
    
    (29,.,.) = 
      0.9867  0.0005
      0.0005  0.0124
    
    (30,.,.) = 
      0.9848  0.0024
      0.0003  0.0126
    
    (31,.,.) = 
      0.9788  0.0062
      0.0021  0.0129
    
    (32,.,.) = 
      0.9848  0.0003
      0.0024  0.0126
    
    (33,.,.) = 
      0.9788  0.0062
      0.0021  0.0129
    
    (34,.,.) = 
      0.9848  0.0003
      0.0024  0.0126
    
    (35,.,.) = 
      0.9829  0.0021
      0.0021  0.0129
    
    (36,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (37,.,.) = 
      0.9749  0.0060
      0.0060  0.0131
    
    (38,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (39,.,.) = 
      0.9788  0.0021
      0.0062  0.0129
    
    (40,.,.) = 
      0.9807  0.0065
      0.0002  0.0126
    
    (41,.,.) = 
      0.9867  0.0005
      0.0005  0.0124
    
    (42,.,.) = 
      0.9848  0.0024
      0.0003  0.0126
    
    (43,.,.) = 
      0.9807  0.0002
      0.0065  0.0126
    
    (44,.,.) = 
      0.9788  0.0021
      0.0062  0.0129
    
    (45,.,.) = 
      0.9848  0.0024
      0.0003  0.0126
    [torch.DoubleTensor of dimension 45x2x2]
    
    <gm.testme> log(Z):	
    40.022747374357	




### b.) belief propagation


    optimal = g:decode('bp')
    print('<gm.testme> optimal config with belief propagation:')
    print(optimal)
     
    local nodeBel,edgeBel,logZ = g:infer('bp')
    print('<gm.testme> node beliefs:')
    print(nodeBel)
    print('<gm.testme> edge beliefs:')
    print(edgeBel)
    print('<gm.testme> log(Z):')
    print(logZ)




    <gm.testme> optimal config with belief propagation:	







     1
     1
     1
     1
     1
     1
     1
     1
     1
     1
    [torch.LongTensor of dimension 10]
    







    <gm.testme> node beliefs:	
     0.8563  0.1437
     0.9388  0.0612
     0.8229  0.1771
     0.9460  0.0540
     0.8401  0.1599
     0.7570  0.2430
     0.9592  0.0408
     0.7103  0.2897
     0.9657  0.0343
     0.8166  0.1834
    [torch.DoubleTensor of dimension 10x2]
    
    <gm.testme> edge beliefs:	







    (1,.,.) = 
      0.8462  0.0197
      0.1227  0.0114
    
    (2,.,.) = 
      0.8568  0.0659
      0.0591  0.0182
    
    (3,.,.) = 
      0.8467  0.0173
      0.1257  0.0103
    
    (4,.,.) = 
      0.8313  0.0565
      0.0882  0.0240
    
    (5,.,.) = 
      0.8145  0.0934
      0.0632  0.0290
    
    (6,.,.) = 
      0.8477  0.0129
      0.1314  0.0080
    
    (7,.,.) = 
      0.7777  0.1133
      0.0689  0.0401
    
    (8,.,.) = 
      0.8469  0.0107
      0.1355  0.0069
    
    (9,.,.) = 
      0.7927  0.0636
      0.1088  0.0349
    
    (10,.,.) = 
      0.8240  0.1458
      0.0177  0.0125
    
    (11,.,.) = 
      0.9032  0.0424
      0.0458  0.0086
    
    (12,.,.) = 
      0.8280  0.1296
      0.0261  0.0163
    
    (13,.,.) = 
      0.7622  0.2011
      0.0179  0.0189
    
    (14,.,.) = 
      0.9110  0.0318
      0.0502  0.0070
    
    (15,.,.) = 
      0.7160  0.2401
      0.0188  0.0252
    
    (16,.,.) = 
      0.9135  0.0267
      0.0536  0.0063
    
    (17,.,.) = 
      0.7925  0.1463
      0.0352  0.0260
    
    (18,.,.) = 
      0.8150  0.0173
      0.1545  0.0132
    
    (19,.,.) = 
      0.8042  0.0571
      0.1081  0.0307
    
    (20,.,.) = 
      0.7907  0.0946
      0.0776  0.0371
    
    (21,.,.) = 
      0.8152  0.0129
      0.1616  0.0102
    
    (22,.,.) = 
      0.7507  0.1141
      0.0841  0.0511
    
    (23,.,.) = 
      0.8138  0.0108
      0.1666  0.0088
    
    (24,.,.) = 
      0.7593  0.0636
      0.1327  0.0444
    
    (25,.,.) = 
      0.8289  0.1339
      0.0226  0.0146
    
    (26,.,.) = 
      0.7606  0.2071
      0.0155  0.0169
    
    (27,.,.) = 
      0.9166  0.0330
      0.0440  0.0063
    
    (28,.,.) = 
      0.7142  0.2471
      0.0163  0.0225
    
    (29,.,.) = 
      0.9195  0.0277
      0.0471  0.0057
    
    (30,.,.) = 
      0.7946  0.1514
      0.0307  0.0234
    
    (31,.,.) = 
      0.7609  0.1391
      0.0578  0.0423
    
    (32,.,.) = 
      0.8271  0.0200
      0.1394  0.0135
    
    (33,.,.) = 
      0.7151  0.1661
      0.0616  0.0572
    
    (34,.,.) = 
      0.8256  0.0167
      0.1459  0.0118
    
    (35,.,.) = 
      0.7448  0.0953
      0.1058  0.0541
    
    (36,.,.) = 
      0.7511  0.0132
      0.2201  0.0155
    
    (37,.,.) = 
      0.6947  0.1176
      0.1119  0.0758
    
    (38,.,.) = 
      0.7483  0.0110
      0.2272  0.0134
    
    (39,.,.) = 
      0.6925  0.0645
      0.1770  0.0660
    
    (40,.,.) = 
      0.7107  0.2602
      0.0118  0.0173
    
    (41,.,.) = 
      0.9306  0.0297
      0.0353  0.0045
    
    (42,.,.) = 
      0.7983  0.1609
      0.0226  0.0182
    
    (43,.,.) = 
      0.7015  0.0118
      0.2687  0.0180
    
    (44,.,.) = 
      0.6422  0.0681
      0.2035  0.0862
    
    (45,.,.) = 
      0.7980  0.1677
      0.0186  0.0157
    [torch.DoubleTensor of dimension 45x2x2]
    
    <gm.testme> log(Z):	
    33.959981875217	




Based on http://code.cogbits.com/wiki/doku.php?id=tutorial_graphicalmodels
