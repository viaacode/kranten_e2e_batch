# kranten e2e batch
Process that starts verifying if newspapers were ingested correctly.

## Description
This process uses the batch (EE) module to parallel process all records within the kranten database and checks the MediaHaven database if the files were ingested correctly.

If an asset is deleted, the status in the Kranten database will be 'DELETED'. `in_progress` will be copied over to the Kranten database and will be rechecked later on.
`failed` will set the Kranten status to 'failed' and get the comment from the last 'NOK' event on that asset.

Any other statuses will be copied.

## Usage
The process can be triggered after all files were fully processed by MediaHaven. This can be checked by looking at the count of in_progress sips for this specific CP.

Triggering is done by sending a HTTP GET message to `<ESB_SERVER>:8079/start?batch_id=kERF_003` making sure the batch_id is filled in correctly. This will initiate the process and return a response immediately. The process will take a while to run, depending on the size of the batch.
