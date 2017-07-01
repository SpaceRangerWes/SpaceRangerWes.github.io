---
layout: post
title: How Garbage is the Rule "I before E except after C"?
---

I had some free time over the holiday weekend, and I thought it would be a good time to put in the effort in showing those of us that speak the English language how incorrect the rule "I before E except after C" is. The following python notebook shows my findings.

```python
%matplotlib inline

import nltk
from nltk.corpus import words
from functools import reduce
```


```python
# Filter out words that don't need to be considered
filtered_list = list(filter(lambda x: "ie" in x or "ei" in x, words.words()))
```


```python
# Apply the rule "i before e except after c" and see if it is true or false
def the_rule(word):
    if 'cie' in word:
        return False
    if 'cei' in word:
        word = word.replace("cei", '')
    if 'ei' in word:
        return False
    return True

ruling_dict = dict(map(lambda word: (word, the_rule(word)), filtered_list))
```


```python
# Count the total number of rule followers and breakers
true_count = reduce(lambda count, val: count + (val == True), list(ruling_dict.values()), 0)
false_count = len(ruling_dict) - true_count

# Plot our discoveries
import matplotlib.pyplot as plt
import numpy as np

dictionary = plt.figure()

D = {u'True':true_count, u'False':false_count}

plt.bar(range(len(D)), D.values(), align='center')
plt.ylabel("Word Count")
plt.yticks(np.arange(0, len(ruling_dict), 750))
plt.xticks(range(len(D)), D.keys())
plt.title('Word follows the rule "i before e except after c"')
```


![png]({{ site.baseurl }}/images/output_3_1.png)


Conclusion: It is in no way a rule unless you consider "every American is a Republican" a rule as well.
