# MergeSort                                                                 
Modified by Julia Boettcher 2012, 2013, 2014, 2016 and Konrad Swanepoel 2018, the recursive version of the MergeSort algorithm (which is a divided and conquer algorithm) can be shown as below. It divides the array to be sorted in two sub-arrays and sorts each of them by dividing each into two sub-arrays and iterates this process inductively. Finally the two sorted sub-arrays can be merged into one sorted arrray.                  
                   
The first critical point to proceed is how we can divide the array to be sorted into two sub-arrays. Firstly we divide the array x[i] into two sub-array with the middle index (start+end)/2 in the following way:              
&nbsp; &nbsp; &nbsp; &nbsp; int[] left = new int[middle-start];              
&nbsp; &nbsp; &nbsp; &nbsp; int[] right = new int[end-middle];          
&nbsp; &nbsp; &nbsp; &nbsp; for (int i=start; i < middle; i++) {left[i-start] = x[i];}                            
&nbsp; &nbsp; &nbsp; &nbsp; for (int i=middle; i < end; i++) {right[i-middle] = x[i];}
                      
**Yours,**                
**Chuwei Zhou**                    
**2019.01.02**                                 
