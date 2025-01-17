# Disease detection Project using Chest X-ray Database
This project aims at a new chest X-ray database, namely “ChestX-ray8”, which comprises 108,948 frontalview X-ray images of 32,717 unique patients with the textmined eight disease image labels (where each image can have multi-labels), from the associated radiological reports using natural language processing.
## Abstract
Chest X-Rays are the most reliable radiobiological imprints of patients, widely used to efficiently diagnose an array of common thoracic diseases. For too long, vast accumulations of image data and their associated diagnoses have been stored in the Picture Archiving and Communication Systems (PACS) of several hospitals and medical institutions. In the meanwhile, data-hungry Deep Learning systems lie in wait of voluminous databases just like these, at the cusp of fulfilling the promise of fully-automated and accurate disease diagnosis. Through this project, we hope to unite one such vast database, the “ChestX-ray8" dataset, with powerful Deep Learning Systems, in order to automate the diagnosis of eight common kinds of lung diseases.
## Introduction 
### Why Deep Learning for Disease Diagnostics?
  * Convolutional Neural Networks, which form the soul of Deep Learning systems, are designed with the assumption that they will be processing images, according to computer science experts at Stanford University- allowing the networks to operate more efficiently and handle larger images.
  * As a result, some CNNs are approaching – or even surpassing – the accuracy of human diagnosticians when identifying important features in diagnostic imaging studies.
  * In June of 2018, a study in the Annals of Oncology showed that a convolutional neural network trained to analyze dermatology images identified melanoma with ten percent more specificity than human clinicians.
  * Researchers at the Mount Sinai Icahn School of Medicine have developed a deep neural network capable of diagnosing crucial neurological conditions, such as stroke and brain hemorrhage, 150 times faster than human radiologists. The tool took just 1.2 seconds to process the image, analyze its contents, and alert providers of a problematic clinical finding.

Speed and Accuracy are characteristic features of ‘Deep Learning- driven’ Medical Diagnostics. This indicates the potential for getting optimum results in exploring this field.
### Why does Medical Diagnosis need Deep Learning?
  * **More affordable treatment:** Diagnosis is faster and more accurate with automation; doctors will be able to recommend the right medicine to patients before illnesses aggravate to require more expensive treatment options.
  * **Safer solutions:** More accurate diagnosis means there’s a lower risk of complications associated with patients receiving ineffective or incorrect treatment.
  * **More patients treated:** By reducing the time it takes to complete a diagnosis, laboratories can perform more tests, covering a much larger number of patients in much lesser time. 
  * **Addressing the global ‘Physician Shortage’:** This is a growing concern in many countries around the world, due to a growing demand for physicians that outmatches the supply. The World Health Organization (WHO) estimates that there is a global shortage of 4.3 million physicians, nurses, and other health professionals. The shortage is often starkest in developing nations due to the limited numbers and capacity of medical schools in these countries. Additionally, rural and remote areas also commonly struggle with a physician shortage the world over.

The growing need for more qualified medical personnel worldwide is like an incomplete jigsaw puzzle.  Deep Learning systems have the potential to finish this puzzle once and for all.

This is exactly why we feel that the quest to build such a system is truly relevant to the needs of society at present. There is no denying the fact that with an ever-growing global population, we are going to need alternative AI-based medical personnel to assist us in achieving the sustainable development goal of providing “**A**ffordable,**A**ccurate and **A**dequate Healthcare for All”. 

## Methodology
Recent work has shown that convolutional networks can be substantially deeper, more accurate, and efficient to train if they contain shorter connections between layers close to the input and those close to the output. In this paper, we embrace this observation and introduce the Dense Convolutional Network (DenseNet), which connects each layer to every other layer in a feed-forward fashion. Whereas traditional convolutional networks with L layers have L connections - one between each layer and its subsequent layer - our network has L(L+1)/2 direct connections. For each layer, the feature-maps of all preceding layers are used as inputs, and its own feature-maps are used as inputs into all subsequent layers. 
Entire project is divided into following stages within the team for implementation.

