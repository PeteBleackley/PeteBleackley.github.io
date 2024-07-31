title: Data Science Notebooks -Is It A Mushroom or Is It A Toadstool?

#Is It A Mushroom or Is It A Toadstool?

The [UCI Machine Learning Mushroom Classification Dataset](https://www.kaggle.com/uciml/mushroom-classification) on Kaggle tabulates discrete features around 8000 specimens of fungi. There are 23 species represented, and the challenge is to classify which are edible and which are poisonous. Since the data are all categorical, I decided that a Bayesian Belief Network would be a suitable, and used an ad-hoc clustering algorithm to infer a hidden variable.

<iframe frameborder="0" height="800" scrolling="auto" src="https://www.kaggle.com/embed/petebleackley/bayesian-belief-network-for-fungus-edibility?kernelSessionId=1503132" title="Bayesian Belief Network for fungus edibility" width="100%"></iframe>

These results seem promising, but I wanted to see if I could do even better. This time I used Mutual Information to infer two hidden variables.

<iframe frameborder="0" height="800" scrolling="auto" src="https://www.kaggle.com/embed/petebleackley/bayesian-belief-network-for-fungi-2?kernelSessionId=6991228" title="Bayesian Belief Network for Fungi 2" width="100%"></iframe>

**WARNING** This is intended solely as a technology demonstration. I cannot accept any liability if you pick wild mushrooms on the basis of these notebooks. If you want to forage for wild mushrooms, find an experienced guide.

by [Dr Peter J Bleackley]({% link index.md %})

[Data Science Notebooks]({% link Data-Science-Notebooks/index.md %})
