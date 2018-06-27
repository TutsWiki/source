---
date: 2018-06-27
linktitle: Deep Learning Helped reducing variability in Cardiovascular Imaging
title: How Deep learning Helped Reducing Variability In Cardiovascular Imaging
weight: 10
url: /deep-learning-cardiovascular-imaging
description: New software based on deep learning algorithms automatically and accurately calculates LVEF
keywords:
  - deeplearning
  - deep-learning
  - data-science
  - cardiovascular
  - health
  - artificial-intelligence
  - machinelearning
  - radiology
---
<meta property="og:image" content="https://tutswiki.com/images/deep-learning-cardiovascular.png"/>
<meta name="twitter:card" content="summary" />
<meta name="twitter:title" content="Deep Learning helped reducing variability in Cardiovascular Imaging" />
<meta name=”twitter:description” content="New software based on deep learning algorithms automatically and accurately calculates LVEF" />

[Bay Labs](https://baylabs.io/), a San Francisco-based medical technology company which focuses on using artificial intelligence to improve cardiovascular imaging, has released a new software [EchoMD AutoEF](https://www.businesswire.com/news/home/20180619005552/en/Bay-Labs%E2%80%99-EchoMD-AutoEF-Software-Receives-FDA). It is being claimed that this software will help to reduce the variability in cardiovascular imaging.

The software uses deep learning techniques to accurately calculate the `left ventricular ejection fraction`. 

## What is Ejection Fraction?
During each pumping cycle of our heart, it contracts and relaxes. When it contracts it passes the blood to ventricles. Ventricles are basically the pumping chambers. Ejection Fraction is the amount of blood withdrawn from the heart when it contracts. Medical professionals use EF to identify the health of a heart. They generally measure the EF from left ventricle because it is the main pumping chamber of the heart which passes the oxygenated blood through the ascending (upward) aorta to the rest of the body. 

![Cardiovascular Imaging with Deep Learning](/images/deep-learning-cardiovascular.png "Cardiovascular Imaging with Deep Learning")

## Measuring EF
There are various methods available to measure LVEF, out of which [Simpson’s biplane method](http://web.stanford.edu/group/ccm_echocardio/cgi-bin/mediawiki/index.php/Left_ventricle_systolic_function) is used widely. This method has about `9.2%` average variability. The EchoMD AutoEF software by Bay Labs has an average variability of `8.2%`. Moreover, EchoMD AutoEF doesn't require any user intervention, it's fully automatic. It calculates LVEF from complete echocardiographic patient studies, automatically.

## How Deep Learning Helped?
At present Human intervention is required to calculate LVEF. Medical professionals have to go through the recorded clips. They watch them carefully and pick the best ones then they manipulate them for quantification. This is a very time consuming and prone-to-error process (as it is done by humans). The deep learning algorithms helped in this case by totally eliminating the need to do all of the above manually. The training set included 4,000,000 images from about 9,000 patients.

Quoting Richard Bae, who is the Director of the [Echocardiography Laboratory](https://www.nhlbi.nih.gov/science/echocardiography-laboratory) at the Minneapolis Heart Institute:

> "Historically there have been challenges with variability and reproducibility in reporting of the ejection fraction, especially when the EF is not normal; our study showed that the EchoMD AutoEF algorithms can aid interpretation enormously and have less variability than cardiologists reported in the literature.”

> "By supporting fast, efficient and accurate AI-assisted echocardiogram analysis, the algorithms can allow physicians to focus on putting results into the context for the patient - guiding prognosis and course of management.”

This is a great innovation in the medical field. It will help cardiologists in better decision making.

{{% notice note %}}
U.S. Food and Drug Administration (FDA) has given [510(k) clearance](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfPMN/pmn.cfm) to this product. Which means that the device is safe and effective in automatically selecting clips and calculating LVEF.
{{% /notice %}}