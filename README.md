# Retrieval-augmented-Encoding-for-Dense-Video-Captioning
Code for paper Retrieval augmented encoding for dense video captioning
To use this code you need faster rcnn features of youcook2 videos.
1. Download youcook2 videos.
2. Extract faster rcnn features from [updown](https://github.com/peteanderson80/bottom-up-attention) (Detailed instruction for extraction process can be found in [LXMERT](https://github.com/airsplay/lxmert) )

For training evidence retrieval,
  1. Download source code of [LXMERT](https://github.com/airsplay/lxmert) into current folder and pretrained weight following instruction in repository.
  2. For faster loading of features, Rearrange object feature extracted from faster rcnn and gold annotation of youocook2.
  3. To download recipes, we used [public api](https://apify.com/dtrungtin/allrecipes-scraper/api) and queried each food name annotated by youcook2.(we used 20 top documents.)
  4. Preprocess downloaded recipes.(Downloaded recipe document will be sorted in each step. We further splitted each step into single sentences)
  5. Save processed recipes. (It correspond to yc2_recipes.json in code)
  6. For dictionary and prior, we used dictionary and prior from [DeVLBert](https://github.com/shengyuzhang/DeVLBert)
  7. Train evidence retriever and retrieve recipe trained model (we used model with best validation R@1)
  8. Conduct inference retrieval for recipes. (To train Dense video captioning, you need to infer in training, validation and test set)
  9. Store retrieved evidence into pklfile. 

For training Dense video captioning
 1. Download source code of [EMT](https://github.com/salesforce/densecap) in current folder. (You may need to revise some sourcecode due to old torch version in original source code)
 2. To extract cross-modal representation of video frame, we fed extracted faster rcnn feature and its aligned transcript to LXMERT and used resulting [CLS] representation.
 3. To encode recipe sentences, we used [CLS] representation from Bert-base-uncased model. (This resulting file corresponds to 'recipe_feat.pkl' in sourcecode) 
 4. Train with all the encodings
