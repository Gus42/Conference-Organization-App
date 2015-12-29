# Conference-Organization-App
Udacity P4, Full Stack - First time with App Engine

date: 29/12/2015

Conference-Organization-App is the fourth project of Full Stack Web Developer Nanodegree of Udacity.
In this project I used Python, and the App Engine of google. 
The objective of the work was develop an API for the application.

## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool

## Design Decisions

The Session kind:
It has the following parameter:
    name 
    highlights  
    speaker
    duration
    typeOfSession
    date 
    startTime
    location 
Session is a child of Conference.

About the speaker: The app save the speakers of the sessions in an array of string.
In this way the implementation is very easy. The method to get all the sessions
of a specific speaker is very short:
	allSess = Session.query()
    allSess = allSess.filter(Session.speaker == request.speaker)
