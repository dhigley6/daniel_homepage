---
layout: ../../layouts/MarkdownPostLayout.astro
title: 'New Things to Consider when Preparing Data for Machine Learning Models'
pubDate: 2025-09-12
description: 'As a person working with components that interact with machine learning systems or using their outputs, there are some important things to know in order to use them most effectively.'
author: 'Daniel Higley'
image:
    url: 'https://docs.astro.build/assets/rose.webp'
    alt: 'The Astro logo on a dark background with a pink glow.'
---

Machine learning models are typically not used in isolation, but rather embedded as parts of software applications. Parts of these systems, such as the data preparation for machine learning models, are very similar to components of normal software systems without machine learning components. There are some important things to keep in mind when working on the non-machine learning components of such systems, however, which run counter to standard practices when machine learning is not part of the system. This post highlights a few such cases for preparation of data input to machine learning models.

## Case 1: Handling Bugs in Model Data Preparation

First, let's examine the case of how fixing a bug in the data preparation for a machine learning model without additional model changes could actually make your model perform worse. To understand why, let's first take a brief digression on the related phenomenon of the impact of different visual data preparations on human ability to carry out routine tasks.

### Seeing Upside Down

In the middle of the 20th century, Theodor Erismann and Ivo Kohler conducted experiments where a subject was given specially-designed goggles which flipped their vision with top becoming bottom and bottom top. Naturally, when people began wearing these goggles, they had great difficulty with everyday tasks that they could accomplish easily in the absence of this visual alteration. In a simple fencing game, for example, the subject would defend high when he needed to defend low and vice versa. Even grasping an object that was presenting to him and walking down stairs were a challenge. Remarkably, however, after some time, the subjects adjusted to these dramatically altered visual conditions and, after 10 days, could navigate everyday tasks using the goggles with ease. Perhaps even more interestingly, the subjects, once they removed the goggles, had to undergo a period of adjustment back to normal vision.

### Experiment with Machine Learning

...

### How this Relates to Data Prepared for Machine Learning Models

Machine learning models, like humans, are able to learn from and work with data that has not been prepared as intended. Similar to how our brain can learn to process upside down images, machine learning models can learn to predict well on data that has not been prepared as intended, such as when a feature is the negative of its intended value. Importantly, however, like brains, machine learning models require that the data that they are predicting on be prepared in the same way as the data they have learned on in order to perform well. If a feature that was negative when it was supposed to be positive for the data that a model learned from is updated to be correctly calculated, then it will cause the model to make inaccurate predictions in the same way that a person who has learned to navigate the world with an upside-down view will initially stumble when returned to normal vision. Unlike brains, however, machine learning models do not automatically learn to process the data prepared in different ways as soon as they are exposed to it. The model will not inherently go from initially making inaccurate predictions when the data processing has been corrected to learning how to deal with it and again making accurate predictions. Instead, the model must be explicitly updated to accurately predict on the correctly processed data, which typically involves retraining the model. To prevent model performance degradation upon making this update, the fix to the data prepartion and model should be made at the same time in the system.

## Case 2: When Expanding or Reducing the Range of Data a Model Predicts on

Next, let's talk about considerations for when we change the range of data a model predicts on. Generally, it is best to only cautiously expand the range of data a model predicts on from that which was originally intended. When developed well, models have a defined range of conditions that they are known to operate well in. They may operate well in a greater range of conditions than this, but it is not guaranteed, and can result in reduced performance. Reducing the range of data a model is exposed to, in contrast, is typically a safe operation from a model performance standpoint, but can result in false alarms and surprising findings in model monitoring if not accounted for.

### Expanding the Range of Data a Model Predicts on

When developing a normal software product, we typically try to get it to behave in desirable ways over the full range of its inputs. We think deeply about edge and corner cases that are rarely encountered and ensure that the program logic handles them correctly. We even think about what unintended on invalid inputs our program could receive and how it could fail gracefully in their presence. Given this deep design into how a program should behave over its full range of possible inputs, it is often safe to extend the range of data it runs on without surprising consequences.

Machine learning models have become common pieces of software products due to their abilities to solve tasks that rules-based software struggles with. With these new abilities, though, does come some downsides that are not present with rules-based software. Machine learning models, in contrast to normal software, are not designed to or even expected to operate sensibly over their full range of possible inputs. In contrast, there is typically a range of input data over which a model is believed to perform acceptably. This is due to the finiteness of the range of data over which they have been trained and evaluated on to perform satisfactorily. Just as extensive physics education brings no guarantee that a person will do well on a biology exam, extensively training a model over a particular range of data brings no guarantee that it will perform well on new, previously unseen ranges of data.

This has important implications for when the range of data that a model is predicting on needs to be expanded. This may occur when new demographics are exposed to the system, for example. It also can occur naturally over time as data drift when circumstances change over time. In any case, it is important that as the range of data fed into a model is expanded, we also ensure that the model continues to perform acceptably on the new range of data, and update the model if it is needed.

### Reducing the Range of Data a Model Predicts on

In contrast to the case of expanding the range of data a model predicts on, reducing the range of data a model predicts on is not typically harmful to the quality of the software system. If built well, the machine learning model component should perform acceptably on the whole range of data it was exposed to, including the new, narrower scope. This can cause surprises in model evaluation and monitoring, though. First, the population of data the model is exposed to is different than what it was evaluated on and its performance on this new data is naturally going to be different as a result. Second, and relatedly, metrics and statistics that are being monitored about the model to detect potential problems will naturally change. Statistics like the proportion of observations predicted as belonging to a certain class will likely change. These changes require informing the people/systems responsible for model monitoring and acting accordingly. A simple fix would be to reset baseline expectations for model behavior with new data collected shortly after the data processing change and monitor for changes from there.

## Conclusions

If you are used to working on software systems without machine learning components, you are likely used to operating in certain time-tested manners. These practices mostly still work well when machine learning components are added to the software system, but there are some cases that warrant re-examination and changes in common practices when there is a machine learning component in your software system. One example discussed above was the importance of considering the impact on the model when fixing bugs in data preparation logic for machine learning models. The second example was the importance of updating model evaluation and monitoring when the range of data that a model predicts on is reduced or expanded.



### Reference:

https://www.theguardian.com/education/2012/nov/12/improbable-research-seeing-upside-down 

http://users.sussex.ac.uk/~ezequiel/AS/Kohler_1962.pdf

