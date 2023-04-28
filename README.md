# Sample Cumulus Library Database

This repository holds a sample database that can be fed into the
[Cumulus Library](https://github.com/smart-on-fhir/cumulus-library-core/).

## How do I use this?

First, check out this repository so that you have local copies of the files to work with.

These next few sections will walk you through creating a new S3 bucket where we will put sample
data and also an Athena database & workgroup to query that data.

### Create CloudFormation stack

1. Log into AWS.
1. Go to the `CloudFormation` service.
1. Click `Stacks` on the left.
1. Click `Create Stack` (choose `With new resources` from dropdown).
1. On the new screen, click `Upload a template file`.
1. Click the `Choose file` button.
1. Navigate to the `aws-template.yaml` template file in this repo.
1. Click Next.
1. Enter a stack name (can be anything).
1. You can edit the parameters, but it isn't necessary.
1. Click Next.
1. Click Next.
1. Scroll to the bottom, click `I acknowledge that AWS CloudFormation might create IAM resources.`
1. Click Submit.
1. Watch it create the stack, this might take a couple of minutes.

### Upload sample data
1. When it's done, switch to the Resources tab and click on the S3Bucket link.
1. This will bring you to the newly created bucket.
1. Click the `Upload` button.
1. Click the `Add folder` button.
1. Navigate to the `data` folder in this repo and upload it itself (don't select any files inside it, just upload the whole `data` folder).
1. You should see a few files listed, with a Folder column value of `data/condition/` or `data/encounter/` etc.
1. Click the `Upload` button.
1. When it's done, you should be able to see files in the bucket, under the `data/` folder.

### Crawl sample data

1. Switch to the `AWS Glue` service.
1. Open up the `Data Catalog` section on the left sidebar and click on `Crawlers`.
1. Select the crawler you created (will be named after the database you created above).
1. Click the `Run` button.
1. Wait for it to finish (a `Succeeded` last run state). You should see `6 created` in the `Table changes` column.

### Confirm data made it to Athena

1. Switch to the `Athena` service.
1. Click on `Query editor`
1. Click on the `Workgroup` dropdown on the upper right.
1. Select the name of the database you used for the stack - if you didn't change it, it's cumulus_library_sample_db.
1. A dialog will pop up, click `Acknowledge`
1. On the left sidebar, click the database dropdown and choose the database name you entered above (likely `cumulus_library_sample_db`)
1. You should see that it has 6 tables (`condition`, etc).

You are now ready to follow the further instructions in the
[Cumulus Library](https://github.com/smart-on-fhir/cumulus-library-core/)!

## How was this database generated?

We started with a [1000-patient Synthea dataset](https://github.com/smart-on-fhir/sample-bulk-fhir-datasets),
and ran it through [Cumulus](https://smarthealthit.org/cumulus-a-universal-sidecar-for-a-smart-learning-healthcare-system/)
ETL for de-identification and compression.
