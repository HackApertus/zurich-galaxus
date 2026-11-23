# Hack Apertus x Galaxus: product image classification

This file describes the Galaxus challenge. Data, leaderboard, evaluation and the competition rules are on Kaggle:
[kaggle.com/competitions/hackapertus-galaxus](https://www.kaggle.com/competitions/hackapertus-galaxus).

## Why this matters

Galaxus shows millions of product images. Knowing what each image shows decides which image comes first in the
gallery, which images are hidden, and where text overlays or packaging shots belong. Today a finetuned Qwen 3.5 9B
labels every new image. Build an Apertus v1.5 method that can replace it: as accurate, fast and cheap enough for
more than 100,000 images per day, and deployable on-premises, air-gapped or in a sovereign Swiss cloud.

## The task

For every image, answer one class per dimension, 41 classes in total:

| Dimension | Question |
| --- | --- |
| subject | What does the image show? |
| background | What is behind the product? |
| perspective | From where is the product seen? |
| individuals | Is a real person or animal in the photo? |
| text_classification | Which kind of text overlay does the image carry? |
| text_language | Which language dominates the text? |

Each image comes with product context from our catalog: title, product type, brand, category and subcategory. The
classes, their definitions and the labeling conventions are in labeling_guideline.md in the
[Kaggle data](https://www.kaggle.com/competitions/hackapertus-galaxus/data).

## Where to find what

| Where | What |
| --- | --- |
| This repository | the challenge, model rules, timeline, what you hand in, how we assess |
| [Kaggle data](https://www.kaggle.com/competitions/hackapertus-galaxus/data) | images, labels, labeling guideline, scoring code, the encrypted final images |
| [Kaggle evaluation](https://www.kaggle.com/competitions/hackapertus-galaxus/overview/evaluation) | metric, scored cells, classes, submission file, baselines |
| [Kaggle leaderboard](https://www.kaggle.com/competitions/hackapertus-galaxus/leaderboard) | public on the dev images, private on the final images |
| [Kaggle rules](https://www.kaggle.com/competitions/hackapertus-galaxus/rules) | the competition rules, binding for everyone who joins |
| Hack Apertus Discord, Galaxus challenge channel | questions, commit hashes, the passphrase of the final images |
| [sprintd](https://app.sprintd.org/events/hack-apertus-zurich) | your project submission |

## Models

The [model and infrastructure rules of Hack Apertus](https://hackapertus.notion.site/zurich-challenges-model-requirements)
apply. In short:

- Apertus v1.5 (8B or 70B), also finetuned or adapted, performs the classification.
- During development you may use any tools and models, including closed models, commercial APIs and coding
  assistants, for example to create training targets. Pseudo-labels on the unlabeled dev images are allowed.
- At runtime your method uses only Apertus and, for supporting tasks, other open-weights models (for example an
  image encoder or OCR). Apertus may run self-hosted or through the inference API that Hack Apertus provides.
- The solution is deployable on-premises, air-gapped or in a sovereign Swiss cloud, and your technical report lists
  every model you used.

The Kaggle rules (Section B(6)) also forbid labeling dev or final images by hand and looking up the products in the
Galaxus shop or by reverse image search.

## Timeline

| When (CET) | What |
| --- | --- |
| Fri 27 Nov, 09:00 | Challenge pitches, then the deep dive with the matched teams. Join the Kaggle competition with the link you get there, accept its rules, and name your Kaggle team like your Hack Apertus team. Download the data, including final_encrypted.bin. |
| During the hack | Train, and submit dev predictions to the public leaderboard (20 per day). |
| Sat 28 Nov, before 13:30 | Post your commit hash and the revision of your published model weights in the Galaxus challenge channel on Discord. If you use Apertus unchanged, the commit hash is enough. |
| Sat 28 Nov, 13:30 | We publish the passphrase of the final images in Discord and in the Discussion tab on Kaggle. Run your method on them: 5,292 images in under an hour need about 2 images per second. |
| Sat 28 Nov, 14:30 | Deadline for the Kaggle submission and for your project submission on sprintd. |

How to decrypt the final images and build the final submission file is described on Kaggle, under
[Data](https://www.kaggle.com/competitions/hackapertus-galaxus/data) and
[Evaluation](https://www.kaggle.com/competitions/hackapertus-galaxus/overview/evaluation).

## What you hand in

By 14:30 on 28 November:

1. Your Kaggle submission with the predictions for the final images, selected for the final ranking.
2. Your project on [sprintd](https://app.sprintd.org/events/hack-apertus-zurich), as Hack Apertus requires and the
   [README](README.md) describes: this repository (public), the demo video, the pitch deck and the technical report.

For this challenge the technical report also fills in section 7 of [technical_report.md](technical_report.md): the
public score of your selected submission (we take the private score from Kaggle), the macro F1 per dimension, what had
the biggest impact (ablations welcome), every model used, and the resources (hardware, images per second, estimated
cost of 100,000 images per day). The README of your repository explains setup, training and inference with pinned
dependencies, configs and seeds. Publish your model weights, for example on Hugging Face, and link them there.

After 13:30 you may fix your code, for example file paths, crashes or the output format, and show the changes as a
diff. You may not train again or change the weights. We may rerun any team's code on the final images from the posted
commit, weights and diff; a score that does not reproduce within its performance group may be disqualified
([Kaggle rules](https://www.kaggle.com/competitions/hackapertus-galaxus/rules), Section B(7)).

## How we assess

The Kaggle ranking is the private leaderboard. For the assessment of this challenge, its score is the performance
part, counted in groups against our baselines, whose scores are on the
[Kaggle Evaluation page](https://www.kaggle.com/competitions/hackapertus-galaxus/overview/evaluation): better than our
finetuned Qwen 3.5 9B, between Gemini 3.8 Flash and Qwen 3.5 9B, or below Gemini 3.8 Flash. Differences of a few
tenths of a percent are not decisive. You may focus on some dimensions, but the performance part uses the mean over
all six.

| Part | Weight |
| --- | --- |
| Performance on the final images | about 30% |
| Innovation: how far the approach goes beyond training longer | 20% |
| Speed and resource usage: images per second, memory, cost per 100,000 images | 20% |
| Insights: clear evidence of what mattered and what did not | 15% |
| Code quality and practicality | 15% |

Very good perspective scores and a list of suspected label errors in train.csv (sent to the hosts, not committed)
earn a bonus. The weights are a guideline: in close cases we prefer the solution that is practical for us to adopt,
and we explain our reasoning. The awards and the advance to the Zurich finals follow the Hack Apertus process.

## The data stays private

The competition data and its labels are for this hackathon only: no commercial use, no passing them on, also not to
other teams, and delete them afterwards. Your code and model weights are open source under the Hack Apertus T&C, but
this repository and your published weights never contain images, csv files, labels or the passphrase. The
.gitignore of this template already keeps the data files out. The
[Kaggle competition rules](https://www.kaggle.com/competitions/hackapertus-galaxus/rules) are binding.

## Questions

Ask in the Galaxus challenge channel of the Hack Apertus Discord or in the
[Discussion tab](https://www.kaggle.com/competitions/hackapertus-galaxus/discussion) on Kaggle. The Galaxus hosts are
on site on 27 November.
