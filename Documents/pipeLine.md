###CircleCI Environment Variables:
    we use these variables dynamically and then when deploying we add them to our app via a script.


#Using CircleCi
    we configure the build steps in .CircleCi folder config.yml file
    we set our (orbs - jobs - workflow)
        ##Orbs:

            are the pipleline's apps required to run the successfull pipleline deployment
            -aws/Cli aws cmd line  
            -aws eb will contain our env variables and backend,
            -node  our server running env
        ##Jobs:
        
            the actual commands of the piple line
            build:
                1. initiate a node instance using docker img on the server.
                2. install the dependencies:
                    -FRONT-END DEPENDENCIES: using cmd "npm run frontend:install"
                    -BACK-END DEPENDENCIES: using cmd "npm run api:install" 
                3. lint the code:
                    - FRONT END: using cmd "npm run frontend:lint"
                4.Build the code:
                    -FRONT-END: using cmd "npm run frontend:build"
                    -BACK-END :using cmd "npm run api:build"
            deploy:
                1. we use a ubuntu linux based img using docker "cimg/base:stable"
                2. install node v 16.17.0 & elasticbean & aws cli
                3.deplying the app:
                    -FRONT-END & BACK-END using cmd "npm run deploy"

        ##WorkFlow:
            here we set out order of excuting our job's on the server
                first we require the build TO FINISH  then Hold till (master User) approves to deploy
                using "- hold:
                        filters:
                            branches:
                            only:
                                - master
                        type: approval"
                second deploy the code after -hold command gets approved


