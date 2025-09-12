---
layout: ../../layouts/MarkdownPostLayout.astro
title: 'Important things to know about Machine Learning-Based Products for Machine Learning-Adjacent Practitioners'
pubDate: 2025-09-11
description: 'As a person working with components that interact with machine learning systems or using their outputs, there are some important things to know in order to use them most effectively.'
author: 'Daniel Higley'
image:
    url: 'https://docs.astro.build/assets/rose.webp'
    alt: 'The Astro logo on a dark background with a pink glow.'
---

Machine learning models are often embedded within software products. Different individuals/teams may work on different parts of the software with the people working on the non-machine learning components not needing machine learning expertise to work on their components effectively. This division works well in most cases, but there are some things to keep in mind when working on such components that may run counter to best practices when machine learning is not involved in the system. This blog post highlights a few of these cases.

## Don't Apply a Fix to Model Data Preparation without Also Updating the Model

When making a software product without machine learning, there may be a lot of data manipulation involved to prepare data for a user to view. There could be bugs in the data manipulation logic when first shipped. Later, someone could discover a bug where data manipulation is not applied exactly as intended. In such cases, it makes perfect sense to fix the bug and change the data manipulation logic to be as intended as soon as is practicable. If the data manipulation is used in preparing data to be input to a machine learning model, however, our natural instinct to correct the data manipulation to be as intended is likely not the best idea. In fact, fixing the data manipulation logic without updating the machine learning model is likely to leave us in a worse state than taking no action. To understand why, let's first take a brief digression in understanding the impact of different visual data preparations on human ability to carry out routine tasks.

### Seeing Everything Upside Down

In the middle of the 20th century, Theodor Erismann and Ivo Kohler conducted experiments where a subject person was given specially-designed goggles which flipped their vision with top becoming bottom and bottom top. 

Naturally, when people began wearing these goggles, they had great difficulty with tasks they would have accomplished easily in the absence of this visual alteration. In a simple fencing game, the subject would defend high when he needed to defend low and vice versa. Even grasping an object that was presenting to him and walking down stairs were a challenge. Remarkably, however, after some time, the subjects adjusted to these dramatically altered visual conditions and, after 10 days, could navigate everyday tasks using the goggles with ease. Perhaps even more interestingly, the subjects, once they removed the goggles, had to undergo a period of adjustment back to normal vision.

### How this Relates to Data Prepared for Machine Learning Models

Machine learning models, like the human brain are able to work with data that has not been prepared as intended. Just as our brain can learn to process upside down images, machine learning models can learn to predict well on data that has not been prepared as intended. Importantly, like brains, machine learning models require a learning period to predict accurately on data prepared in different ways. If a machine learning model trained on data prepared in one manner is used to predict on data prepared in another manner, it will perform worse, potentially even catastrophically so. Unlike brains, however, machine learning models do not automatically learn to process the data prepared in different ways as soon as they are exposed to it. Instead, 

### Conclusions

If you are used to working on software systems without machine learning components, you may be used to fixing bugs where data is not processed as intended as soon as discovered and without other changes to the system. When the data processing is used to prepare data for a machine learning model, however, it is critical to also update the machine learning model when fixing such bugs. Not doing so could result in decreased model performance.



### Reference:

https://www.theguardian.com/education/2012/nov/12/improbable-research-seeing-upside-down 

http://users.sussex.ac.uk/~ezequiel/AS/Kohler_1962.pdf

