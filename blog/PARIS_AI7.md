# Paris AI #7 Meet-up Summary

Alexandre Zouaoui

2018.04.09

***

### Abstract

<p style='text-align: justify;'>This report consists in the hand notes I have taken during the 7th edition of Paris' research and product focused machine learning meetup that was held on the 4th April 2018.

This meet-up specifically target **ethics** in AI with topics such as *privacy-by-design*, *security* and *value-alignment*.</p>

Special mention to [Algolia](https://www.algolia.com/) for hosting the venue in their Paris office - 55 rue d'Amsterdam 75008 Paris.

Huge thanks to the organizers for setting up such a lovely meet-up ([@LaureAndrieux](https://www.twitter.com/LaureAndrieux), 
[@raffikamber](https://www.twitter.com/raffikamber}) and [@itsflamant](https://www.twitter.com/itsflamant)).

This meet would not have left the flavor it did if it was not the speakers attending:
* [Alexis Lê-Quôc](https://www.twitter.com/alq) - CTO & Founder at [Datadog](https://www.datadoghq.com/).
* [Alice Coucke](https://www.twitter.com/alicecoucke) - Senior Machine Learning Scientist at [Snips](https://snips.ai/)
* [Andrew Trask](https://www.twitter.com/iamtrask) - Creator & Leader of [OpenMined](https://www.twitter.com/OpenMinedOrg)
* [Julien Cornebise](https://www.twitter.com/JCornebise) - Director of Research & Head of London Office at [Element AI](https://www.elementai.com/).
 
 
Last but not least, thanks to the amazing audience!
Witty questions were asked and interesting conversations were held.

***

### Table of contents
1. [Alexis Lê-Quôc on Anomaly Detection](#Alexis Lê-Quôc on Anomaly Detection)
2. [Alice Coucke on offline and privacy-by-design voice assistant at Snips](#Alice Coucke on offline and privacy-by-design voice assistant at Snips)
3. [Andrew Trask on OpenMined](#Andrew Trask on OpenMined)
4. [Julien Cornebise on AI for Good](#Julien Cornebise on AI for Good)
* [Disclaimer](#Disclaimer)

***

## Alexis Lê-Quôc on Anomaly Detection

### Talk

###### Definitions
There are two types of anomaly:
* **Outliers** are space-based anomalies.
* **Anomalies** are time-based anomalies.

###### Anomaly detection algorithms
Datadog has customers that handle huge quantity of data in *real-time*. Therefore they need anomaly detection algorithms that are robust and scale well.

They have targeted two main algorithms:
* **MAD** (Mean Absolute Deviation) is robust as it is insensitive to time shift.
* **DBSCAN** (Density-Based Spatial Clustering of Applications with Noise) is a clustering technique that can identify outliers.

Both methods are time-window sensitive. They perform well on well-defined metrics such as latency, throughput and business metrics where seasonality is involved.

Anomaly detection is a hard technical problem. Besides *no learning* can be done as they are *no labels* on the data Datadog handles.

###### Typical use cases
Typical use cases involve Round-robin web requests as they can easily be modelized as independent statistical processes and GBM (Generic Buffer Management) heap monitoring.

For instance, Datadog can provide a forecast of the next 30 minutes at a 10 seconds granularity. Forecast is defined as:

*Forecast* = *Predicted Trend* + *Seasonal pattern* + *Tolerance*

###### Closing thoughts
* For sure any customer dealing with anomaly detection will be scared of
false negatives - i.e. when you cannot identify the anomaly - as you would
miss an important event that may hurt your business.
* In practice customers get annoyed by false positive - i.e. when you thought
you had identified an anomaly when it was not - as processing the anomaly
alert requires manual labor and is time consuming.

Final advices when it comes to any anomaly detection product are:
* **Test** your product **to death**. Test it over and over until it meets your
quality requirement.
* **Do not oversell** your product. False positives are unavoidable.

### Questions
1. What was the metric used on DBSCAN?
  *Absolute Error.*
2. Do you have examples of false negatives?
  *Earthquakes in Japan made the traffic blow up unexpectedly. We could not forecast this traffic spike.*

***

## Alice Coucke on offline and privacy-by-design voice assistant at Snips

### Talk

###### Opening thoughs
Voice interaction on devices has skyrocketed in the United States as 20% of the adult population now uses it. It represents around 50 millions users that have adopted this technology in the last 2 years. Cable television and even the Internet were much slower in comparison.

###### Snips in a nutshell
This however raises the question of data privacy in
an ever more connected world. Snips therefore decided on creating an offline and on-device voice interaction solution. Two advantages that directly come to mind are:
* *Privacy-by-design* as the users data does not leave their device.
* *Resilience* to cloud-outrage.
* *Speed increase* due to lack of server round trip time.

However these features come at a price of a few constraints, namely:
* There is no data transfered from the users devices to a central server in
order to improve the underlying models.
* The devices only offer limited CPU 1 resources.
These challenges also raise additional questions:
* What performance is achievable when using an offline and on-device solu-
tion?
* How can we evaluate the performance if no data is ever send from the
users to the company developing the models?

###### Voice interaction workflow
When it comes to voice interaction devices there is a general workflow one encounters:
1. First the device is waken-up when it detects the wakeword.
2. Second the speed recognition module records the order and transform it
into text.
3. Third the NLU (Natural Language Understanding) unit decyphers the order meaning.
4. Eventually an action or a dialog response ensues.

###### Zoom on the recognition and NLU modules
Any voice interaction device has to process an analog signal - in this case voice - and turn it into digital information to be processed - in this case text. There needs to be an *Acoustic Model* that takes the analog signal as input and transform it into a bunch of phonetic syllabus that are fed into the *Language Model* that turns the phonetic syllabus into actual words of the targeted language.

This process and the two models associated to it are data and processing
intensive tasks. The models are also too big.

There are however solutions to overcome these constraints:
• Models too big can be *compressed* and undergo *features selection*.
• Models can be *specialized* to the use case they are meant to cover.

As a result the device requires less data that can directly be taken from
public sources. Models are so small that even a Raspberry Pi can run it - between 100 kbs to a few Mbs.

###### NLU details
The NLU unit parses the intent of the order that the device recorded. There are two tasks to be performed:
* **Intent classification** aims at understanding the intent embedded in the
vocal order - i.e. *Booking a cab.*
* **Entity extraction** aims at collecting the surrounding information neces-
sary to perform the task. - i.e. *At 7 pm on Main Street.*

Snips NLU has open-sourced its software on GitHub as a commitment to its
transparency and privacy-seeking values.

###### Model evaluation
Model evaluation is tricky given that there cannot be direct feedback from the users devices.

Snips team has therefore chosen to benchmark its offline device to other
competitors such as Google, Amazon and Apple voice assistants. Results show
that regardless of who comes first the overall accuracy suggests that nobody has cracked the Natural Language Understanding task yet.

###### Data constraints
As no data can be retrieved from the users to feed the giant models on a remote server Snips had to come up with a way to generate training data.

They stand between two extremes when it comes to data generation:
* **Query pattern** tries every possible combination of words to create meaningful sentence. It is not flexible but cannot be corrupted easily.
* **Crowdsourcing** relies on people to label audio samples with relevant
content. This can be prone to errors but is far more flexible.

Snips even started to offer *data generation* as a service although it was not in the blocks originally.

###### More on Snips
Snips runs on **Python** to develop and train its models and
rely on **Rust** for software portability.

### Questions
1. What Neural Network model is Snips using for the wakeword detection?

*Big players have barely started to roll out complex Neural Nets models.*
N.B. : I did not catch the whole answer.


***

## Andrew Trask on OpenMined

### Talk 

###### Opening
OpenMined is an open source community dedicated to build and promote **secure**, **privacy-by-design** and **value-aligned** AI.

Its main activies revolve around **raising awareness** on the ethics in AI and **empower tools** such as *PyTorch*, *Keras* and *Tensorflow* with features that implement the values OpenMined stands for.

###### AI Business Model
The first question that comes to mind is *why has AI not been built with such features by default?* A logical explanation lies in AI
standard Business Model that can be summed up as:

1. **Acquire Data** to train machine learning models.
This practice has two major flaws:
  * Firstly the user **loses control** over its **privacy**.
  * Sensitive products - *such as AI suicide watch* - cannot be developed as it transgresses moral grounds.

2. Train & Predict using the acquired data.

This practice has again inherent flaws:
  * It creates a contagious privacy loss as the data acquired from other
users in the training phase might lead to predicting information on
other users - *e.g. Twitter location from some users can be traced to their pictures so that other users pictures that can be traced to their location although it was not mentioned anywhere on Twitter.*
  * Models can be biased or unfair.

###### Potential solutions
Potential solutions might come from keeping the data where it is using **Federated Learning**. Federated Learning sends the model to the users and update it with the user data.

Federated Learning however does not exists in common DL (Deep Learning) frameworks.
Besides this update mechanism is not entirely harmless as information on the data can be found in the models gradient update - this is called *differential privacy*.

This limit can be lifted using encryption mechanisms, such as:
* **Homomorphic Encryption** that enables doing maths with cyphers.
There are multiple variants of Homomorphic Encryption but it is painfully
slow.
* **Multi-Party Computation** that splits a big number into shares to con-
duct peer-to-peer computation.
This option is much faster than Homomorphic Encryption.

### Questions

1. How can you trust other users using Multi-Party Computation?
  *It works as long as all the other members do not collude to get your shares- and therefore your data.*
2. How to deal with number fudging? What if users willingly corrupt the data they are sending?
  *You can spot corruption very easily as numbers explode.*
3. Does Multi-Party Computation make it difficult to add noise?
  *For that very reason, very. It will corrupt the numbers - the weights.*


***

## Julien Cornebise on AI for Good

### Talk

###### Opening
As a research student and after 4 years at Google DeepMind crafting
ever more complex models Julien grew unsatisfied not to have a positive impact using AI. It is a shame we allocate the mental resources of our most brilliant minds only to empower finance and improve targeted advertisement when we could instead use it to help people in need.

###### Collaborating with Amnesty International
Julien spent a year using Deep Learning to decode Darfour - located in South Sudan. He relied on satellite data to train a model to identify South Sudan villages that are composed of characteristic huts called tukuls. Once tukuls were identified the model was used to spot villages that had been burned by the army to scare the villagers.
The goal was to provide evidence to Amnesty International so that they could launch a lawsuit against the South Sudan government.

Along his journey, Julien had to go back to the very fundamental basics
of statistics to accompany the NGO (Non Governmental Organization). Instead of optimizing the latest state-of-the-art models to publish yet another paper he was left with the satisfaction of having contributed to the greater good. His product was meant to be used by end-users that are not tech savvy. It involves a lot more data engineering and careful thinking to provide a viable solution.

###### Closing thoughts
It’s time we use this wonderful technology to its full extent and dedicate our time to doing *good*. Research is not useless by any mean.
However it should be used to solve real-world problems such as those that the NGOs face.

***

### Disclaimer

* The reported answers in the *Questions* sub-sections are not an actual words for words transcription. They are my own interpretation of the answers.

* If you want to contact me use the website main page.


