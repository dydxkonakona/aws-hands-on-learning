# Amazon S3

This hands-on will walk you through how I explored deploying a static website on Amazon S3.

## What I did
- Created an S3 bucket
- Turning on the website hosting configuration on the bucket.

### Creating the bucket
I created a bucket on the console and configured its object ownership by enabling ACLs to specify control access to this bucket and its object. I disabled the block public access settings for this bucket because we will use the bucket to host a static website. Bucket versioning is also turned on so that the objects on this bucket will be versioned for updation and prevention of accidental deletion. Lastly I created tags to this bucket to organize buckets and track costs.

