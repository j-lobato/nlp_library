
def setting2():
    from IPython.core.display import display, HTML
    display(HTML("<style>.container { width:99% !important; }</style>"))
    display(HTML("<style>.output_result { max-width:99% !important; }</style>"))
    display(HTML("<style>.prompt { display:none !important; }</style>"))
    import pandas as pd
    pd.options.display.max_colwidth =1800
    
def setting():
    import pandas as pd
    pd.options.display.max_colwidth =1800
    from IPython.display import display, HTML

    display(HTML(data="""
    <style>
        div#notebook-container    { width: 99%; }
        div#menubar-container     { width: 99%; hight: 70%;}
        div#maintoolbar-container { width: 99%; }
    </style>
    """))

# extract texts with keywords from columns in a df

def keyword_extract(keywords_list, text_column):
    text_combined = []
    for keyword in text_column:
        for i in range(0, len(keywords_list)):
            if keywords_list[i] in keyword.lower():
                text_combined.append(keyword)
                break
    return text_combined     
    

def simple_clean(text):
    import re
    text = text.lower()
    text = re.sub(r'(\w+@[a-zA-Z]+?\.[a-zA-Z]{2,6})', ' ', text) # removes email
    text = re.sub('\\n', ' ', text)
    text = re.sub(r'\W', ' ', text) # removes non-words characters
    
    return text

def clean_keep(text):
    import re
    text = text.lower()
    text = re.sub('\\n', ' ', text)
    text = re.sub('\n', ' ', text)
    text = re.sub('\n\n', ' ', text)
    text = re.sub('\n', ' ', text)
    text = re.sub(r'\\xa0', ' ',text)
    text = re.sub(r'\s+[a-zA-Z]\s+', ' ', text) # removes 1 character words 
    return text

def clean(text):
    import re
    text = text.lower()
    text = re.sub(r'<[^>]+>', ' ', text) # html tags
    text = re.sub(r'http\S+|www.\S+', ' ', text)
    text = re.sub(r'https?://\S+', ' ', text) # URLs
    text = re.sub(r'\W', ' ', text)  # removes non-words characters
    text = re.sub('\\n', ' ', text)
    text = re.sub('\n', ' ', text)
    text = re.sub('\n\n', ' ', text)
    text = re.sub('\n', ' ', text)
    text = re.sub('[^a-zA-Z\s]', ' ', text) #links
    text = re.sub(r'\\xa0', ' ',text)
    text = re.sub(r'\s+[a-zA-Z]\s+', ' ', text) # removes 1 character words 

    #text = re.sub('([0-9]+)', ' ', text) # remove numbers
    return text


def lemma(text):
    from nltk.stem.wordnet import WordNetLemmatizer
    from nltk.tokenize import RegexpTokenizer
    tokenizer = RegexpTokenizer(r'\w+')
    lemmatizer = WordNetLemmatizer()
    tokens = tokenizer.tokenize(str(text))
    lems = [lemmatizer.lemmatize(i) for i in tokens]
    return (" ".join(lems))


def stemmer_tok(text):
    import nltk
    from nltk.tokenize import word_tokenize
    from nltk.tokenize import sent_tokenize
    from nltk.stem.snowball import SnowballStemmer
    import re
    stemmer = SnowballStemmer("english")
    # first tokenize by sentence, then by word to ensure that punctuation is caught as it's own token
    tokens = [word for sent in nltk.sent_tokenize(text) for word in nltk.word_tokenize(sent)]
    filtered_tokens = []
    # filter out any tokens not containing letters (e.g., numeric tokens, raw punctuation)
    for token in tokens:
        if re.search('[a-zA-Z]', token):
            filtered_tokens.append(token)
    stems = [stemmer.stem(t) for t in filtered_tokens]
    return stems

    
def lemma_stopwords(text, lang):
    from nltk.stem.wordnet import WordNetLemmatizer
    from nltk.tokenize import RegexpTokenizer
    from nltk.corpus import stopwords

    tokenizer = RegexpTokenizer(r'\w+')
    
    stopwords = set(stopwords.words(lang))
    
    lemmatizer = WordNetLemmatizer()
    
    tokens = tokenizer.tokenize(str(text))
    tokens2 =  [item for item in tokens if item not in stopwords]
    lems = [lemmatizer.lemmatize(i) for i in tokens2]
    return (" ".join(lems))

