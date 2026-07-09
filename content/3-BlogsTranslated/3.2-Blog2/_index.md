---
title: "Blog 2"
date: 2026-07-09
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Learning About Amazon Nova Forge - A Solution for Building Custom Frontier Models for Businesses

Currently, the most common way for businesses to "teach" AI to understand internal data is fine-tuning, which means feeding company data into a model so it can learn from it. That sounds reasonable, but the problem is this: the more new data you feed into the model, the higher the risk that it will "forget" the foundational skills it already has, such as reasoning, natural language understanding, and logical analysis. This phenomenon is called Catastrophic Forgetting.

So what about building a model from scratch? The cost can reach millions of dollars, and it requires a team of experts and massive infrastructure. Not every business can do that. This is the problem most businesses are stuck with: they need AI that deeply understands internal data, but they do not have a way that is both effective and cost-efficient. And that is why Amazon Nova Forge was created.

**Amazon Nova Forge** is an AWS service that allows businesses to build their own frontier AI models, deeply trained on industry-specific data. The core difference compared with traditional approaches is this: instead of shallow fine-tuning at the surface layer, or building everything expensively from zero, Nova Forge lets you start from an existing Amazon Nova checkpoint. In other words, the model has already been trained by AWS up to a certain stage, and from there you continue training it with your own internal data.

To put it more simply: AWS has already handled the hardest and most expensive part. You only need to "take over" midway and teach the model what it does not yet know about your industry.<br>
Nova Forge gives you three different starting points depending on how much internal data you have:

- *Pre-trained*: The model already has foundational knowledge - it can read, write, and reason - but it is not specialized in any particular field yet. This is suitable if the company has a large amount of internal data, because this is the stage where the model is most "hungry" for specialized industry knowledge.
- *Mid-trained*: The model is still in the refinement process and is not yet complete. It is still "open" to absorbing more information. This is suitable if the company has a moderate amount of data.
- *Post-trained*: The model is complete and ready to work. This is suitable if the company only wants to lightly adjust the model's behavior without retraining it on deep specialized knowledge.

A major risk when training AI with internal data is Catastrophic Forgetting. This means that newly added data can overwrite the model's existing knowledge, gradually causing it to lose foundational skills such as reasoning and natural language understanding.

Nova Forge solves this problem with Data Mixing. Instead of training the model only on the company's internal data, AWS allows you to mix that data with Amazon Nova's standard data at an appropriate ratio.

**The result**: the model learns the company's specialized knowledge while still preserving the general intelligence foundation it already had.

A practical example that immediately comes to mind is e-commerce. Imagine visiting the website of a cosmetics shop. A chatbot asks about your skin condition - whether your skin is oily or acne-prone - and then automatically recommends the right products for you. That chatbot is not just "searching"; it truly understands the shop's product data, policies, and customer characteristics. That is the kind of thing Nova Forge can help build.

**Conclusion**: Amazon Nova Forge is a solution that helps businesses build their own frontier model at a much lower cost than building from scratch, while training it deeply on industry-specific data aligned with the company's goals.

![Illustration](/images/3-BlogsTranslated/blog2.jpg) <small style="display: block; text-align: center;">Amazon Nova Forge</small>

Article link: [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/posts/2207272326704394/?notif_id=1783555137963774&notif_t=group_post_approved)
