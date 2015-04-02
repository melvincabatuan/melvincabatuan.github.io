---
layout: post
title: Local Binary Pattern 
---

> "*They who are acquainted with the present state of the theory of Symbolical Algebra, are aware, that the validity of the processes of analysis does not depend upon the interpretation of the symbols which are employed, but solely upon the laws of their combination.*"  
                                       George Boole


    from __future__ import print_function
    import numpy as np
    import matplotlib.pyplot as plt
    %matplotlib inline


    METHOD = 'uniform'
    plt.rcParams['font.size'] = 9
    
    
    def plot_circle(ax, center, radius, color):
        circle = plt.Circle(center, radius, facecolor=color, edgecolor='0.5')
        ax.add_patch(circle)
    
    
    def plot_lbp_model(ax, binary_values):
        """Draw the schematic for a local binary pattern."""
        # Geometry spec
        theta = np.deg2rad(45)
        R = 1
        r = 0.15
        w = 1.5
        gray = '0.5'
    
        # Draw the central pixel.
        plot_circle(ax, (0, 0), radius=r, color=gray)
        # Draw the surrounding pixels.
        for i, facecolor in enumerate(binary_values):
            x = R * np.cos(i * theta)
            y = R * np.sin(i * theta)
            plot_circle(ax, (x, y), radius=r, color=str(facecolor))
    
        # Draw the pixel grid.
        for x in np.linspace(-w, w, 4):
            ax.axvline(x, color=gray)
            ax.axhline(x, color=gray)
    
        # Tweak the layout.
        ax.axis('image')
        ax.axis('off')
        size = w + 0.2
        ax.set_xlim(-size, size)
        ax.set_ylim(-size, size)
    
    
    fig, axes = plt.subplots(ncols=5, figsize=(16, 4))
    
    titles = ['flat', 'flat', 'edge', 'corner', 'non-uniform']
    
    binary_patterns = [np.zeros(8),
                       np.ones(8),
                       np.hstack([np.ones(4), np.zeros(4)]),
                       np.hstack([np.zeros(3), np.ones(5)]),
                       [1, 0, 0, 1, 1, 1, 0, 0]]
    
    for ax, values, name in zip(axes, binary_patterns, titles):
        plot_lbp_model(ax, values)
        ax.set_title(name)


