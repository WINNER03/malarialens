# MalariaLens

A deep learning model that detects malaria parasites in red blood cell images, built as a first step toward a phone-based diagnostic tool for rural clinics in West Africa.

## Why
Malaria diagnosis still relies on trained microscopists counting infected cells by hand, which is slow, and those experts are scarce in rural clinics. MalariaLens explores whether a lightweight model can flag infected cells automatically.

## What it does
- Classifies a single blood-cell image as **parasitized** or **uninfected**
- Includes a web demo (Gradio): upload an image and get a prediction with confidence

## How it works
- **Data:** NIH malaria dataset, 27,558 labeled cell images (80/10/10 train/val/test split)
- **Model:** MobileNetV2 pretrained on ImageNet, chosen because it is small enough to run on phones
- **Training:** transfer learning (frozen base), then fine-tuning the top layers with a low learning rate, data augmentation (flips, rotations), and early stopping
- **Evaluation:** accuracy, **sensitivity** and **specificity** on a held-out test set. Sensitivity is the priority, since a missed infection is worse than a false alarm.

## Results (held-out test set)
| Metric | Score |
|---|---|
| Accuracy | 93.76% |
| Sensitivity | 89.06% (infected cells caught) |
| Specificity | 98.41% (healthy cells correctly cleared) |

Evaluated on 2,756 held-out test images the model never saw during training.

**What this tells me:** the model is conservative. It rarely raises a false alarm, but it misses about 11% of infected cells. For diagnosis that tradeoff is the wrong way round, so the next improvement is lowering the decision threshold to trade some specificity for higher sensitivity.

## Demo

**[🔬 Try MalariaLens live on Hugging Face](https://huggingface.co/spaces/jadjano/malarialens)**

<img width="2296" height="1069" alt="image" src="https://github.com/user-attachments/assets/1d6bc302-a7fb-4c69-9991-9d99da06a49b" />




## Run it
Open `MalariaLens.ipynb` in Google Colab, set the runtime to T4 GPU, then Run all.

## Next steps
- Tune the decision threshold to push sensitivity above 95%
- Segment every cell in a full blood-smear photo and compute parasite density
- Offline mobile app for clinics
- French and local-language reports

## Tech
Python, TensorFlow/Keras, TensorFlow Datasets, Gradio, Google Colab
