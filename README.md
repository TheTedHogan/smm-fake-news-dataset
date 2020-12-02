# smm-fake-news-dataset

Code snippet to add image count, authors count, keywords count, and the time of date published

newsUserContent.loc[newsUserContent["images"].isna(), "images"] = "[]"
newsUserContent.loc[newsUserContent["authors"].isna(), "authors"] = "[]"
newsUserContent.loc[newsUserContent["keywords"].isna(), "keywords"] = "[]"

newsUserContent['countImages'] = newsUserContent.apply(lambda x: len(eval(x["images"])), axis=1)
newsUserContent['countAuthors'] = newsUserContent.apply(lambda x: len(eval(x["authors"])), axis=1)
newsUserContent['countKeywords'] = newsUserContent.apply(lambda x: len(eval(x["keywords"])), axis=1)


def pub_date_to_datetime(pub_date):
    matcher = re.search(r"(\d{13})", pub_date)
    match = matcher.group(0)
    timestamp_str = match[:-3]
    timestamp = int(timestamp_str)
    pub_datetime = datetime.utcfromtimestamp(timestamp)
    return pub_datetime
newsUserContent.loc[newsUserContent["publish_date"].isna(), "publish_date"] =  "{'$date': 0000000000000}"
newsUserContent["publish_date"] = newsUserContent.apply(lambda r: pub_date_to_datetime(r["publish_date"]) , axis=1)
newsUserContent["publish_tod"]  = pd.to_datetime(newsUserContent["publish_date"] ,format= '%H:%M:%S' ).dt.time
