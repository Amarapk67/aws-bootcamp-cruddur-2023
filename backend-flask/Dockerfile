FROM python:3.10-slim-buster

# Container
#make a new folder inside the container
WORKDIR /backend-flask

# Outside Container -> Inside Conainer
#this contianer the librarys we want to install
COPY requirements.txt requirements.txt

#inside Container
#Install the python libraries used for the app
RUN pip3 install -r requirements.txt

#Outsode con continer -> inside container
#. means everything in the current directory
# first period.  / back end
COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}
#CMD (Command)
# python3 -m flask --host=0.0.0.0 --port=4567
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]