![_config.yml]({{ site.baseurl}}/images/Local-Binary-Pattern_2_0.png)



    from skimage.transform import rotate
    from skimage.feature import local_binary_pattern
    from skimage import data
    from skimage.color import label2rgb
    
    # settings for LBP
    radius = 3
    n_points = 8 * radius
    
    
    def overlay_labels(image, lbp, labels):
        mask = np.logical_or.reduce([lbp == each for each in labels])
        return label2rgb(mask, image=image, bg_label=0, alpha=0.5)
    
    
    def highlight_bars(bars, indexes):
        for i in indexes:
            bars[i].set_facecolor('r')
    
    
    image = data.load('brick.png')
    lbp = local_binary_pattern(image, n_points, radius, METHOD)
    
    def hist(ax, lbp):
        n_bins = lbp.max() + 1
        return ax.hist(lbp.ravel(), normed=True, bins=n_bins, range=(0, n_bins),
                       facecolor='0.5')
    
    # plot histograms of LBP of textures
    fig, (ax_img, ax_hist) = plt.subplots(nrows=2, ncols=3, figsize=(16, 4))
    plt.gray()
    
    titles = ('edge', 'flat', 'corner')
    w = width = radius - 1
    edge_labels = range(n_points // 2 - w, n_points // 2 + w + 1)
    flat_labels = list(range(0, w + 1)) + list(range(n_points - w, n_points + 2))
    i_14 = n_points // 4            # 1/4th of the histogram
    i_34 = 3 * (n_points // 4)      # 3/4th of the histogram
    corner_labels = (list(range(i_14 - w, i_14 + w + 1)) +
                     list(range(i_34 - w, i_34 + w + 1)))
    
    label_sets = (edge_labels, flat_labels, corner_labels)
    
    for ax, labels in zip(ax_img, label_sets):
        ax.imshow(overlay_labels(image, lbp, labels))
    
    for ax, labels, name in zip(ax_hist, label_sets, titles):
        counts, _, bars = hist(ax, lbp)
        highlight_bars(bars, labels)
        ax.set_ylim(ymax=np.max(counts[:-1]))
        ax.set_xlim(xmax=n_points + 2)
        ax.set_title(name)
    
    ax_hist[0].set_ylabel('Percentage')
    for ax in ax_img:
        ax.axis('off')


![_config.yml]({{ site.baseurl}}/images/Local-Binary-Pattern_3_0.png)



    from skimage.transform import rotate
    from skimage.feature import local_binary_pattern
    from skimage import data
    from skimage import io
    from skimage.color import label2rgb
    from skimage.color import rgb2gray
    
    # settings for LBP
    radius = 3
    n_points = 8 * radius
    
    
    def overlay_labels(image, lbp, labels):
        mask = np.logical_or.reduce([lbp == each for each in labels])
        return label2rgb(mask, image=image, bg_label=0, alpha=0.5)
    
    
    def highlight_bars(bars, indexes):
        for i in indexes:
            bars[i].set_facecolor('r')
    
    
    src = io.imread('/home/cobalt/Pictures/dovahkin.png')
    image = rgb2gray(src)
    lbp = local_binary_pattern(image, n_points, radius, METHOD)
    plt.imshow(lbp);
    
    def hist(ax, lbp):
        n_bins = lbp.max() + 1
        return ax.hist(lbp.ravel(), normed=True, bins=n_bins, range=(0, n_bins),
                       facecolor='0.5')
    
    # plot histograms of LBP of textures
    fig, (ax_img, ax_hist) = plt.subplots(nrows=2, ncols=3, figsize=(16, 4))
    plt.gray()
    
    titles = ('edge', 'flat', 'corner')
    w = width = radius - 1
    edge_labels = range(n_points // 2 - w, n_points // 2 + w + 1)
    flat_labels = list(range(0, w + 1)) + list(range(n_points - w, n_points + 2))
    i_14 = n_points // 4            # 1/4th of the histogram
    i_34 = 3 * (n_points // 4)      # 3/4th of the histogram
    corner_labels = (list(range(i_14 - w, i_14 + w + 1)) +
                     list(range(i_34 - w, i_34 + w + 1)))
    
    label_sets = (edge_labels, flat_labels, corner_labels)
    
    for ax, labels in zip(ax_img, label_sets):
        ax.imshow(overlay_labels(image, lbp, labels))
    
    for ax, labels, name in zip(ax_hist, label_sets, titles):
        counts, _, bars = hist(ax, lbp)
        highlight_bars(bars, labels)
        ax.set_ylim(ymax=np.max(counts[:-1]))
        ax.set_xlim(xmax=n_points + 2)
        ax.set_title(name)
    
    ax_hist[0].set_ylabel('Percentage')
    for ax in ax_img:
        ax.axis('off')


![_config.yml]({{ site.baseurl}}/images/Local-Binary-Pattern_4_0.png)



![_config.yml]({{ site.baseurl}}/images/Local-Binary-Pattern_4_1.png)


Didn't perform well.


    # settings for LBP
    radius = 2
    n_points = 8 * radius
    
    
    def kullback_leibler_divergence(p, q):
        p = np.asarray(p)
        q = np.asarray(q)
        filt = np.logical_and(p != 0, q != 0)
        return np.sum(p[filt] * np.log2(p[filt] / q[filt]))
    
    
    def match(refs, img):
        best_score = 10
        best_name = None
        lbp = local_binary_pattern(img, n_points, radius, METHOD)
        n_bins = lbp.max() + 1
        hist, _ = np.histogram(lbp, normed=True, bins=n_bins, range=(0, n_bins))
        for name, ref in refs.items():
            ref_hist, _ = np.histogram(ref, normed=True, bins=n_bins,
                                       range=(0, n_bins))
            score = kullback_leibler_divergence(hist, ref_hist)
            if score < best_score:
                best_score = score
                best_name = name
        return best_name
    
    
    brick = data.load('brick.png')
    grass = data.load('grass.png')
    wall = data.load('rough-wall.png')
    
    refs = {
        'brick': local_binary_pattern(brick, n_points, radius, METHOD),
        'grass': local_binary_pattern(grass, n_points, radius, METHOD),
        'wall': local_binary_pattern(wall, n_points, radius, METHOD)
    }
    
    # classify rotated textures
    print('Rotated images matched against references using LBP:')
    print('original: brick, rotated: 30deg, match result: ',
          match(refs, rotate(brick, angle=30, resize=False)))
    print('original: brick, rotated: 70deg, match result: ',
          match(refs, rotate(brick, angle=70, resize=False)))
    print('original: grass, rotated: 145deg, match result: ',
          match(refs, rotate(grass, angle=145, resize=False)))
    
    # plot histograms of LBP of textures
    fig, ((ax1, ax2, ax3), (ax4, ax5, ax6)) = plt.subplots(nrows=2, ncols=3,
                                                           figsize=(9, 6))
    plt.gray()
    
    ax1.imshow(brick)
    ax1.axis('off')
    hist(ax4, refs['brick'])
    ax4.set_ylabel('Percentage')
    
    ax2.imshow(grass)
    ax2.axis('off')
    hist(ax5, refs['grass'])
    ax5.set_xlabel('Uniform LBP values')
    
    ax3.imshow(wall)
    ax3.axis('off')
    hist(ax6, refs['wall'])

    Rotated images matched against references using LBP:
    original: brick, rotated: 30deg, match result:  brick
    original: brick, rotated: 70deg, match result:  brick
    original: grass, rotated: 145deg, match result:  grass





    (array([ 0.06064224,  0.03585434,  0.03443527,  0.03126526,  0.03159714,
             0.03417969,  0.04234695,  0.05599594,  0.06238937,  0.05499268,
             0.04071808,  0.03316879,  0.03037643,  0.03200531,  0.0348587 ,
             0.03236771,  0.06035614,  0.29244995]),
     array([  0.,   1.,   2.,   3.,   4.,   5.,   6.,   7.,   8.,   9.,  10.,
             11.,  12.,  13.,  14.,  15.,  16.,  17.,  18.]),
     <a list of 18 Patch objects>)




![_config.yml]({{ site.baseurl}}/images/Local-Binary-Pattern_6_2.png)


# Reference:
    [1.] http://scikit-image.org/docs/dev/auto_examples/plot_local_binary_pattern.html