def tweet_short(keyword, number_tweets, lang):

    import tweepy
    from tweepy import Stream
    from tweepy import OAuthHandler
    from tweepy.streaming import StreamListener
    import sys
    import datetime

    import pandas as pd
    pd.options.display.max_colwidth =1800
    
    # keys

    consumer_key = 'PAKlkz9EAKgmRP2d0OIDbEIYm'
    consumer_secret = 'UbGYhpaSyJOQxPJPnM3mQhx4u597JdxptXuyyignaFjEx78D6z'
    access_token = '1177114275959255040-NGvHhb9XXY8y1aAw1nl0yGqpCFe0SL'
    access_token_secret = 'LrMD58UTMf1qMyl2I35yyso5fSMZuZA7Ba57TVYu0bnlu'

    auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    api = tweepy.API(auth) 



    #Switching to application authentication

    auth=tweepy.AppAuthHandler(consumer_key, consumer_secret)


    # TWEEPY

    # http://docs.tweepy.org/en/v3.5.0/api.html


    # Standard queries
    # https://developer.twitter.com/en/docs/tweets/rules-and-filtering/overview/standard-operators



    #Setting up new api wrapper, using authentication only

    api = tweepy.API(auth, wait_on_rate_limit=True,wait_on_rate_limit_notify=True) 

    if (not api):
        print('problem with api')



    listOfTweets = []

    keyword = keyword

    for tweet in tweepy.Cursor(api.search, q=keyword, tweet_mode='extended', lang=lang).items(number_tweets):
            # Add tweets in this format
            dict_ = {'Screen Name': tweet.user.screen_name,
                    'User Name': tweet.user.name,
                    'Tweet Created At': tweet.created_at,
                    'Tweet Text': tweet.full_text,
                    'User Location': tweet.user.location,
                    'Tweet Coordinates': tweet.coordinates,
                    'Retweet Count': tweet.retweet_count,
                    'Retweeted': tweet.retweeted,
                    'Phone Type': tweet.source,
                    'Favorite Count': tweet.favorite_count,
                    'Favorited': tweet.favorited,
                    'Replied': tweet.in_reply_to_status_id_str
                    }
            listOfTweets.append(dict_)
            df_tweets = pd.DataFrame(listOfTweets)
    return df_tweets




def tweet_long(keyword, lang):
    
    import pandas as pd
    from twitterscraper import query_tweets
    import datetime as dt

    lang = lang

    data = query_tweets(keyword, lang=lang)

    df = pd.DataFrame(t.__dict__ for t in data)
    
    return df



def stopwords_pt():
    import nltk
    stopwords = nltk.corpus.stopwords.words('portuguese')
    return stopwords

def newspapers(limit, json_, name_file):
    import feedparser as fp
    import json
    import newspaper
    from newspaper import Article
    from time import mktime
    from datetime import datetime
    import csv

    LIMIT = limit
    articles_array = []
    data = {}
    data['newspapers'] = {}

    # loads the JSON files with news sites
    with open(json_) as data_file:
        companies = json.load(data_file)
        
        
        
        
    count = 1

# Iterate through each news company
    for company, value in companies.items():
        # If a RSS link is provided in the JSON file, this will be the first choice.
        # Reason for this is that, RSS feeds often give more consistent and correct data. RSS (Rich Site Summary; originally RDF Site Summary; often called Really Simple Syndication) is a type of
        # web feed which allows users to access updates to online content in a standardized, computer-readable format
        # If you do not want to scrape from the RSS-feed, just leave the RSS attr empty in the JSON file.
        company_name = company
        if 'rss' in value:
            d = fp.parse(value['rss'])
            print("Downloading articles from ", company)
            newsPaper = {
                "rss": value['rss'],
                "link": value['link'],
                "articles": []
            }
            for entry in d.entries:
                # Check if publish date is provided, if no the article is skipped.
                # This is done to keep consistency in the data and to keep the script from crashing.
                if hasattr(entry, 'published'):
                    if count > LIMIT:
                        break
                    article = {}
                    article['link'] = entry.link
                    date = entry.published_parsed
                    article['published'] = datetime.fromtimestamp(mktime(date)).isoformat()
                    try:
                        content = Article(entry.link)
                        content.download()
                        content.parse()
                    except Exception as e:
                        # If the download for some reason fails (ex. 404) the script will continue downloading
                        # the next article.
                        print(e)
                        print("continuing...")
                        continue
                    article['site'] = company_name
                    article['title'] = content.title
                    article['text'] = content.text
                    article['authors'] = content.authors
                    article['top_image'] =  content.top_image
                    article['movies'] = content.movies
                    newsPaper['articles'].append(article)
                    articles_array.append(article)
                    print(count, "articles downloaded from", company, ", url: ", entry.link)
                    count = count + 1
        else:
            # This is the fallback method if a RSS-feed link is not provided.
            # It uses the python newspaper library to extract articles
            print("Building site for ", company)
            paper = newspaper.build(value['link'], memoize_articles=False)
            newsPaper = {
                "link": value['link'],
                "articles": []
            }
            noneTypeCount = 0
            for content in paper.articles:
                if count > LIMIT:
                    break
                try:
                    content.download()
                    content.parse()
                except Exception as e:
                    print(e)
                    print("continuing...")
                    continue
                # Again, for consistency, if there is no found publish date the article will be skipped.
                # After 10 downloaded articles from the same newspaper without publish date, the company will be skipped.

                article = {}
                article['site'] = company_name
                article['title'] = content.title
                article['authors'] = content.authors
                article['text'] = content.text
                article['top_image'] =  content.top_image
                article['movies'] = content.movies
                article['link'] = content.url
                article['published'] = content.publish_date
                newsPaper['articles'].append(article)
                articles_array.append(article)
                print(count, "articles downloaded from", company, " using newspaper, url: ", content.url)
                count = count + 1
                #noneTypeCount = 0
        count = 1
        data['newspapers'][company] = newsPaper


    try:
        name_file = name_file    
        f = csv.writer(open(name_file+'.csv', 'w', encoding='utf-8'))
        f.writerow(['Title', 'Website', 'Authors','Text','Link','Published_Date'])
        #print(article)
        for artist_name in articles_array:
            title = artist_name['title']
            website=company_name
            authors=artist_name['authors']
            text=artist_name['text']
            link=artist_name['link']
            publish_date=artist_name['published']

            # Add each artist’s name and associated link to a row
            f.writerow([title, website, authors, text, link, publish_date])
    except Exception as e: print(e)
