# First Dance with PySpark                   
- Buckets and clusters conctruction in Google Cloud Platform                       
- VM SSH connection and browser configuration                    
- 'Local repository' constructed in Hadoop, upload/download files to/from the bucket                   
- Coding performance in PySpark using jupyter                                        
                              
## Buckets and clusters construction in Google Cloud Platform                    
This part can be finished in PowerShell (Windows System) or Google Cloud SDK Shell.                  
```python
# Create a bucket, for example creating a bucket named chuwei:                    
gsutil mb gs://chuwei/          
# cluster construction and SSH configuration
gcloud dataproc clusters create chuweizhou --bucket chuwei --subnet default --zone europe-west3-a --master-machine-type n1-standard-4 --master-boot-disk-size 500 --num-workers 2 --worker-machine-type n1-standard-4 --worker-boot-disk-size 500 --image-version 1.3-deb9 --project curious-ocean-228920 --initialization-actions gs://dataproc-initialization-actions/jupyter/jupyter.sh
```
Sometimes if the zone we have selected has no enough space, we can switch to us-east1-b, europe-west1-b, europe-west1-c, europe-west3-b, etc.                 
```python
gcloud dataproc clusters create chuweizhou --bucket chuwei --subnet default --zone europe-west3-b --master-machine-type n1-standard-4 --master-boot-disk-size 500 --num-workers 2 --worker-machine-type n1-standard-4 --worker-boot-disk-size 500 --image-version 1.3-deb9 --project curious-ocean-228920 --initialization-actions gs://dataproc-initialization-actions/jupyter/jupyter.sh
```                   
![VM SSH](https://github.com/zhouchw5/Course_study_uk.github.io/blob/First-Dance-with-PySpark/VM%20SSH.png)
## VM SSH connection and browser configuration                   
Actually we have finished some parts of VM SSH connection when creating the cluster above. But before configuring our browser so that we can get access to different ports based on our browser, like http://chuweizhou-m:8123 for jupyter, we should execute the statement in the Power Shell or Google Cloud SDK Shell shown as below.                                    
```python
gcloud compute ssh chuweizhou-m --project=curious-ocean-228920 --zone=europe-west3-a -- -D 1080 -N
```
Other options like (but the zone should be consistent with that you use for cluster creation)                   
```python     
gcloud compute ssh chuweizhou-m --project=curious-ocean-228920 --zone=us-east1-b -- -D 1080 -N
```             
To configure our browser using the cmd operations:                     
```python
cd C:\Program Files (x86)\Google\Chrome\Application
chrome.exe --proxy-server="socks5://localhost:1080" --user-data-dir="%Temp%\chuweizhou-m" http://chuweizhou-m:8088
```
![hadoop](https://github.com/zhouchw5/Course_study_uk.github.io/blob/First-Dance-with-PySpark/hadoop.png)                  
![jupyter](https://github.com/zhouchw5/Course_study_uk.github.io/blob/First-Dance-with-PySpark/google%20jupyter.png)               
           
## 'Local repository' constructed in Hadoop, upload/download files to/from the bucket                  
This part we should utilize the VM SSH terminal.                                
![SSH Terminal](https://github.com/zhouchw5/Course_study_uk.github.io/blob/First-Dance-with-PySpark/VM%20SSH%20Terminal.png)                                        
                     
For example, we need to create a directory dblp in Hadoop and download two files dblp.xml.gz and author-large.txt, both of which would be got attaced to the bucket.                                       
```python
mkdir dblp
cd dblp
wget http://dblp.uni-trier.de/xml/dblp.xml.gz
wget http://webdam.inria.fr/Jorge/files/author-large.txt
gunzip -k dblp.xml.gz
cd ..          
# Copy the dblp directory from your local file system to HDFS: 
hadoop fs -put dblp/ /dblp
```
To list files:                 
```python
hadoop fs -ls /
```
To check the content of dblp directory in my local file system:             
```python
ls -al dblp
```

Yours,                  
Chuwei Zhou        
2019.2.21                 
