---
title: Assignment 1
subtitle: Computer performance, reliability, and scalability calculation
author: Emily Phillips
---

## 1.2 

Bytes Conversion

https://stackoverflow.com/questions/2365100/converting-bytes-to-megabytes

Bytes --> MB = Byte size / (1024*1024)
Bytes --> GB = Byte size / (1024 * 1024 * 1024)
Bytes --> TB = Byte size / (1024 * 1024 * 1024 * 1024)

#### a. Data Sizes

| Data Item                                  | Size per Item | 
|--------------------------------------------|--------------:|
| 128 character message.                     | 128 Bytes     |
Total size (in Bytes) = ((Number of bits used to encode a single character) * Number of characters))/8
-	ASCII encoding: 8 bits to encode each character
Total size (in Bytes) = (8*128)/8 = 128 bytes


| 1024x768 PNG image                         | 0.405 MB |
https://4nsi.com/how-do-i-calculate-the-file-size-for-a-digital-image/
-	https://superuser.com/questions/759026/how-big-is-an-800x600-image-file-in-png
- https://toolstud.io/photo/filesize.php?imagewidth=1024&imageheight=768
- Full PNG has a file size of 402KB
- Example: To calculate how much data would be needed by uncompressed raw image data you have to do this simple thing: raw data size = image width * image heigth * (bits per pixel / 8). Then just divide raw data size by your PNG's file size by and you have the estimated compression ratio (not exact value because of the headers, etc.). For example 640x480x32 image would need 640 * 480 * (32 / 8) which is 1 273 800 bytes. Now lets assume your PNG has 200kB. You divide (200 * 1024) / 1273800. That gives you a compression ratio of about 0.16.
- Raw data size = 1024 * 768 * (24-bit/8)
= 2359296 bytes
= 420 KB --> divide (420 * 1024) / 2359296 = .18 compression ratio
= 2359296 bytes * 0.18 compression ratio = 424673 bytes
= 424673 bytes / (1024 * 1024) = .405 MB


| 1024x768 RAW image                         |1.125 MB | 
http://preservationtutorial.library.cornell.edu/intro/intro-06.html#:~:text=FILE%20SIZE%20is%20calculated%20by,divide%20this%20figure%20by%208.
- I am going to assume a 12-bit image for bit depth
File Size = (pixel dimensions x bit depth) / 8
= ((1024 * 768)*12-bit)/8
= 1179648
= 1,179,648/1024 = 1152/1024 = 1.125 MB

| HD (1080p) HEVC Video (15 minutes)         | 160.2 MB|
- https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding
-	Formula = 24 (bit) * 1920 * 1080 (pixels) * 30 (frames per second) * 900 seconds (~15 mintutes) = 1.344e12 bytes
-	1.344e12 / 8192 = 164,025,000 / 1024 = 160,180MB
-	160,180 MB / 1000 (compression ratio) = 160.18 MB 

| HD (1080p) Uncompressed Video (15 minutes) |160,180MB = 156 GB|
- https://en.wikipedia.org/wiki/High_Efficiency_Video_Coding
- Not dividing by 1000:1 compression ratio since the video is uncompressed
= 160,180MB  (160.2*1000)

| 4K UHD HEVC Video (15 minutes)             |641 MB|
-	https://www.circlehd.com/blog/how-to-calculate-video-file-size
-	https://www.lifewire.com/4k-resolution-overview-and-perspective-1846842
-	Pixels = 3840 x 2160 pixel
-	Formula = 24 (bit) * 3840 * 2160 (pixels) * 30 frames per second * 900 seconds = 5.37e12
-	5.37e12 / 8192 =656,100,000 / 1024 = 640,723 MB
-	640,723 MB / 1000 = 641 MB 

| 4k UHD Uncompressed Video (15 minutes)     |641,000 MB/1024 = 625 GB|
- Multiply HEVC amount by 1000 to mitigate ratio
- 641 MB * 1000 = 641,000 MB

| Human Genome (Uncompressed)                | 6 GB |
-	https://www.biostars.org/p/5514/
-	6e9 bits uncompressed  6000000000 bytes  6GB


#### b. Scaling

- From https://drivesaversdatarecovery.com/is-bigger-better-how-to-choose-the-right-hard-drive-sizes/,
it noted that about 10-15% of a hard drive's storage needs to be saved for other processes for it to function.
- Therefore, for deciding the amount of storage required, I will be increasing the amount by 10-15% more than the calculated size of the scenarios to account for this extra percentage of space that is needed in the hard drive to keep it running and able. 
- I will also multiply the amount of storage required by three since a HDFS storage system is being used, and divide by ten to find the final amount of hard drives needed since we assuming that each hard drive used by us can store 10 TB. 

