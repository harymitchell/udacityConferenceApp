App Engine application for the Udacity training course.

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

## Session Design Choices
    
Sessions consist of a name, highlights, a list of speakers, duration, type, date, and time.
Speakers are implemented only as string values.

## Featured Speakers
Upon created of a session, for sessions with any speakers, we add a task to the default queue with the URL /tasks/add_featured_speaker.
In main.py, AddFeaturedSpeaker passes the session Key back to ConferenceApi._addFeaturedSpeaker, which will examine the speakers.
ConferenceApi._addFeaturedSpeaker builds a data structure featuredSpeakers which is list of TUPLE (speaker, [session.name]),
and this structure is added in the memcache. GetFeaturedSpeaker then has the responsibiliy of retrieval and conversion.
This process does support multiple featured speakers, if a session is created with multiple speakers who are speaking in other sessions.

## Other design considerations
For the problem of those who dont like workshops or sessions before 7pm:
getSessionsAfterTimeExcludingType:
The encountered problem is "BadRequestError: Only one inequality filter per query is supported.",
which can be solved by using twp separate queries for each inequality, getting the intersection
of the keys, and then retrieving the entities from the intersected keys.

## Other queries
1. getSessionsByMinimumDuration returns all Sessions for given conference with a minimum duration as requested.
1. getOpeningDaySessions returns all Sessions on opening day of given conference.

[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
