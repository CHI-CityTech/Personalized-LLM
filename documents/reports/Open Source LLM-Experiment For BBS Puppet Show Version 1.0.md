

![][image1]

# 

#   Open Source LLM Experiment for BBS Puppet Show

Kazi Islam 

Version 1.0

Prepared for Prof David Smith

Date \[10-05-2025\]

[**1\. Abstract	3**](#1.-abstract)

[**2\. Objective	3**](#2.-objective)

[**3\. Open Source LLMs	3**](#3.-open-source-llms)

[3.1. Reason Behind Picking These LLMs	4](#3.1.-reason-behind-picking-these-llms)

[**4\. Scope	4**](#4.-scope)

[**5\. Requirements and Constraints	5**](#5.-requirements-and-constraints)

[**6 Dataset Plan	5**](#6-dataset-plan)

[6.1 Class list	5](#6.1-class-list)

[6.2 Data augmentation (training time only)	5](#6.2-data-augmentation-\(training-time-only\))

[6.3 Testing via single-image upload	5](#6.3-testing-via-single-image-upload)

[**7\. Evaluation Plan	6**](#7.-evaluation-plan)

[7.1 Identification task & score (Top-K)	6](#7.1-identification-task-&-score-\(top-k\))

[7.2 Latency score (speed per image)	6](#7.2-latency-score-\(speed-per-image\))

[7.3 Overall scoring	6](#7.3-overall-scoring)

[**8\. Model Comparison	6**](#8.-model-comparison)

[**9\. Version History	7**](#9.-version-history)

# 

# 

# 

# 

# **1\. Abstract** {#1.-abstract}

This document outlines a structured framework to build a simple, open-source model for the Balanced Blended Space (BBS) puppet show that learns our characters and objects from pictures with short descriptions. After training, the model should recognize who or what appears in a new photo that is similar to the images in our dataset and label it for the show.

Our current focus is to collect and review open-source models that can be trained on image-and-text data. We will compile a shortlist based on clear criteria: permissive license, active community, documented training steps, ability to fine-tune on our images and captions, and support for evaluating recognition on new scenes. With the chosen models, we will organize our dataset, run small training tests, and measure simple outcomes: naming accuracy and whether the correct answer appears in the first few guesses. The result will be a practical, reusable recipe and model list that BBS (and similar projects) can adopt to recognize characters and props for future performances.

# **2\. Objective** {#2.-objective}

This study is aimed to build a simple, open-source LLM for the Balanced Blended Space (BBS) puppet show. We will train the LLM with a curated dataset of pictures of characters and objects along with their short descriptions. After training, it will be able to recognize what object or character appears in a new photo which is similar or related to the characters or objects that the model is trained on for the Balanced Blended Space Puppet show.

# **3\. Open Source LLMs** {#3.-open-source-llms}

We are selecting a small set of open, well-supported vision-language models we can fine-tune on our BBS image-and-caption dataset. Each model will learn our characters and props from labeled photos and short descriptions, then recognize similar new photos taken during rehearsals and shows. Our plan: organize clean images, write consistent captions, fine-tune with lightweight methods, and check results on fresh scenes with different backgrounds and lighting. We’ll start with one model, compare speed to learn and reliability on look-alike puppets, and iterate by adding diverse views or clearer captions if needed. The goal is a practical, repeatable setup that we can share, reuse, and improve over time. Based on our goals we have picked five models to be the best fit for exploration for our BBS puppet show. These are-

1. Qwen2-VL-7B (Instruct- “Instruction-tuned.” The model was fine-tuned to follow plain, chat-style directions. You can ask it questions in simple language and it will try to follow your intent.)  
2. LLaVA-OneVision-1.5 (4B/8B- Model size options: \~4 Billion (smaller, faster, easier to run; a bit less accurate) vs \~8 Billion (bigger, slower, needs more memory; usually more accurate)  parameters.)  
3. Idefics-2-8B  
4. Phi-3.5-Vision (Instruct)  
5. InternVL 2 (InternVL2-8B-  A specific checkpoint name inside the InternVL2 family—the 8B-parameter version. The bracket just points you to the exact variant to download/use.)

## **3.1. Reason Behind Picking These LLMs** {#3.1.-reason-behind-picking-these-llms}

Below are the reasons why we picked these open source LLMs: 

**Qwen2-VL-7B**  
Great all-rounder for pictures \+ text. Understands prompts well and tends to give solid, on-topic answers.

**LLaVA-OneVision-1.5 (4B or 8B)**  
Lots of tutorials and examples online, so it’s easy to customize for our puppets/props. 4B is faster; 8B is more accurate.

**Idefics-2-8B**  
Especially good when images include text (signs, labels on props). Helpful if some props have printed names or numbers.

**Phi-3.5-Vision**  
Smaller but surprisingly capable. Good choice if you want quick experiments and short training cycles.

**InternVL2-8B**  
Modern, well-maintained model that handles varied image resolutions nicely. Solid baseline for recognition \+ short captions.

# **4\. Scope** {#4.-scope}

We will build a small first version that teaches an image-and-text AI (a vision-language model) to name known BBS puppets and props from a single photo. We’ll lightly fine-tune the model on our own labeled pictures with short captions so it learns our characters. The system will run offline on a normal laptop (using a GPU if available, or a CPU if not). For testing, we will give the model one photo at a time and check if it names the right character or prop. We’ll repeat this across a fixed test set with different backgrounds, angles, and lighting. We will compare several models using two main metrics: accuracy (how often the name is correct—Top-1 and Top-3) and latency (time per image to make a prediction).

# **5\. Requirements and Constraints** {#5.-requirements-and-constraints}

We’ll keep the LLM small so it can run on a normal laptop. If a graphics card is available, we’ll use its memory (VRAM); if not, it should still work on the regular processor (CPU), just slower. We’ll process only a few images at a time (small batch size) so we don’t run out of memory. During the BBS puppet show, the model should answer quickly, aim for under a second per image (low latency) and work offline with no internet. We’ll watch costs by running locally first and only using the cloud if needed, with a clear monthly limit. For privacy and ethics, we’ll use only images we have permission to use, avoid or blur people and sensitive signs, respect copyrights and trademarks (IP), and store all data securely.

# **6 Dataset Plan**  {#6-dataset-plan}

Build a small, clean image-text dataset so a vision-language model can learn the names of known BBS puppets and props and recognize them in new photos.

## **6.1 Class list** {#6.1-class-list}

* Make a fixed list of images for a particular character/prop (10–40 images in a list).  
* Give each list a short, unique name (e.g., `Blue_Dragon`, `Red_Fan`) to every list of characters.  
* Add a one-line description per list (color, material, any unique feature).  
* Formats of images: `.jpg/.jpeg/.png` (RGB), 480p–4K; max 10 MB each.  
* File name example: `Blue_Dragon_2025-06-01_side_medium_bright.jpg`.  
* Delete blurry/overexposed shots unless needed for “hard cases.”

## **6.2 Data augmentation (training time only)** {#6.2-data-augmentation-(training-time-only)}

* Light crop/resize, horizontal flip, slight brightness/contrast.  
* Avoid heavy color shifts that change identity cues.

## **6.3 Testing via single-image upload** {#6.3-testing-via-single-image-upload}

* **Input:** We will upload one image in order for the model to make a set number of guesses about which list the image/ character belongs to or resembles with.

# **7\. Evaluation Plan**  {#7.-evaluation-plan}

## **7.1 Identification task & score (Top-K)** {#7.1-identification-task-&-score-(top-k)}

* Give the model one uploaded image. It returns an ordered list of guesses for the correct class/list.  
* Default K \= 5 guesses (we can also run K \= 3 for a quicker test).  
* Score per image (Top-5 mode):  
   1st guess correct → **5/5**  
   2nd → **4/5**  
   3rd → **3/5**  
   4th → **2/5**  
   5th → **1/5**  
   Not in Top-5 → **0/5**  
  (If using Top-3: 1st=3/3, 2nd=2/3, 3rd=1/3, else 0/3.)

## **7.2 Latency score (speed per image)** {#7.2-latency-score-(speed-per-image)}

Measure time for prediction:

* **\< 1s → 3/3**  
* **\< 2s → 2/3**  
* **\< 3s → 1/3**  
* **≥ 3s → 0/3**

## **7.3 Overall scoring** {#7.3-overall-scoring}

* Per image total \= Identification (0–5) \+ Latency (0–3) \= 0–8.

* Model score \= average over all test images,  also report median latency.

# **8\. Model Comparison** {#8.-model-comparison}

We will make a chart consisting of the scores of all the models. We will order them based on accuracy and latency time. Then we will pick the top 2 models for our BBS puppet show.

# **9\. Version History** {#9.-version-history}

| Version | Created on | Created by |
| :---- | :---- | :---- |
| 1.0 | 10/05/2025 | Kazi Islam |
|  |  |  |
|  |  |  |

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAJgAAACWCAIAAACtqtYGAAAiw0lEQVR4Xu19aZBj13Xe7QZ6nZkeDoczQ2pIOrJjk4piS7bsJEpKjkqyYztVjstxlkqVZbuUlJWkkpSVUtaK5VRSSvLDJYvi1hsaDeA9PKwNNIDG0uhlZihyJC6iSIqrOJwRt+HMcLZesLzt5pxzHx7QALrZ08PunnmNU6e6Hx7eu+/hfvece865597LeIdufzJNkzWf69BtSB0gHUIdIB1CHSAdQh0gHUIdIB1CHSAdQh0gHUIdIB1CHSAdQh0gHUIdIB1CHSAdQh0gHUIdIB1CHSAdQh0gHUIdIB1COwxkhWtalZeaT98OpHNe1lSTqypfLXGz+evdpp0AUuP8bc675AUWzFrszwwMP4Z1ww28wuDfevoFl5QXXOG8O5DrDaTsM418eDIzGArjcaB4LDdjPwUewcJ4AVOmRKkWGby1EMFMKZhc+72F061fCTYbCmLhdK9chJd3Sdmu6CxLznNTE3jCZX84My9uOeoLNT2XhVJ2IdtH2w6kztXPjnhZJIP4KSn8G8p2y/RxNGgD+X9/+GMbZjjhljLdwRRA5Q5AdePFcKbXT+ANx0v6FSYXWDjD/Em+WoXbda6xUA5Pyvl3sOnUAajyCtwFPORBGLCoQFaccckZePbvF78v4GGxhHW+xtwwsCQdUJy1Xk+B90lACXgcKa7SIwDIr8wUxAVHJoK153LrlmjCfpnto50AUvyebiVaUqvwo6EVd8t5UXcok7wNkDbdMxZmQYT/0w95G/UZi86Ii98kpafxinW7N1O/qIkMq2YFfvZZASRwrxRvvNwinR/Gd8hCK7lr1INnTH6XL4RnoIVFc3RiDwAJNWX9nqCtYYy+AGID7fp968QNA4lSQpLK5HmuaywYE/deWVq/99oqkNaLKZlL1injcg0ktzTL9wiQZguQcKbPjxhA//GuOHXjQNpizZSsrpssaNUjN7T6RU20NSC1Gh7B7AVxxuRXBEhKtkcu0ok9CSSRihYscvs+0qb1gITjStWA7oopSewsoXeMJsIXrzRc0UIfBWS/NI3vRXxV3MDbAck16hHo5emVGoH8BJg2VIJu7gkg19IWgMSK1AcDVk+JLOfIQF6fPgrINawUyKhuC2QzNQLZhjtA8g2BROVVwwb4j5JFtF83oI8EMpwEV0Hw/slZ62k3CqRilcCUReuMU4GEMybJjobHRFsDEgwRXbfvAjdjYxw/EsjN95Hg7cDLo++hihN7so/8WIwdm+y7NKjUnQJyLxo7IH7W71GS4gS0ZeF+gP65DYA0uVsmkOR82TplrNJzu2V0LumSPQGkZv0epfZ7oEIVCgjImeu1M7cskADeUHCa3j/z1x/14ClD+9fz+P7uQL43gGpmTwAJv+fBEZ8Ag0WyLv+JO8bnRYgOK1Rgc2sAieFAeQ2j4WpwvWyy6CK5rQUWKmC4kRoiU9KiIe4JIDlFPv/LM68wuRauBJ4q9vgwtL3lEJ1N9l03D2QrV2sXgc/oDqYZBc0tHi+gr4NGz54BUlAZa6PMdcOA31iumGtrXdixOnHTefEVL6lNXwkS1i9+pRttL7DJpFqn2m9oEgYOrVG0tv4C9scGvPFj2TJSKZqhr3n/EljQ4l7N6klVvSrKadf8Pn7aOSA7tK207UCidrLb+a6wUZPr3WIS7e2mbQdyifMuaWYXuVtKfPOVc8ivnrW48aM4vqGPbctZv9h/99Z6EaGPk7YdyOUGe2R3OLQTtsYG9E7ziW2hnQPyyERoiQR0hxk8hD4pB9xT48aP4viGPrYtp7XY3oBlfjsNyHtHw83f7QzZw0k7zuL5jgWySoO/O8YrmAWS2jkO1cezxO91LJBQvb0BjOxgJKWxCWPqRqHPn93nnXf7iQOFbqiaCKZdiWtcEmVJieNAgYXj4rgPLohk3FJKfMSSQ9Yxi0Q5pgoBpmDDqvjfxCMx5kWH9nuhq1v7jB/gW7gDHE04QveQ7qryCo60cGNF3G8Tmcd3TFhJJ0yewS9NpwB5fX0goSIQPGrC3XK2R8mEOKYQmktLOkZ99Ot8Gb5d4lUWPFlDJV/mJguLmsqt8uviXncwioHscSy2L5CCSkdox6fAXrzMef9k9ItKjNPwGQvN3TchPeBPsZGJJXNlhetDnqjA4BceevyyTlJcQa/pcOjEU5zfMarc7ZW/kVt8An9IjPnTUHKPNMviibKmupVslS/D28J9Q2MJg1frQGKwCYF2PpBmFWrWcAUw7gVgdAWSx4fH4dgAqOQicLc0z0LZi1CCXxKFfG3h+3A/kxbhuCdaLNYK75YzBx8Zex9xyrLpUz2TKRbIXcLeMcrkRZOr5YrRF3kO7w3kuFEBpFNL5ml8l/JnR2IgX3d5ZAQQw07G/3j+1R/he5Ys6TT5K5y/SQfQGvqC4QqFA1e5AcbUr8fgdxklvbJEPuteBBLEZTCWKJvloUCEhZKu4MzdHqlbxrRgqy78KdC0IJTlWiFQoX3eyE/p4ycf9vT65u1a6/MVzpNAvw1/5SQUzuJTiGvN/fgX0/N5eKIUM03UsK74Ase8VfXv+iJDwYxQg4K65AWKJdSDcO9QAjTCanKQQlVcSnraHY5COzjy0JiOX6p7EUiodxZJslAO6sPz/mUWSA95AnAeKgg6QrcEfV5S3At15pIzP0SBAz2cw3Rkb3LVJP1ZqzXi4grXBiaLLFREOYYuUy7YQH7jiaf//LUzUAJg3BVO6dRrwrP+gd/HfLGGrpLjuMdaAjDgbUX/2hPMWxnmRCYvHfA/sSrCsHxPSiTWtfjN0vxp6qWGPD6QSL2eVGDxveOeuzzJByIyo7GLrM5BfEUhTawZVj60W5LexPJTNpAsUACwXJG5sqFBF+j2Shz+a/yBR0CY1C5lXiALdGcwtCwCezU6y/kZkkjQFm45hzZRjUAKoRewL96LQNppvshg9Uz6j4+NumQ0Ve59RP7kSOQbL75Q+7bISyv947WBsPRJruk4ZNgCZLmWct6l5O7IzV/CR4Qva5x5w6UrVY5pzXGwYKF+81w75CuA+uzzxuitDLdnfrWqIlyrar9PeR3PodXT4/H/71MvPEUCCcgwJb/agDH0rCx8gut7TyKxUUu2MBVAwlgow5SUNaGCGC6A84K7ibfwURy7axGWTZBItd2I0WlZg2IzHaXeYU8ACaah8AURucgJMPzg50IFLfNqL3SK5I18Vl47o+pm6GON7CzzSo88DaJaXuf9joz77YsFkNa0iG2mXQKSZOWbL76p85Vr8ONH40d8qbcpNPrKahnUrABSWIk3yzi5AN2bm2TxK0xD07jxrbfeZZEpThOSTLJv7XHyvQXkIBiW0lSlRG4fefcii8ftR2vTFUwIIDUUpvxNMjgk6BHeHGu1X6Gbmk5dLPSY0FMOeONorurU5gy0gfcWkGDRoLcezloTcdYy/HIBJL5cy7c3zEpq7RttiUwrFxCAtM5oxsvo4cwe9ScOyeHDocTBiURCq+4tINFRa8j2b2LwCT4GIOWZrz/7HLiVv/BXj4K8kAzdBNWAxCirWj40KrHQ/B8k81YWTwM5Fshr7YDslgqYZNZOHBnGYM2PAUif0hdeULUSlOOOzFYq/LE3rqMHsTVIa0ByrbJfmv6TF1/i10VMvZn2FpDgR94/5u0LJJpqH3rHwfDc2x+Hah0ILZhVo9+XsNMStXKlK174YMlWjjdCDap1CfvIhjLgASpOBxGu5N4C0h6KYjQDBESTzEJLQMGmvRkgQdyZXLhINeyWFuw47T3jQWF8YlZxe99hfaoBWeJarxzFAHqNmL+wz1t0hQv9UwtLvLRHgXQFCn3y9A84Di8cnKTMfLRg8zcDJAsWPlN4QkRhDk0UjJofCdU96M/hcWi+nVLckOpAlpicWKtUVcxxNfkK519SEnsRyIHQlInjtEBGtYJSonHtTh+uvLB1INMLF7As1HIvYz3iAh/iq9+JLdqXNQZLN0U1IA3D+A/PvtZWoqnXzxwZn7Sf4nwgB3wFFiliFYeSGJ+jC6CDZPL8inqNKcUtA2n3XVUaeMIKRyDhEanFUtW+7D+deKJxCZePJruPNHi/MtvYRdoEjeYXx0JHx/aURMoZ5XIVdFQbw1WeOXsTxo419ZTzw77aWkokkZc4H5yoTz75kk++IRxtIE2j+uenf/Qkp6FMW8HiocFiOMm5VbVeaChm+2h3gMQaqZ1vZVxjaatA2jrzzgmPJpwNcD+kFFd1So6iyyLZa7XLNks1IFdJVz8oBYciJ3q98q9JiftH/Sy36ArGr2jo3bQC6ZCgeVsgB/3JCoVAW5EA5iv6loE8Nhk6Nhk55o0e8sTYROyQN3rAF8C1PgwwRswyzXe8jPV7Q/LYIJG2VtVRdX9u3P+V3IlG5bm3gHRLmTNwXmlYk0OwkmGRxMWbUK0lEkKh80wKZ2uYFoRLm3XXmEUyPxFfb54agcRUI2gZJqjr3yuefui9ayYqdMzUq+41IF1yskdBFdoEQ7eEq7sN+me3DCTzFUDcG/lAILVKCzfYTPPeti6RoEuO++UuX+JuT+y0an7n1TPME+uPziJghnOBtNMAWt2PQ2Me+KHLnI+cv/6XP/2gUsaf/ej591g0tXUgybDUGiZjraAOp0HmYErwP/I1OYKboDqQxj/NncL8nUaRRlmsshHZyRL57vpAgoHAQsVHruLycRVTfwmujOZZDBOubgbI50r8FZ2/YvCXTeR3SvxTUhr4Adnir02ltgwkHB4Mp1fbCTSg+LNjsVb3w/lAbsBbBnLIn8QOy9SQ0RVRecnAWe/ItcUBpJNbB9Lk/+r7r7bXzCZ3BdKtAQFHAymnnrZUk4a2pAFGicqNFUz85SWwg35JjuLY0w0BGSowX4TJxfOoUVdMevSnpqJ3jE2Aebl4fenI41KvAlbP4ndf/gCeQ0utbjoqYAcEMLE205A6WacVvvynqRN7S7WyYGb04iprSC+2ud+bOSqn/4YvvFkgQ2LSCHh416HanqNVIwcn8r2B2H+dfw4gBBv40GTyvpFAvIpZdBxzINUDI0G6fd5+z8Z05DbUYOyA+bZvImRUVvAOykfBUqsGm8yU9TZBc4cD+e23LrBAm4X4wLl0SfnNANktFQAhKI6FF4YmCld1LBlrDg/QHeBC6E2EVuTd9Pkz+7wFOsaI0tBEFmRSV/lgbPoDoYnXowYgAbarnPcEZxnId7CA2e7+AgulEdV2qR4OB/Kx9693hYptUz2ANwMki6Xfo56wyjWcqrFM7h3Jx+ceUeouf0MWnUuZfhH914yIRfT5Cr8l4QgMNJ0heOIGQrk2IIAP0jHhitLVVcwTgL64yi/tNau1N5B9As5PWjlXrbwpIFGp1qeVa3p56DvfRltGwQWpEKrkbEVDdLpC0/c9Gly6Xlqpzd8w9as44ydWe3oouwGISK2RnVbSqy+ZbYB0SNC8LZCA34A/gyPArfAQtwLpjhb/17uX4eCob5zriE+ZN+zagH2VSvMbqeOCb1V+x6jcH3uyO5jQOK7R81POD0xmWHjeiu/QSlbWHE0l2S0V7egPDptoxj8uzBDsRJsBsopBO+cD+TPDa1RrnqMigm/PcX6W+C2q6/dpkLkVyIppGjSOcefkFM5X3QShSQq3hJNQm2Ly3iaZlKHRHUwyv/XOjQGBeutZS/B67+8FIO8ZWzP6EVjWh89c/rOXz/zp8y/989M/EPzNH/0kbfCnW4AE2SV788aAtInJLUHdDdkG0hWsjYXV3Q8VfCfEODCNuSlyEY57I1nmmwJX9fmyc4E82x7IzP977R2w+jBhp6UeWYtqvSkgaxhsnjcA0latwh7mOMtH06yUZSTHAvnWOkA++t415p9ZbyRrU0Bi5ZGlSjlsJh2vWQ3bRNuS4/ypDFlGa5+ioAO6xmxWcP1V6Dg3ASQq7Tt9CpNzLFj8uXHJ1rh7C0jAbwr300CXbk3l1vgjgYQKZVIGBcFEC+c1jjtXwaf7ApL9aMqimwXDCBEKJar2Dh7BbG8EF4gEG+erydllO1VamT7+V0EWKW4MZIWX98+e3OePVzUaSFExCXvAl71/ZJI72GptCyS4BwOeHItMbxnIQW8OPf4Wn+HvxDGubRE6HkWDl0n4IiyR1nmpNyEwyy7RiwEAFzj/1XEZlDyOuIXQe7GBBIvXKqoGJEj8vf64UVVxiqytT0lO36Gsjr0FJBj97sm4S0n3eHO9k7Nuaa4nMAcHLDwD6q7HF/9IIGl2eAuMnP/hs7jig0UApJQjDAp9EXR1XJ7CJV7qngbPp2iNr+EsonkM22FGiOVWNkhkM5AAzNAorQLSSlXOItk7PbWguTzjfCB7A9kTdExNGZfMrWAPh96hzo3XN2G1Mt9M29r89Gh9OWsTA9xxxCCUcgUL2FMCx2ZBlB8+8/7V2oshwMHEp77jsz9uACQUevhR/zrepNEVyB/eUxIpQnRMsmKe3dL8mYZqPbcJIPkSZ9EiN5dNEe/WrJDNwGgOp6DiQjna81SPgEGvPw9P7w7Xkq+C+VOX6rOLgAflyNfmn7Y/bgAkPIVNzRsiy6CJVGPkhXN7S7Va7oeUE+6HK1A4Z+91Fcye3QyQVEcgW7+fe+qxCzjuYRMg9GvJ/Ov2Z8LAHZl1hRsCSUr+j88v2x9dwZxbLFP+UUCiLFZKrsj8isjXM5FNCm6AAi/tNWOnwf3A6oMu84MblUhu1SAaHS1kollrB83b+JE4sm2vcdbCGwNJzzMOBCQc/fAXXHIKOtoHJyKr8MtUrRXIy1YR20u7BWQ2onKG+2YQkBLuG2H7ee+1AOmilVWagdwktQNyY94YSHEC/4m168iDpEEzpD0GZDQBxmXf6JT4Cpcqi+QpsR8/Zm9hIOE19nsThz2hT476v/HsK5EST6zyWIn/5Zvnv/Xqh+ccDOSbNSAbg+b1nB3Qb55ZVLDoeICTUBTnH/Qr2NLbAXlInkYcVfI+DKoq0LFGncX5OtQ1DPp8YLjmcJtC4GCilV2RNJOhbSU2AFJkr6M46tbTda79rl92DdPutA2Z5tAlOwpI2xy9d9TaRboRyHNUIYQF1U6F9ymL0Ovc4/NVxfymFiA3z73Rxf/57JsVvgJe4ym4vYQTIzdNa4GsRX/MBqsKG4tmsKnZ42Pj4vV5o0QqBXHGIUDaea37/LaHZ618tQrAVUrMTzsYEx9S8nrZALnZN5nB9q5ZEukmIKuGWcVhSA0YQV6PK9gucKWG4WC3nDnmk8R6gOer1TsCEdThkSSLpVh0ej3ul2bd4TgLp9yyBaTJS+JNtNrmIgDoSY6z51dXdPxQo4O1la96aotQXKx/uY207UCi4yVwCiXFGfDzDvjiLHKCl+ouRyNzs8rCmQpmhZcA8pbx5wILWI39VyZ8vzQc+sUR+dMjoV8di3x+MvFFf+K3g1PxZdyHjGNL4APynFFVe4cTLJrtDy388ZNPFwx+imD4Iec/pmmU0GG/gNZWBhuQWWkNNWA6QTDFlFmOVYZeJIvO/r1AsDULj8WttYCZtCjOZC80SvF20bYDqZGNikw79CEZ/OsvnB0I5r6SKbZN9bjXGx6Q8tjMdX7fw/WAi+AeXJm4RAVVoJX8/ESYhRLUIHDgAq/BLjDbNZVJ45Y5uKrjcYzpJHFnPEq0sXpWWuVIaPUzuLTnNBRC2+glW9DhLBaGEj4XmubWJAgcrEZt2nJlvdkp1u895plce8m20LYDWa0pJRav59fgFnbh9Kce9rUNmvf7F35+/LFf9goJxjEK+yuo60XqVEHgjk6lV1HNGiAibn+bzEoWmSOvfUV81C3419AyL4ONg4upybhU4JHItFtKHXtktOkyigNnqqQnwQ6/QgcfUKB86sPyvy88df/jof3BFLyJPW+XBb3i3p5wbQhlO2nbgeSa2hPM01Bf/pmaSQAOGFQ9jj+0U60snHsFvgon0Dw0jcFAuvFbtxy37pIy5McJUt/guC7Dmiv9RWwytalCjUOVKE9lfvi7Y0wR61DgvrwVdPbR3rWdQk5dw9+fxtBBVyBPuSPrez4UoLD3X8c19ogeeNgREgn0K6NjYudPdyhj9T8VzhJxMFnR/YiAYswe8EVFnjHwVc7/SQxHfTEJiuaquaW5RoSQ5fyXx2QsSld/czyA808BNAPXp6a1CsNoGIeTOIBvapjIGiCxMHABMgCHTaW7onM0CFP4/go0Ghy9Qsu2hVa1KvOmBnyFqsH/pHCa+eYOIzAqltPU9xn8fqW+4Iw1pVmsar/9tBNAargiMsVUgwXh31W5huIYW7QcMqyRCovWY2ZYxXIC5JiT0/YZWWkGEkTEv3AWCgnEoDX8n2fesB6mW8tUk3TRBnc1F9A9lh0MY4NAEQwlEGklcY0evmSsftLv4WqjKBIZanc41RvI9tl7hKpqCfRnPOeWTz6Fn0mIhagbvMvqpPNMLtAeBOohr89eXHlbaSeAhKpy0dIrbinzXIN1wLxFWwrXcu6ri09BvXwpnev2pgQYfVK6bYdKwGSgHUDDf540W0zMDMA61ioch0aw34pkeiat66cvLsOXh4bHcCkKOWU5gE1EwvTXvEGQ766xNNfq/SsUPnsVrWL/NbXPn90vTZ9DB0T7b6d/jKOb4GhNZoTNDA32yPBO6FW+Q0ByA3PpZEvtXDdw7IlOr519HgOJWfxSJHcFa7EyGJv+ghTo9ib/ZfZ7GD7hGvOnKaemGUhkufCZ9AlhA//B4knsDnXDLSXfo6fAyS9PhuAkS6Bp+jrHlgEPeWmlSqniLaYnRxTfQocExGsGJcpeLqsKn/TPS6Q5URbRr3TLuWqFjHOry8+RUYw9OGr7HaGdAJI2PTG6Jy0MfqhZtYKmgT0EoWRe5PjjoWkPoJmKnRbzhi6BQR8r/Nkbb0AFGrrqmlh3yKLO8dm/ePV1kvXUWSoTTkpXVziKJ/f95Dwvlyyvfn36nWxaWMsARlmlvSKI8J1jJ9bcjrnR2r/J13txDCDBdbrZH8u3HZzZDtoJIAVZW74Hs73+giUEAJpagRrntD6fWsYFhwDalzgfVJIuEq8euYh15899AXopbOfaZx73N2/hsx4rKbRfcG/uDGZnbZoG/UmSrRSKLepY44DiZaH0UCDSN+4HWRe2lU06bVMhHro/NsfJOH+dprm3F/dtoJ0D0sRfa022Ouq1lrmBKgArpdc3j1t5RGv9pVJcVpeZJ8LIccTNXCIp0HK9cFeV7tHQ01ivy6xzOPW7uSepj8Q1YDdD5asc4+agUUegq9WugqEUo2khZDyDqPXixKvUDFoyqmVU8VKXv/5Q27K5w5exi90B2jkggcqlJauTCy3gRmdE8AbHlVMsMBs9f9Wujt7wLJtuYwe5fHMahd1NrmKERW4XB7A5lLrn8SBJJC6YtBGpmDfEorRSXTD7N0fAQjHYRJirdUfVIkCvVJ66rNPOMqfgLjiwtu6KZD4vp0Tv2CfN4OS+HaQdBdLklb947nVUjEqmW0rbzrWOWjd+xBMe8voxIbEVkgbulgr9wRQqL5IIMExpEKrdXeHksbEIXOaWUu09AB39TAq5LVhrjk6GuF6CXg+XLN+we0OH2OBflhv77LhQM9acr52lHQVSUFcNKreIfVudiLE/94PgNQM6tmY8kDPMnzkop1dJv0GHt1/ODXpknI6FVifW+CUKhtHOE3QL/T3kVziGzVKU+UGMfR7e9jLn/RNxyvnIDE7mu+UprP0l3LwcULrPMyEaShPZ5+CykfdWra4R9P9kwaQhLhDt+8YDG7aBbaFdABJ9f3m6z58H2QIPveE3aywyAx1kC4pQ1zMiUIKrBATFNNX84WEPeDVdE/O/Hp4H3YhT7agnE2gB3vKFi7/16AjeFcx+9cW37h4NYc4x3BvOsFgSmlGXlO6NPHkdNauBtlggW6igQoRC2MRU24hMmbAE+f722x+KtuKWE4O+FKcN7uAVhiaCNGNrh2wcm3YDSPS9li3AwMeI4tiQIKjEQw/XV8Wwmeto1zIfbhlQPw/drZThS1f7PKirmZx2eQr//cUf26Xp6Pag4QnY47Q6e+6Hkgmfe8dEJ9AA5crGAyCd+hKuhMRpt7rrXO2VkgPj9dkHNt3lx7ERtzeAW8+IlwllzepqhV6+K3lyx/yNJtoNIDluwYCtnqKsUBcH/KcaR95xaLABRVe4gGZqJNu2+/yZYR8KE/h83hx0b5c5Pzous+gi1O+AL0t7Y9HGVaRWNTIyL+tQWgzgPDiR4Ka+byL4s95pu0fUcHIVDpq6Jqd7E0+JgWqkVe2u1Dyv6Cycw/aHDSLFomnoaDH+UNYO+maNxt3PdpZ2CUgiUFM9ETQ70WXE0cq6OtIQG6v7gW/5+lsSfHosSHMWM1/EDSDR08DGIRd7H3rs2OPDYElCBwn92d8alg95o+iElMCjT9uRGhySVMtMmquaFWgQdwZONMRbtQ9x0YcpEP393nSZpnZg4o/9dA8ObBEZbDiNmxnuuEa1aTeBRKpyXPFP1MtUhiyX+pcXEM5FDJ3TBK7mUWgc+SuYZHeAAwDXz+H1zUgDkPUSiXriOGugkTCrNpQso73ZnnADvYm0m14Vm52SPfjIWM3yUQ94Qk0hgp2n3QaS3uDQeEBkSAAen3h4XFihgkCtPRhOgBLG0ZKx+i7KYG0OBBYpeqKhSzcxDdV6FvGIglD2Son66qy1bQdtOjDuv2d4Yk3Fo2U7Z8NI6nTlJ6gY0Mf4jWkox5rzjAGKZE6k4YA0L5sqjoHsMohIuw+koFWoShnNH/QHAimV7M96h6Px4w+NHZ8E9cXvHg+zyemvL7yGmgx6LHAilaTv4jUTZ0xidHS/f/paQ8ysFUhU1MpCPQ5OBC2mf2oB1SP0oKOx1+nkz43gjpU1LuKsLl8AX4u4x5PaocHGTdCtAiRiVsWACNmfeTBBe2VM2yEsxXZnGnRm30M7KN0NVoyS7p/6Hu28m0K9WtX/87vviQgLbpQuL9YDfq1Acs2FwxfNQQIo7bdn5sTc1YPhNJOzvQ07IUJ3y0W0xuDX1Go/ja+1XV1wV+hWAVKQkBEhmgyHjjNsNLDmCoMu0o1/trBoZ+BB94lpbf6UnWaxhsP1iXYWVXhfIPG3Y82pNDrujy6Dlm4uAYf7sZvkFBp0ZZ7iOopug8bYfbq1gBQEShL6JzeNImFvF0kNThaXaVPCRsJ1yjS0fX9Zibq81Ie1Wjp4stnYMWlcQsegkFHhxm/609gmQmt2IrX48ZH6zp8r2sHw9BVyNG8pCAXdikByEas0jO++8AYoNJGW3gfSqeT/YXJuRVteY9rapCNEAqSH3nz72JiCllF0pi+QoilU2pKBQ0tfmMkO+jI0xby4b3Id7AOZ0bevwAtYy9vr2j5f8hka3G5+6C1DtyiQddKqtH95Q42DEauc3O/xC8HAEfuNq3d9H7TOmKKRYVL8VZRULI7GyrBNsGDuKjaR5g71VqNbHkibNPWPTpxkgTSLR9eMdcQLzB95Dhx9VcMkcWEf0V6/Jq3kyBvjRNEU7sAcKHbjgEnmgDz/CcCJrheNoYr5xwZXNZDjH+D5HR2Kuhm6fYCskc7Vf/vMi8wPcpnBETEZdxBolrBGDhcymnGKUs6bieROqPGqyDePLOaaL7o96PYDkqOZI+ZcqFxVpz7UcDUOa787is/V/4qTOFgm9o0g94/6V9qXHoO64dn9cvxtCiygUFoOz+1HtyWQjUSKFFPZKiSsON5bxZDbf3zh1XtHg3f7E/uH5YPjwbvHAveNyycNNH3Rf4FGQGF00Rc6gG57IDskqAOkQ6gDpEOoA6RDqAOkQ6gDpEOoA6RDqAOkQ6gDpEOoA6RDqAOkQ6gDpEOoA6RDqAOkQ6gDpEOoA6RDqAOkQ6gDpEOoA6RDqAOkQ6gDpEOoA6RDqAOkQ6gDpEOoA6RDqAOkQ6gDpEOoA6RDqAOkQwiA/P+wsJGGqkLUNwAAAABJRU5ErkJggg==>