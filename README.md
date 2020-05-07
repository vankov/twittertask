# twittertask
Inferring the location, date and sentiment of tweets about concerts

The task is completed in two stages. 

The first stage is to filter the twitter data set and leave only tweets which refer to a concert and contain an artist name. To this end, we first tokenize and lemmatize the tweets and then look for words indicating whether the tweet is about a concert by looking up a dictionary of "concert" words (i.e. "concert", "performance", etc"). We then check whether the selected tweets also contain the name of an artist. We use two databases of known artist - a list of top 100 most followed music performers and a list of music artists provided by TwitterNER. At the end of stage we are left with tweets which refer to a concert and contain the name of known performers. The filtered dataset is stored for further processing

In the second stage we load the filtered dataset with relevant tweets and try to identify the location and date of the artist and sentiment of the tweet. To this end, we further process the tweets by using  pretrained POS and NER taggers, provided by SparkNLP. The NER tagger is used to identify tokens in the tweets which refer to locations. The results are not very good, so we further check these tokens whether they are appropriate parts of speech, using the POS tags. We also check the locations against a database of locations and venues provided byt TwitterNER. The sentiment analysis is done by a pretrained SparkNLP model. Currently SparkNLP NER tagger doesn't encode date tokens, so we use the python module "Spacy" to detect date phrases. We then try to infer the correct date using another python library - "dateparser". At the end of stage 2, for each tweet we have the list of artists mentioned, a list of locations, a list of recognized (i.e. known) locations, a list of detected date phrases and a list of datetime objects. Of course, we also have the sentiment of each tweet.

Notes:

1. The pretrained NER tagger doesn't work well with twitter data. Using a model specifically designed and trained on twitter data will achieve much better results (f.e. TwitterNER).
2. I guess sentiment analysis doesn't work well for the same reason, but I haven't checked this.
3. Results can be improved by considering the grammatical strucutre of the tweets, rather than just the NER tags. It is possible that a tweet is about a concert, mentions a certain artist and location, but yet this doesn't mean that this artist had a concert at the given location. We can use the SparkNLP dependency parser or some other pretrained model to improve results, but again we have the problem that tweets don't look very much like "normal" text, which pretrained models are usually trained on.
