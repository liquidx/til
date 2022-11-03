# Authentication on Google Cloud Storage 

To authenticate from a workstation to be able to write to Google Cloud Storage from the command-line using Node JS, you need to first createa key for a service account.

https://console.cloud.google.com/iam-admin/serviceaccounts/

Once you do that, you'll get a JSON file, then you can load it like this:

```
const getCloudStorageClient = () => {
  return new Storage({
    projectId: PROJECT_ID,
    keyFilename: '/credentials-google.json',
  });
};

const getCloudStorageBucket = () => {
  return getCloudStorageClient().bucket(CLOUD_STORAGE_BUCKET);
};
```
