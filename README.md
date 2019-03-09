
# Nutrivali News BackEnd Service

### What is that ?
This system was build to populate the news repository for [Nutrivali App](https://github.com/wiltonribeiro/nutrivali-app). The server is running on Node.js and it's using News API to request the News and the database used to save is MongoDB. 

##How it works?
The server is responsible to save the languages and key words by language that will be used to request the news and save it on database. Each language has a unique code name(e.g pt, en, de) and an array of keywords. Keywords are words or phrases to search for in the API. The server is prepared to populate the database with 50 article by language. The news are saved sorted by publication date, and the server will only save a new Article if the one was published after the last one saved by that language, that mechanism avoid the system to duplicate articles and keep the database always sorted.

## Database Model

#### Language Model 
![alt text](https://github.com/wiltonribeiro/nutrivali-news-backend/blob/master/docs/language_model.png "Language Model")

#### Article Model 
![alt text](https://github.com/wiltonribeiro/nutrivali-news-backend/blob/master/docs/article_model.png "Article Model")

#### News Model 
![alt text](https://github.com/wiltonribeiro/nutrivali-news-backend/blob/master/docs/news_model.png "News Model")

## API Methods

### GET /
Return the version of running server, database status and last request made to populate the datasbase.

Return format:
```javascript
{
"version":"<string>", //production version
"databse":"<string>" //connection status(connected, disconnected, connecting, disconnecting)
"last_request":"<string>" //data time in string format
}
```

### GET /languages
Return all languages registrated.

Return format:
```javascript
{[
  {
    "keyWord": ["<string>"], //KeyWord to search for
    "lang":"<string>", //Language conde
    "_id":"<string>" //Language id generated by MongoDB
  }, ...
]}
```

### GET /language/:lang
Return the langauge that you are looking for.

Return format:
```javascript
{
  {
    "keyWord": ["<string>"], //KeyWord to search for
    "lang":"<string>", //Language conde
    "_id":"<string>" //Language id generated by MongoDB
  }
}
```
> If the language not exists in database the status code of return will be 404.

### POST /language/:lang/remove
Remove the specific language.

> The return will be 200 if language has been removed successful

### POST /language

Save language

Format:
```javascript
{
  {
    "keyWord": ["<string>"], // (required, unique) KeyWords to search for
    "lang":"<string>" //Language conde
  }
}
```

### GET pupulate/news/all

This method has been used has Cron Job in App Engine. In my case, in every 3 hours the App Engine call this method to populate the database with new articles to each language. By defaul all http call by Cron Job must be a GET.

> The return will be 200 if excuted with success, and 401 if who is calling this method is not the Cron Job.


