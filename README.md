# Conference-Organization-App
Udacity P4, Full Stack - First time with App Engine

date: 29/12/2015

Conference-Organization-App is the fourth project of Full Stack Web Developer Nanodegree of Udacity.
In this project I used Python, and the App Engine of Google. 
The objective of the work was develop an API for the application.
The app is online here: https://full-stack-4.appspot.com/

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
(I hope that my English is understandable)

### The `Session` kind, and the `Speaker` string:

It has in particular the following parameter: `name`, `speaker[]`, `typeOfSession[]`.
Because of the `class SessionForm`, Session is a child of Conference, it can't be created without a conference key.
A new session is created with the endpoint `createSession` that call `_createSessionObject` 
and it is transformed in `SessionForm` obj by `_copySessionToForm`.
- The `name` of the sessios is require.
- The `speaker[]`, is an array of string, is not an array of entity of the kind Speaker.
I have not implemented the kind Speaker.

Pros: Faster implementation. eg: The method to get all the sessions:
	`allSess = Session.query()`
        `allSess = allSess.filter(Session.speaker == request.speaker)`
      There isn't keys, so the implementation of it is very easy.
      
Cons: Bad control of it, in my implementation the name of a speaker is unique. 
      There isn't the possibility of 2 speacker with the same name.
      Also, the implementation of the kind speaker offers more possibilities then mine.
The function `getSessionsBySpeaker` filter by `speaker[]`
- The function `getConferenceSessionsByType` filter by `typeOfSession[]`
- All parameter are String property, except for duration and startTime that are time property and date that is a date property. In this way you can use inequality filter in theese property.


### The other 2 queries:

- `getConferenceSessionsBySpeaker`, it's like getSessionsBySpeaker but it returns the only sessions of a specific conference.
- `getSeatsAvailableInSession`, I thought about the session wishlist. Maybe a user has entered in its wishlist a session, but this session is not a session of a joined conference. So the method provides an easy answer to the user who wonders whether the conference with the session in wishlist, has seats available. 



### The query to solve:

My solution is at the endpoint `noWorkshopAndBeforeSeven`.
The problem was that with the non-sql database(datastore), 
you can't filter two different properties with inequality filters.
You can use the inequality filter only in one property. It's a problem with indexes.
My solution is to query all sessions:
`sessions = Session.query()`
Then filter by the startTime parameter:
`Session.startTime <= datetime.strptime("7:00 pm", "%I:%M %p").time()`
So now from this results I have to take only the ones that don't contains workshop in their typeOfSessions:
`final_sessions = [session for session in sessions if 'Workshop' not in session.typeOfSession]`
Then the results return as SessionForms.



Author: Francesco Gusella

Contacts: gus815@gmail.com
