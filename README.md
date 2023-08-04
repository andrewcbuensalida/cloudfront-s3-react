https://www.youtube.com/watch?v=mls8tiiI3uc&t=15s

create a react app with
  npx create-react-app cloudfront-s3-react
change some of it then start with
  npm start
build with
  npm run build

create an s3 bucket with bucket name cloudfront.anhonestobserver.com and another with www.cloudfront.anhonestobserver.com
upload files in the build folder to the www one

uncheck block all public access in permissions tab for www

paste this into the bucket policy
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
                "arn:aws:s3:::www.cloudfront.anhonestobserver.com/*"
            ]
        }
    ]
}

test that bucket is public by going to the url of the index.html. You should see a blank page but with the title of it on the tab.

enable static website hosting by going to properties tab
now you can go to http://www.cloudfront.anhonestobserver.com.s3-website-us-west-1.amazonaws.com and it will work.

redirect cloudfront.anhonestobserver.com to the www one by going to properties, enable static website hosting, hosting type = redirect, host name should be www.cloudfront.anhonestobserver.com, http protocol = https. If not using cloudfront, can't get around just http if you're using s3 for static website hosting.

go to cloudfront. create distribution. origin name = the s3 bucket www but it will suggest to use the endpoint instead so choose the endpoint. viewer protocol property = redirect http to https. check enable security protections. alternate domain name (cname) = www.cloudfront.anhonestobserver.com. Then request a certificate. 
It will bring you to ACM. Fully qualified domain names = www.cloudfront.anhonestobserver.com and cloudfront.anhonestobserver.com.
When it's pending validation, copy the CNAME name to the record name when creating a record in route53. cname value to value. After a few minutes, it should show success status in ACM. Now in cloudfront you can choose that certificate. 
Create another distribution but for the non-www one. Use the same certificate. use the non-www for alternate domain name cname. 
open route53. create A record pointing to cloudfront. check alias. route traffic to alias to cloudfront you just created. 
Basically route53 A records point to cloudfront which points to s3. 






==================================================================================
# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can't go back!**

If you aren't satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you're on your own.

You don't have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn't feel obligated to use this feature. However we understand that this tool wouldn't be useful if you couldn't customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