|                                           | Size     | # HD | 
|-------------------------------------------|---------:|-----:|
| Daily Twitter Tweets (Uncompressed)       |239GB     |500GB * 3 = 1500GB or 1.5TB (1 HD)|
-	https://contingencycoder.wordpress.com/2013/04/10/how-much-space-do-all-those-tweets-take-up/
-	Bytes for each tweet = 4 * 128 characters + 1 (length of characters in the database) = 513 bytes
-	 513 bytes * 500 million tweets a day = 2.565e11
o	GB = 238.9 GB a day
o	TB = 0.23
-	Need 10-15% of a hard drive’s storage to function

| Daily Twitter Tweets (Snappy Compressed)  |149GB    |256GB * 3 = 768GB or .75TB (1 HD)|
-	https://www.infoq.com/news/2011/04/Snappy/#:~:text=The%20high%20compression%20speed%20is,other%20already%2Dcompressed%20data%E2%80%9D.
-	For compressed files, we need to remember to multiply by the compression ratio on the total file size
-	Snappy compresses at about 250 MB/sec
-	Compression ratio of about 1.5-1.7x for plain text  tweets are usually in UTF-8 encoding
-	Uncompressed file size ~ 238.9 GB  aday
-	Compressed file size = 238.9GB /1.6 ratio = 149GB a day
- 149 GB a day = .15 TB a day

| Daily Instagram Photos                    |29 TB   |35 TB * 3 = 105 TB / 10 = (10.5 HDs)|
-	100 million * .75 = 75 million photos a day
-	1024 * 768 PNG photos
-	https://superuser.com/questions/759026/how-big-is-an-800x600-image-file-in-png
- Part a --> size of one 1024x768 PNG image = 0.405MB
-	File size in MB = .405 MB for one image * 75000000 = 30375000 MB
- 30375000 MB / (1024*1024) = 29 TB

| Daily YouTube Videos                      |440 TB   |500 * 3 = 1500 TB / 10 = (150 HDs)|
-	500 hours of video uploaded to YouTube every minute 
-	24 hours in  a day * 60 minutes = 1440 minutes
-	500 * 1440 = 720,000 hours of video uploaded every day * 3600 =  2.592e9 seconds
- For simplicity, assume all videos are HD quality encoded using HEVC at 30 frames per second.
-	Formula = 24 (bit) * 1920 * 1080 (pixels) * 30 (frames per second) * 2.592e9 seconds = 
-	3.89e18 / 8192 = 4.7e14 / 1024 = 4.6e11 MB
-	4.6e11 / 1000 (compression ratio) = 461320312 MB
- MB --> TB = 439 TB

| Yearly Twitter Tweets (Uncompressed)      |85 TB    |100 TB of storage * 3 = 300 TB / 10 = 30 HDs|
- 239 GB  a day * 365 days = 87235 GB a year
- 87235/1024 = 85 TB

| Yearly Twitter Tweets (Snappy Compressed) |53 TB    |70 of storage * 3 = 210 TB / 10 TB =  21 HDs|
- 149 GB a day * 365 days = 54385 GB a year
- 53 TB a year

| Yearly Instagram Photos                  |10,585 TB =10 PB|10 PB * 3 = 30 PB * 1024 = 30,720TB / 10 = 3,072 HDs|
- 29 TB  a day * 365 days = 10,585 TB a year
- 10585/1024 ~ 10 PB a year

| Yearly YouTube Videos         |160,600TB = 157 PB|200 PB * 1024 = 204,800 TB *3 = 614,400 / 10 TB = 61,440 HDs)|
- 440TB a day * 365 days = 160,600 TB a year
- 160,600/1024 = 157 PB a year

#### c. Reliability
Since we are using a 10TB for estimating the number of hard sizes, I am going to reference the Seagate 10TB drive for its statistics since it is the only 10TB Backblaze hard drive mentioned in the data.
-AFR = 2.26% = 2.26/100 = 0.0226
- I will multiply the above AFR by the number of hard drives required for the above scaling examples to determine how many hard drive failures occurred, out of the total amount.

|                                    | # HD   | # Failures |
|------------------------------------|-------:|-----------:|
| Twitter Tweets (Uncompressed)      | 30     |30 * 2.26% = 30 * 0.0226 = 0.68|
| Twitter Tweets (Snappy Compressed) | 21     |21 * 0.0226 = .47|
| Instagram Photos                   | 3,072  |3,072 * 0.0226 = 69|
| YouTube Videos                     | 61,440 |61,440 * 0.0226 = 1,338|

#### d. Latency

Los Angeles to Amsterdam: https://wondernetwork.com/pings/Los+Angeles (average ping time)

Low Earth Orbit Satellite & Geostationary Satellite: https://www.omniaccess.com/leo/

Earth to the Moon & Earth to Mars: https://www.spaceacademy.net.au/spacelink/commdly.htm

|                           | One Way Latency      |
|---------------------------|---------------------:|
| Los Angeles to Amsterdam  | 133.47 ms            |
| Low Earth Orbit Satellite | 40 ms                |
| Geostationary Satellite   | 600 ms               |
| Earth to the Moon         | 1.3 seconds * 1000 = 1300 ms|
| Earth to Mars             | Average: (21+3)/2 = 12 minutes| 
