# Abstract
This research paves the way toward a new approach for performance and reliability improvement in face recognition systems for crowded environments with the help of artificial intelligence and human agents through collaborative decisions. This combines input from eight human agents and eight ResNet models. These AI agents are preprogrammed with individual behavior patterns in terms of confidence and biases to offset prevalent biases in other systems. We evaluate both human participants and artificial intelligence agents on target identification for 288 crowded images. We collected human target decisions and confidence levels from a target identification decision-making experiment. Then, the ResNet models were tuned to different confidence levels and biases to simulate real-world group target decision-making scenarios. The encodings of the target face images at each trial are preprocessed with ResNet to extract relevant encodings that will be compared to encodings of faces detected in the crowded image. The ResNet confidence is determined with the distance between the target face encodings and encodings of faces in the crowded image. A test evaluation was done in the groups of 1 to 8 agents with every possible combination of human and AI agents. All these groups significantly outperformed their respective individual agents, with accuracy gains being robust across all group sizes. More importantly enhancements were observed only for those groups that used the best AI components in combination with human members, making the collaborative configuration effective for the task. So, in general, this study proved the betterment of human-AI interaction synergies in developing better face recognition systems with more powerful and less biased identification solutions in solving real-life tasks.

# Dataset
The dataset to be used in this research has been sourced from the sequences P2L_S5 and P2E_S5 of the chokepoint video dataset. Each sequence contains more than 700 frames of video streams capturing 29 people in each walking indoors through two different portals. Two persons were randomly 
selected from each as the “target” persons. A subset of 48 frames is randomly chosen from each sequence for the experiment. This subset consists of 12 frames where the "target" person is visible, and 36 frames do not have the target faces. To represent each frame effectively, three imagesare captured from cameras positioned at the top-left, top-center, and top-right of each portal. This process results in a total of 288 images (2 sequences × 48 frames × 3 images). Sample images captured at the same time frame from three different angles have been provided in figure 1 and identifiable facial features have been masked for publication purposes. More details on this publicly available image dataset can be found in https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0212935 

![image](https://github.com/user-attachments/assets/97e54b21-a7ff-4981-a753-50f8de620092)

# ResNet Architecture and Training 
A residual neural network was used to create the AI agents for the experiments in this work. The deep residual learning framework was first introduced by Kaiming He et al to address the problem of gradient degradation in deep neural networks. The ResNet model used in this approach consists of 29 convolutional layers similar to ResNet-34, but with a few layers removed and the number of filters per layer cut in half. The network was pretrained from scratch using a dataset of roughly 3 million faces which is a combination of the VGG dataset, the face scrub dataset, and a sizable collection of images scrapped from the internet. The model achieved an accuracy of 99.38% on the “Labelled Faces in the Wild” benchmark dataset. The ResNet model and a pretrained face-detection model are integrated into the face_recognition python library.

![image](https://github.com/user-attachments/assets/8e5e3289-2582-4ac0-8c62-dda69d612a1d)

During the experiment, the target face is first loaded, and the ResNet computes and stores the encodings of the target face. Then the ResNet scans the crowded image to identify and draw out each face using a previously trained face detection 
model integrated in the face_recognition library. A face_encodings function in the face_recognition library maps each face in the crowded image to a 128-dimensional vector which indicates where photos of the same person are most likely to be located. For each face detected in the crowded 
image, the ResNet computes the Euclidean distance between the face encoding of the target face and the encoding of the detected face, then the ResNet finds the face with the minimum encoding difference and based on the calculated difference and a predefined threshold t, it determines whether 
the face is the target or not. The ResNet confidence scores are calculated based on the encoding difference and threshold.

# ResNet Decision Confidence Modulator

![image](https://github.com/user-attachments/assets/97c2b6b1-a1e2-4978-aded-83c59f745b55)

Where:
t = threshold to set the biased behavior of agents
V.p = scaling factor to tune the confidence behavior 
d = difference between target face and detected face encoding

![image](https://github.com/user-attachments/assets/6d0211e0-e644-4fc6-8d5c-c60ae92bd8f6)

![image](https://github.com/user-attachments/assets/d436736f-acb0-4f41-a293-ae176e5fabdc)


# Group Decision Methods
Three categories of groups were formed, Human only groups, ResNet only groups and a combination of both humans and the best ResNet models. To compute group decision for the different group categories, firstly, each ResNet model’s responses were converted to correctness by 
comparing the responses with the ground truth. Correct decision was labelled -1 and incorrect decisions were labelled +1 (similar to the decision correctness obtained from the human data). Then each decision correctness was multiplied with the corresponding model confidence to get 
the weighted correctness.

I. Human Only and AI Only Groups
All possible combinations of the 8 human participants were created for group sizes of 2 to 8. 
Let 𝐴1, 𝐴2, …., 𝐴𝑛 represent the agents in the group, where n is the number of agents.For each trial t, the group decision 𝐷𝑡for a group of size k 
(where 2≤ 𝑘 ≤ 8) was calculated thus:

![image](https://github.com/user-attachments/assets/02df8752-03e2-46e7-be85-49351fcdc723)

II. Human & AI Groups
The best 2 AI agents were combined with the 8 human agents and all possible combinations of the ResNet and Human agents were created for 2 to 8 groups.

The decision correctness, and confidences of 8 human subjects in an experiment to identify if a target face is present in a crowded image or not, were collected. AI models with different behavioral patterns to carry out the same task were developed using ResNet models. The models were 
evaluated individually using sensitivity, specificity, and accuracy. In this context, sensitivity is the fraction of “Targets” that are actually labelled as “Target” while specificity is the fraction of “Nontarget” that are actually labelled as “Nontarget.”

# Results 
A. ResNet Models

![image](https://github.com/user-attachments/assets/de812d5f-ed26-470f-80c7-e0f6a3e580ce)

B. Human Agents

![image](https://github.com/user-attachments/assets/cceba112-d5aa-4676-a13b-6abbe5f9f3b2)

![image](https://github.com/user-attachments/assets/5075903c-ce94-4219-8483-4b5da7f310c3)

The best two AI agents (High confidence unbiased and Low 
confidence unbiased) were merged with the 8 human agents 
and all possible combinations of the ResNet and Human 
agents were created for group sizes 2 to 8. The mean 
accuracy of each group size was achieved. Figure 8 shows 
that humans and AI in collaborative setting demonstrate 
better group performance than either all-human or all-AI 
groups. Performance tends to increase as group sizes 
increase. Combined agents of group size 8 had the highest average accuracy of 88% surpassing both human only and 
ResNet only groups. Also, we combined 4 ResNet models with 
all 8 human agents and created groups of sizes 1 to 8 and 
computed their average accuracies. No significant 
improvement was observed across the various group sizes. 
However, groups of size 8 had slightly higher accuracy of 89%.
The results provide some insight into how partnering humans 
with AI has the potential to make face recognition more 
accurate in crowded conditions. The human agents bring in 
their contextual understanding, intuition, and adaptability into 
the task, while the AI agents support the human with 
computational power, and pattern recognition. Combining both 
human and AI agents can offer better performance and 
reliability by leveraging the strengths of each. This partnership 
is most beneficial in contexts where the complexity of the 
environment or the volume of data exceeds the capacity of 
either humans or AI systems alone.









