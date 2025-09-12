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

Machine learning models are typically embedded as one part of software applications. Different individuals/teams may work on different parts of the software with the people working on the non-machine learning components not needing machine learning expertise to work on their components effectively. There are some things to keep in mind when working on the non-machine learning components of such systems, however, that run counter to standard practices when machine learning is not involved in the system. This blog post highlights a few of these cases for the preparation of data input to machine learning models.

## Case 1: When Fixing a Bug in Model Data Preparation

First, let's talk about the surprising case of how fixing a bug in the data preparation for a machine learning model can actually make your model perform worse (if the model is not simultaneously updated).

When making a software product without machine learning, there is often significant data manipulation involved to prepare data for a model to predict on. As with any software, there could be bugs in the data preperation logic when first shipped. When such a bug is disovered in systems without a machine learning component, fixing it by itself is a natural, valuable thing to do. When the data manipulation is used in preparing data to be input to a machine learning model, however, this natural response to quickly correct the data manipulation to be as intended is often not the best idea unless one also takes into account how it impacts the model. In fact, fixing the data manipulation logic without updating the machine learning model is likely to leave us in a worse state than taking no action. To understand why, let's first take a brief digression on the related phenomenon of the impact of different visual data preparations on human ability to carry out routine tasks.

### Seeing Upside Down

In the middle of the 20th century, Theodor Erismann and Ivo Kohler conducted experiments where a subject person was given specially-designed goggles which flipped their vision with top becoming bottom and bottom top. 

Naturally, when people began wearing these goggles, they had great difficulty with tasks they would have accomplished easily in the absence of this visual alteration. In a simple fencing game, the subject would defend high when he needed to defend low and vice versa. Even grasping an object that was presenting to him and walking down stairs were a challenge. Remarkably, however, after some time, the subjects adjusted to these dramatically altered visual conditions and, after 10 days, could navigate everyday tasks using the goggles with ease. Perhaps even more interestingly, the subjects, once they removed the goggles, had to undergo a period of adjustment back to normal vision.

### How this Relates to Data Prepared for Machine Learning Models

Machine learning models, like the human brain are able to work with data that has not been prepared as intended. Just as our brain can learn to process upside down images, machine learning models can learn to predict well on data that has not been prepared as intended, such as when a feature has been multiplied by negative one. Importantly, like brains, machine learning models require that the data that they are predicting on be prepared in the same way as the data they have learned on in order to perform well. If a feature that was negative when it was supposed to be positive for the data that a model learned from is updated to be correctly calculated, then it will cause the model to make inaccurate predictions in the same way that a person who has learned to navigate the world with an upside-down view will initially stumble when returned to normal vision. Unlike brains, however, machine learning models do not automatically learn to process the data prepared in different ways as soon as they are exposed to it. The model will not inherently go from initially making inaccurate predictions when the data processing has been corrected to learning how to deal with it and again making accurate predictions. Instead, the model must be explicitly updated to accurately predict on the correctly processed data. To prevent model performance degradation upon making this update, the fix to the data prepartion and model should be made at the same time in the system.

## Case 2: When Expanding or Reducing the Range of Data a Model Predicts on

Next, let's talk about considerations for when we change the range of data a model predicts on. Generally, it is best to only cautiously expand the range of data a model predicts on from that which was originally intended. When developed well, models have a defined range of conditions that they are known to operate well in. They may operate well in a greater range of conditions than this, but it is not guaranteed, and can result in reduced performance. Reducing the range of data a model is exposed to, in contrast, is typically a safe operation from a model performance standpoint, but can result in false alarms and surprising findings in model monitoring if not accounted for.

### Expanding the Range of Data a Model Predicts on

When developing a normal software product, we typically try to get it to behave in desirable ways over the full range of its inputs. We think deeply about edge and corner cases that are rarely encountered and ensure that the program logic handles them correctly. We even think about what unintended on invalid inputs our program could receive and how it could fail gracefully in their presence. Given this deep design into how a program should behave over its full range of possible inputs, it is often safe to extend the range of data it runs on without surprising consequences.

Machine learning models have become common pieces of software products due to their abilities to solve tasks that rules-based software struggles with. With these new abilities, though, does come some downsides that are not present with rules-based software. Machine learning models, in contrast to normal software, are not designed to or even expected to operate sensibly over their full range of possible inputs. In contrast, there is typically a range of input data over which a model is believed to perform acceptably. This is due to the finiteness of the range of data over which they have been trained and evaluated on to perform satisfactorily. Just as extensive physics education brings no guarantee that a person will do well on a biology exam, extensively training a model over a particular range of data brings no guarantee that it will perform well on new, previously unseen ranges of data.

This has important implications for when the range of data that a model is predicting on needs to be expanded. This may occur when new demographics are exposed to the system, for example. It also can occur naturally over time as data drift when circumstances change over time. In any case, it is important that as the range of data fed into a model is expanded, we also ensure that the model continues to perform acceptably on the new range of data, and update the model if it is needed.

### Reducing the Range of Data a Model Predicts on

In contrast to the case of 

## Conclusions

If you are used to working on software systems without machine learning components, you are likely used to operating in certain time-tested manners. These practices mostly still work well when machine learning components are added to the software system, but there are some cases that warrant re-examination and changes in common practices when there is a machine learning component in your software system. One example discussed above was the importance of considering the impact on the model when fixing bugs in data preparation logic for machine learning models. The second example was the importance of updating model evaluation and monitoring when the range of data that a model predicts on is reduced or expanded.



### Reference:

https://www.theguardian.com/education/2012/nov/12/improbable-research-seeing-upside-down 

http://users.sussex.ac.uk/~ezequiel/AS/Kohler_1962.pdf

