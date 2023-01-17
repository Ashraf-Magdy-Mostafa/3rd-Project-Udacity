###hint : we must install and configure aws Cli (https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
    u should crate IAM user using the console and get it's (AWS_ACCESS_KEY_ID & AWS_SECRET_ACCESS_KEY) preffer creating a user with password adn user name for manually login using aws console, open the cmd and type "aws configure" then enter your IAM access-key & secret-access-key & the rest is default. all good to go now using your cmd to connect to your aws


#RDS-Postgres

        we create a aws RDS using Postgres instance, 
        we set the values ( username-password-DBname-port) and pass it via CircleCi Environment Variables.
        make the DB public (using the console), 
        edit the inbound rules to allow our app to work with it with no proplems (allow => alltraffic-0.0.0.0)
        (ps) check the database using PostBird ('test-connection')

#aws ElasticBean

        we create container
            1-using the terminal:
                set the app name (udagram-api) node version 16.17.0 (in package.json) & region us-east-1 (using eb init)
                    "eb init udagram-api --platform node.js --region us-east-1"
                then we create a sample environment 
                    "eb create udagram-env --sample --region us-east-1"
                    "eb list" we should see "*udagram-env" 
                    now we have our working eb env container.
                    ----check the health using "eb use udagram-env" then "eb health" should be okay if not run "eb logs" 
            2- using the console:
                from our AWS console we create elasticbean app name it "udagram-api" platform "node.js" branch "node.js v16" application code "sample application" then hit "Create application"
                then create a app-env name it "udagram-env" platform "node.js" branch "node.js v16" application code "sample application" then hit "Create environment"
                ----after completion check the health of the env should be green ----

            EB-env URL ex = http://udagram-env.eba-3gzmxwqn.us-east-1.elasticbeanstalk.com

#aws s3 
        aws s3 bucket for hosting a static web site
            using the cmd line:
                after installing and configuring AWS shown above enter "aws s3 mb s3://udagram-bucket-926 --region us-east-1 "
            using the console:
                got to aws s3 buckets create bucket
                trigger option "ACls enabled" -- uncheck "Block all public access" and check i acknowledge ... . then "Create bucket"
                open the newly created bucket "udagram-bucket-926"
                go to properties enable "Static website hosting" writing "index.html" as Index document
                then go to "permissions" policy tab click edit add these:
                                                                         {
                                                                            "Version": "2012-10-17",
                                                                            "Statement": [
                                                                                {
                                                                                    "Sid": "PublicReadGetObject",
                                                                                    "Effect": "Allow",
                                                                                    "Principal": "*",
                                                                                    "Action": [
                                                                                        "s3:GetObject"
                                                                                    ],
                                                                                    "Resource": [
                                                                                        "arn:aws:s3:::(your bucket name)/*"
                                                                                    ]
                                                                                }
                                                                            ]
                                                                        }
                 change (your bucket name)(removing the brackets of course) with your bucket name and click "save changes"
                 this makes our bucket accessable to public
                 then go to CORS in the bottom of th page add these:
                                                                [
                                                                    {
                                                                        "AllowedHeaders": [
                                                                            "*"
                                                                        ],
                                                                        "AllowedMethods": [
                                                                        "GET",
                                                                            "PUT",
                                                                            "POST",
                                                                            "DELETE"
                                                                        ],
                                                                        "AllowedOrigins": [
                                                                            "*"
                                                                        ],
                                                                        "ExposeHeaders": [
                                                                            "x-amz-server-side-encryption",
                                                                            "x-amz-request-id",
                                                                            "x-amz-id-2"
                                                                        ],
                                                                        "MaxAgeSeconds": 3000
                                                                    }
                                                                ]
                this takes care of CORS on our bucket communicating to the api.

                s3 URL ex= http://udagram-bucket-926.s3-website-us-east-1.amazonaws.com
