
Create an Amazon S3 bucket to host static content using the Amazon S3 console. For more information about Amazon S3, see Introduction to Amazon S3.

    Open the Amazon S3 console at https://console.aws.amazon.com/s3/.
    From the console dashboard, choose Create bucket.

s3-create-bucket-1

    Enter a Bucket name for your bucket, type a unique DNS-compliant name for your new bucket. Follow these naming guidelines:

    The name must be unique across all existing bucket names in Amazon S3.
    The name must not contain uppercase characters.
    The name must start with a lowercase letter or number.
    The name must be between 3 and 63 characters long.

    Choose an AWS Region where you want the bucket to reside. Choose a Region close to you to minimize latency and costs, or to address regulatory requirements. Note that for this example we will accept the default settings and this bucket is secure by default. Consider enabling additional security options such as logging and encryption, the S3 documentation has additional information such as Protecting Data in Amazon S3.
    Accept default value for Block all public access as CloudFront will serve the content for you from S3.
    Enable bucket versioning, to keep multiple versions of an object so you can recover an object if you unintentionally modify or delete it.

s3-create-bucket-2

    Click Create bucket.

    Create a simple index.html file, you can create by coping the following text into your favourite text editor.

<!DOCTYPE html>
<html>
  <head>
    <title>Example</title>
  </head>
  <body>
    <h1>Example Heading</h1>
    <p>Example paragraph.</p>
  </body>
</html>

    Open the Amazon S3 console at https://console.aws.amazon.com/s3/.
    In the console click the name of your bucket you just created.
    Click the Upload button.

s3-upload-object-1

    Click the Add files button, select your index.html file, then click the Upload button.

s3-upload-object-2

    Your index.html file should now appear in the list.

s3-upload-object-3

Using the AWS Management Console, we will create a CloudFront distribution, and configure it to serve the S3 bucket we previously created.

    Open the Amazon CloudFront console at https://console.aws.amazon.com/cloudfront/home.
    From the console dashboard, click Create Distribution.

cloudfront-create

    Click Get Started in the Web section.

cloudfront-getstarted

    Specify the following settings for the distribution:

    In the Origin Domain Name field Select the S3 bucket you created previously.
    In Restrict Bucket Access click the Yes radio then click Create a New Identity.
    Click the Yes, Update Bucket Policy Button.

cloudfront-create-distribution-s3-1

    Scroll down to the Distribution Settings section, in the Default Root Object field enter index.html
    Click Create Distribution.
    To return to the main CloudFront page click Distributions from the left navigation menu.
    For more information on the other configuration options, see Values That You Specify When You Create or Update a Web Distribution in the CloudFront documentation.

    After CloudFront creates your distribution which may take approximately 10 minutes, the value of the Status column for your distribution will change from In Progress to Deployed.

cloudfront-deployed

    When your distribution is deployed, confirm that you can access your content using your new CloudFront Domain Name which you can see in the console. Copy the Domain Name into a web browser to test.

cloudfront-test-s3

For more information, see Testing a Web Distribution in the CloudFront documentation.

    You now have content in a private S3 bucket, that only CloudFront has secure access to. CloudFront then serves the requests, effectively becoming a secure, reliable static hosting service with additional features available such as custom certificates and alternate domain names.

For more information on configuring CloudFront, see Viewing and Updating CloudFront Distributions in the CloudFront documentation.
