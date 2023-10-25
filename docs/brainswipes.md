# BrainSwipes
[BrainSwipes](https://brainswipes.us) is a web application that brings the swipe right, swipe left paradigm to quality assurance.

The DCAN Labs handle many neuroimaging datasets, and BrainSwipes helps centralize and scale the process of ensureing quality data.

BrainSwipes is built off of the open source [Swipes For Science](https://docs.BrainSwipes.org/) platform.


## BrainSwipes Architecture
BrainSwipes is hosted on [AWS Lightsail](https://lightsail.aws.amazon.com/ls/webapp/home/instances?#), uses a [Firebase](https://console.firebase.google.com/project/brainswipes/overview) database to track data on swipes, and S3 storage on UMN's [Minnesota Supercomputing Institute (MSI)](https://www.msi.umn.edu/).

![Swipes app flow](https://raw.githubusercontent.com/DCAN-Labs/BrainSwipes/main/src/assets/swipes-flow.png "Standard flow for the swipes app")

### AWS resources
Account information for the DCAN Labs AWS account is available in [Box](https://umn.app.box.com/file/1112270181881).

Brainswipes uses an AWS Lightsail instance to host the live web application. Refer to Lightsail documentation for maintaining this server.

This site is hosted on http://brainswipes.us this domain is managed on AWS Route 53.
Access AWS route 53 through the DCAN Labs AWS account to manage this domain.

### Firebase
Brainswipes uses several Firebase tools. The instance for this project is [here](https://console.firebase.google.com/project/brainswipes/overview). Log in with the DCAN Labs google account to gain access and set new users.

BrainSwipes uses a Realtime Database for storing data and Firebase Authentication for account management. Refer to Firebase documentation for tasks involved in managing these resources.

### MSI S3
Images are stored on MSI Tier 2 storage. 
The application uses the [JavaScript AWS SDK](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/welcome.html) to pull images from these buckets.
The application uses a service account that needs **read only** access to buckets with images to be used in the app. It is recommended to use the method [outlined here](https://www.msi.umn.edu/support/faq/how-do-i-use-s3-buckets-share-data-tier-2-storage-other-users) to update access.
The BrainSwipes service account's UID is **77766**

Here is an example bucket policy for the bucket `swipes_test` that gives full access to someone managing the data (uid 77291) and read only access to the service account.
```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {"AWS": [ "arn:aws:iam:::user/uid=77291" ]},
        "Action": [ "s3:*" ],
        "Resource": ["arn:aws:s3:::swipes_test/*", "arn:aws:s3:::swipes_test"]
      },
      {
        "Effect": "Allow",
        "Principal": {"AWS": [ "arn:aws:iam:::user/uid=77766" ]},
        "Action": [ "s3:GetObject", "s3:GetObjectVersion" ],
        "Resource": ["arn:aws:s3:::swipes_test/*"]
      },
      {
        "Effect": "Allow",
        "Principal": {"AWS": [ "arn:aws:iam:::user/uid=77766" ]},
        "Action": [ "s3:ListBucket" ],
        "Resource": ["arn:aws:s3:::swipes_test"]
      }
    ]
}

```

#### List of S3 buckets used by BrainSwipes

| S3 Bucket                | study       | notes                                              |
|--------------------------|-------------|----------------------------------------------------|
| s3://brainswipes         | BCP v 1.0.0 | many tutorial images are pulled from here          |
| s3://brainswipes-backups |      -      | Bucket that the script firebaseBackup.js pushes to |
| s3://swipes-abcd         | ABCC Year 2 |                                                    |
| s3://swipes-elabe        | ELABE       |                                                    |
| s3://swipesABIDE1        | ABIDE 1     | many tutorial images are pulled from here          |
| s3://swipesABIDE2        | ABIDE 2     |                                                    |
| s3://swipesBCP           | BCP v 1.0.1 |                                                    |
| s3://hbcd-main-study     | HBCD        | HBCD main study, T1/T2 and Diffusion               |
| s3://midb-hbcd-pilot-pr  | HBCD        | HBCD pilot data, T1/T2 and Diffusion               |
| s3://swipes_test         | Test data   | Some tutorial images live here                     |

## Development

### Setup for Development
All development should be done on a branch on your local machine.
Install node v17.2.0 and npm v8.1.4 (version compatibility is critical). It is recommended to use [nvm](https://nodejs.org/en/download/package-manager) for installation.
```
git clone git@github.com:DCAN-Labs/BrainSwipes.git
cd BrainSwipes
npm install
npm run dev
```
​​Your application is running here: http://localhost:8080  

The database uses [firebase appcheck](https://firebase.google.com/docs/app-check/web/recaptcha-provider) to protect the app. You may need to add a token or temporarily enable localhost.

### Deployment
To build the files for use with a http server, use the command `npm run build` in the BrainSwipes folder on your local machine. These files will appear in the `/dist` directory. Use `git checkout lightsail` and replace the files in `/public` with the new files from `/dist`. To test the build locally use node express.js in the directory with the file express.js. Your app will be running on http://localhost:3000

The server running the live version of this is an instance of Amazon Linux 2 on AWS Lightsail.
Push the built files to the origin. On the lightsail instance, open the console.  `cd BrainSwipes` and `git pull`.
To update the live version of the app run `pm2 restart express.js`.
If you modify the `express.js` the pm2 restart command will not update this file. Instead run `pm2 kill` and `pm2 start express.js`


## Management

## Adding new images
Use the tools/es2sfs_img_converter.py script to format images for use with brainswipes

[Adding new studies to BrainSwipes](https://docs.google.com/document/d/1apA6hc4Oj33BoP_t7oacL-x3vDvglYWSt0CdlycPeuM/edit?usp=sharing)

