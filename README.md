# Flask_test_app2

In this project I focused on CI/CD process. I made simple flask app and wrote tests to it. Application code was sent on github. 
I created Jenkins file that performs: building, testing and deploying docker image to the my dockerhub account. 

The build and test stages were performed in 
docker containers inside Jenkins. The Docker image that ran through the entire pipeline was uploaded to my repo on dockerhub.

The following technologies were used to complete the project:
Jenkins, Docker( and Docker in docker aka docker:dind ), Flask, Python.