### Sampling
The dataset was highly imbalanced, a high of [62,000+] and low of 110, and huge for our timeline and we had to resort to using a well represented sample. We eventually scaled down on the dataset to [12000] from [112000].

We initially tested our strategies for classifying all 15 conditions and redefined our goals through exploration, until in addition to identifying Healthy X-rays, the model could more accurately classify two conditions, Cardiomegaly and Effusion.
For our final round, and based on clinical considerations, we proceeded with two sets:
Version 4.1 - for images taken using both antero-posterior position (AP) and postero-anterior position (PA).
Version 4.2 - containing images taken using PA for Effusion and healthy (No finding) classes, and AP and PA for cardiomegaly. Exception was made for the latter due to inadequate PA images.
The distribution for Version 4.1 finally settled at:
 (AP + PA) Cardiomegaly – 1093
 (AP + PA) No Finding – 1500
 (AP + PA) Effusion - 1500
The distribution for Version 4.2 finally settled at:
 (AP + PA) for cardiomegaly – 1093
(PA) No Finding – 1500
(PA) Effusion - 1500
 
### Preprocessing
Given that our dataset covered medical conditions which manifestations could be present at edges of X-ray images, we exercised caution with our transformations. After several deliberations and clinical considerations, we resorted to avoiding cropping and extreme random rotations and kept to these:
Random Rotation: within the angle range -10 to 10, with expansion enabled
Resize: to a size of 224 by 224 to match our densenet model input size
Random Horizontal Flip
Random Vertical Flip
 Conversion To Pytorch floatTensor type
This minimalistic approach was our shot at preserving as much information that would be clinically relevant for diagnosing our target conditions.
 
### Modelling
We had a run with densenet161, and resNext50 during our model staging to assess and compare performances, before finally settling with densenet161.
The modelling stage was characterized by several iterative cycles that called for new sampling and processing strategies on demand. Notwithstanding, the team model facilitated swift responses so that we could re-orient quickly without disrupting overall progress.
We also had the technical expertise that allowed us to try novel activation functions - namely mila, mish and beta mish – which we believe contributed greatly to our results, in addition to hyperparameter tunings.

## Results
## Discussion
* Reason to use Densenet over Resnet  
  * they alleviate the vanishing-gradient problem 
  * strengthen feature propagation 
  * encourage feature reuse 
  * substantially reduce the number of parameters
DenseNets obtain significant improvements over the state-of-the-art on most of them, whilst requiring less memory and computation to achieve high performance.
## Limitations
Few Limitations that we found while working on this project are as follows:
- The fluorescence channel
  * rotation degree (we used ‘hernia’ to demonstrate a small rotation or central cropping is going to dismiss the key info) for augmentation, which may contribute to the difficulty of generalization (?). Fixed value rotation VS random rotation.
  * We discussed extensively about the positions (PA/posterior anterior or AP/anterior posterior) of how the X rays are taken and how they would affect the images, especially for ‘Effusion’ group. Ideally we just want PA pictures, but we also needed to consider the data size in each group if we only include PA pictures.
  * limited to 8 classes based on our understanding of the images, the data size, and the reported performance of past model. 
  * We discussed the concerns for AP pictures, considering the classes we are including (Cardiomegaly, effusion, no finding). Both of cardiomegaly and effusion are sensitive to positions. But considering the picture quantities we have for cardiomegaly, we decided to still include both of AP and PA pictures. This may lower our accuracy of our model.
