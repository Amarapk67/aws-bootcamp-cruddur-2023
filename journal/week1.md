# Week 1 — App Containerization

## Here is quick summary of week one of AWS cloud bootcamp:
- First we Launch our project code  via Gitpod,
- then we contanerized our app ( Frontend and Backend) using dockerfile created in frontend-react-js and backend-flask directories.
- created a docker-compose yml file in the root directory and add both front end and backend environments so we could run or stop both containers.
- Installed NPM in frontend-react-js and unlocked port 3000 so we could be able to access the Front end of the Cruddur app.
- Wrote a React Page Notification by creating creating new page in Front-react-js/srs/pages/NotificationsFeedpage
- Wrote a flask backend notification endpoint by creating a new api endpoint /api/activities/notifications for backend and make port 4567 public so we when we append  notification api to the backend url, and if a json file returned, this means the api was configure correctly.
  We containerized Postgres and DynamoDB local by adding them to the docker come yml.
  
  
![Front end Notification!](https://media.discordapp.net/attachments/1057351515905458279/1082677326925541377/front_notifications.PNG?width=749&height=326)

![Backend json notification!](https://media.discordapp.net/attachments/1057351515905458279/1082677326589988874/backend_json_api_for_activities_notifications_2.PNG?width=749&height=364)
