# FAQ_rank


<b>This repository is currently being updated...</b>

The code belonging to :

Gonzalez-Garduno, Ana Valeria; Augenstein, Isabelle; Søgaard, Anders. 2018. A strong baseline for question relevancy ranking. Conference on Empirical Methods in Natural Language Processing (EMNLP) 2018. Brussels, Belgium.

note: extra data preprocessing has been added to improve accuracies from what has been reported
### Before Running

The code was written in python 3.5 and requires keras (with tensorflow backend). Gensim needs to be installed as well as nltk. The code requires nltk data to be downloaded. If not downloaded already, type the following in the command terminal:

          >> python -c "import nltk; nltk.Download()"


The model uses the pretrained GloVe embeddings found here: https://nlp.stanford.edu/projects/glove/ .Specifically we use the vectors trained on Wikipedia (<i>glove.6B.50d.txt</i>). We place the embeddings in the folder <b><i>feature_extraction/</b></i>, however you can place them wherever and specify their location in the script extract_features.py
          
### Extracting queries from XML files
The files used to extract the train, dev and test sets are under <b><i> QA_data/semEval_data/ </i></b>...  to extract the queries and relevant information for preprocessing simply go into the directory <b><i>feature_extraction/</b></i> and run :

          >> python run_queryExtractor.py
          
the extracted queries and other data will be dumped in <b><i>QA_data/data_dumps/</i></b>

### Extracting feature vectors
Once the queries have been extracted from the XML files run the following script:

          >> python extract_features.py [path_to_glove_embeddings][path_to_data_dumps]
          
    i.e.  >> python extract_features.py glove.6B.50d.txt /Users/username/FAQ_rank/QA_data/data_dumps

This will create a list of vectors and labels in the following format:

[vectors_train,
vectors_dev,
vectors_test,
labels_train,
labels_dev,
labels_test]

This will be dumped in the data_dumps folder.

### Training a STL model

Once the vector files are created, one can easily train and test a single task model. Go into the <b><i>models</b></i> folder and run:
    
        >> python stl_mlp.py  [model_name] [path_to_vectors]
    
        i.e. >> python stl_mlp.py  test_model /Users/username/FAQ_rank/QA_data/data_dumps/qq_data.p

### Evaluation
Prepare following files under folder <b><i> QA_data/semEval_data/english_scorer_and_random_baselines.v2.2/v2.2/ </i></b>

[1] SemEval2016-Task3-CQA-QL-test.xml.subtaskB.relevancy file by running:
          >> perl SemEval2016_task3_English_random_baseline_subtaskB.pl SemEval2016-Task3-CQA-QL-test.xml
     (it can also generates SemEval2016-Task3-CQA-QL-test.xml.subtaskB.pred)
[2]  Prediction_file (created from provided test dataset and probability output)
#### IR baseline & Tested System
running:
         >> python MAP_scripts/ev.py SemEval2016-Task3-CQA-QL-test.xml.subtaskB.relevancy Prediction_file
         
          
#### IR baseline & Random baseline
running:
        >> python MAP_scripts/ev.py SemEval2016-Task3-CQA-QL-test.xml.subtaskB.relevancy SemEval2016-Task3-CQA-QL-                                test.xml.subtaskB.pred