> We ran into significant difficulties in our attempt to find a working approach towards processing the Chest X-Ray dataset and training a successful deep learning model. While many factors can be taken into account for this complication, the root cause can be traced back to the original dataset itself, which contains many imperfections and deficiencies that posed numerous challenges for the Data Acquisition, Pre-processing and Modelling teams alike. All of those shall be explored and clarified in-depth for our readers in this section.
  * First of all, the Chest X-Ray dataset that we obtained and worked on is extremely imbalanced when it comes to the distribution of number of instances for each class. For example, there is a huge disparity between the number of the top class – No Finding with more than 60,000 instances and the bottom one – Hernia with mere 110 instances. This created a difficult situation for us where we had to make a decision on how we should further process and transform our data before feeding to the model. Furthermore, the X-ray images in our dataset were taken from two different positions: PA/posterior anterior and AP/anterior posterior. This especially held true for “Effusion” class, with the number of images captured in each position almost equal. Since many of the diseases in our dataset were sensitive to such positions, we debated on whether we should split each class into two smaller ones: AP and PA or more ideally, only retain PA pictures. The latter approach, nevertheless, had one major disadvantage: we need to take into account the quantity of pictures in PA position, which in many cases there were simply not enough of them. In the end, considering the limited number of PA images we had for “cardiomegaly”, we decided to include both PA and AP pictures in each class, and this would definitely have a negative impact on the accuracy of our model. Another problem that we faced during the pre-processing stage was about how we should proceed with data augmentation. Given the nature of X-ray images for lung diseases, even a small rotation or central cropping could end up leaving out important diagnosis information and lead to the difficulty of generalization. Having researched extensively to gain a deep understanding of the images, the size of our dataset, and the reported performance of past models, after lengthy discussions and countless trial and error attempts, we finally came to an agreement: reduce the number of classes in the wrangled and modified dataset to only 3: CardioMegaly, Effusion and No finding, with the number of sampled instances for each class became much more balanced and therefore prevented biases from occurring when being trained by the model.
  * In addition, after consulting with the subject matter expert Olivia in our team, it turned out that the X-ray images in the dataset were missing some crucial information typically used for diagnosing diseases. In particular, for conditions that tend to look similar in X-ray images such as “Mass”, “Infiltration” and “Pneumonia”, in real life doctors would rely on other factors such as white count and temperature to make a formal diagnosis. Unfortunately, those information was not available in our dataset, which in turn can be problematic for whatever deep learning model being trained and deployed to classify conditions. In fact, in the original paper, all the fore-mentioned diseases performed poorly and recorded a high prediction error rate from the model, which helped to reinforce our earlier finding.
  * Last but not least, it is clearly stated in the paper that all the disease labels for the X-ray chest dataset were not done manually, but rather through a number of different natural language processing techniques when constructing the image database. Although the authors have gone the extra mile to mitigate the risk of wrong labelling by crafting a variety of custom rules and then applying them in the pre-processing step, the problem of defective labels was not entirely eliminated. Specifically, we can refer to “Table 2. Evaluation of image labeling results on OpenI dataset.” in page 4 of the paper for a detailed assessment on this phenomenon. The vast majority of classes exhibit varying degrees of being mislabeled, from Effusion with 0.93 precision rate to Infiltration with a modest score of 0.74. As a consequence, this has considerably limited our model’s ability to train on the dataset and later classify disease labels with great accuracy, given that the data was not without flaw right from the beginning.

## Conclusion
## Recommendations
The team would like to extend the project to institutions where aid to diagnosis is of utmost importance. Currently, as per discussed in the previous sections, the project is limited to the public domain dataset and to the best-effort analyses of health records via natural language processing. The idea here is to improve the current accuracy of the model by augmenting it with real-world datasets which are available from medical institutions. 

Due to the sensitive nature of these datasets and with intentions of privacy, naturally these are currently being kept private. 
## Appendix
## Collaborators
Members | Slack Handle
------------ | -------------
Victor Mawusi Ayi | @ayivima
Anju Mercian | @Anju Mercian
George Christopoulos | @George Christopoulos
Ashish Bairwa  | @Stark
Pooja Vinod | @Pooja Vinod
Ingus Terbets | @Ingus Terbets
Alexander Villasoto | @Alexander Villasoto
Olivia Milgrom | @Olivia
Tuan Hung Truong | @Hung
Marwa Qabeel | @Marwa
Shudipto Trafder | @Shudipto Trafder
Aarthi Alagammai | @Aarthi Alagammai
Agata | @Agata [OR, USA]
Kapil Chandorikar | @Kapil Chandorikar
Archit | @Archit
Cibaca Khandelwal | @cibaca
Oudarjya Sen Sarma | @Oudarjya Sen Sarma
Rosa Paccotacya | @Rosa Paccotacya
