## Offline Deal
### Prepare files for Offline Deal
https://docs.filecoin.io/store/lotus/very-large-files/#maximizing-storage-per-sector

### Generate a unique piece CID
Use the Lotus client to generate a CAR file of the input without importing:
```
lotus client generate-car <inputPath> <outputPath>
```
Use the Lotus client to generate the piece CID:
```
lotus client commP <inputCarFilePath>
```
Use the Lotus client to generate the data CID:
```
lotus client import <inputCarFilePath>
```
### Propose an offline deal
```
lotus client deal --start-epoch <current_epoch+11640> --manual-piece-cid=CID --manual-piece-size=datasize <Data CID> <miner> <price> <duration>
```
The default start epoch is the 49 hours after you publish the deal, so it is current_epoch +5880

We recommend you using 4 days for us prepare import for you, so it should be  current_epoch+(24*4+1)*60*2 = current_epoch+11640

The default start epoch is the 49 hours after you publish the deal, so it is current_epoch +5880

We recommend you using 4 days for us prepare import for you, so it should be  current_epoch+(24*4+1)*60*2 = current_epoch+11640

### Prepare CSV for Offline Sealing

Prepare the https://github.com/nebulaai/trusted-miner/blob/main/import_deal_template.csv with the data.
You can upload a CSV with no deal id first, to enable miner downloading first.The re-upload it again after the downlownding car file completed.
For fields you don't know, please add the '', e.g.  f019104,bafy2bzacebikhvpxget3hz55jno74llgv7nmohu4jdl2r2n6onlynqq7jh3v6,https://www.download.com/test.car,,343612 if you do not have the md5 at the moment.

There are two ways of uploading files:

* One-stage Upload
Complete step  **Generate a unique piece CID**, **Propose an offline deal**,  **Prepare CSV for Offline Sealing**
  * upload the csv with deal_cid 

* Two-stage Upload
  * stage1: Complete step  **Generate a unique piece CID** , **upload the csv with deal_id empty**
  * stage2:  Complete step  **Prepare CSV for Offline Sealing**, re-upload the csv with deal_id


miner_id(mandatory) : miner you want to send the deal

file_source_url(mandatory): remote URL for download car

md5 (optional): md5sum used to verify the integrity of files

start_epoch(mandatory): the epoch of the deal start

deal_cid(mandatory if choose one-stage upload): Deal_id you need for sending, if you decide using two steps upload you can keep this field empty for the first time, and reuploaded for the 2nd time.
