# goal 
The goal of this project was to begin learning about the specificities of torch(what are the object that are manipulated in torch model, how could i vizualise the various datas).

In order to make this process more interesting, I used the kaggle challenge https://www.kaggle.com/c/sartorius-cell-instance-segmentation as a way to learn from real data, as well as well as progress from reading other people code.

## Specificities of the challenge : 
Challenge = segmentation different neurons cell. Each cell correspound to one segmentation mask. 

## Objective : 
define a custum dataloader & interactive visualisation. 
In order to define the right data, i explored a bit the mask rcnn model from torchvision to understand the needed data for the model. However, I didn't implement any model (this will be a project to come)

## Material : 
- data from kaggle
- I also used a notebook  https://www.kaggle.com/julian3833/sartorius-starter-torch-mask-r-cnn-lb-0-273/notebook because it was implemented in pytorch. 
- documentation from the kaggle website


## what i've done 
(in chronological order, start from a first basic implementation of the dataset & visualisation on plotly) (for the next project, i'll use git push instead of this)
- visualisation tool : added an image of random color with some transparancy
- change the representation of the masks : the get methode return the image & mask of size (height,width,n_labels) (idea came from kaggle, better for nn architecture such as torch vision) 
- adapted all visualisation for thoses new representation :
    - move the decoding part to the __getitem__ methode
    - change the visualisation of the data to match the new representation
- decoding function too slow : 
    - found a better implementation of rle decoding 
- decoding the data is still very slow. Is it a problem ?
    - not a problem if we use gpu for training & cpu is sufficient
    - if it become a botteneck, we could use some other strategies to solve the problem
- issue with the dataloader : variable lenght for the mask. we have to define our own collate function.
    - first collide function based on the article
- issues of performances : 
    - get a memorry error (5min run time) : decrease the batch size & close internet pages to increase available cpus
    - 30s per batch (12min total)-> memory is insuficient (cpu)
    - change the dtype for the mask to speed up the process
    - savin mask in order to speed up the process -> test shows that the speeds up the process. However, it takes too much place on disk (more than 10Go), x2 times for first loop. 
	- changing the saving file : representation with a mask with value correspounding to the value of the number of the class : speed up the process, but overlapping masks are not well supported
	- 
## other things i've learn :

- the utility of tranform : because the __getitem is called every time, we can use transform to perform data augmentation 
- structure of torchvision rcnn : there is a general definition of the faster-rcnn/masked rcnn algorithm. it can be extended to implement a given version of the model (changing the convolutional layer, or the detection head). 
- importance of some parameters : when browsing some submission, I realise that some parameters where way more important than i thought (like initilaisation of the weight for exemple). 
- other possible representation of the data can be possible?  (however, we can't use the torchvision package directly
	- representation as polygons :  https://www.kaggle.com/ctawong/cell-instance-segmentation-detectron2-mask-rcnn/ 
	- representation as rle: sould define a layer with a custum backpropagation step for the final mask results




    
