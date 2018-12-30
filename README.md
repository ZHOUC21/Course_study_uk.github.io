# Palindrome, the Longest Mirror Sequence, a Strange Performance VS an Ordinary Performance in Java                   
A sequence of characters can be shown as x = x[0] x[1] ... x[n].                


Suppose that my architect has been limited that I could only focus on the pair i and j with x[i] = x[j].                
When using the dynamic programming, two basic factors we should figure out firstly: the initialized basic states and the 'induction relations'. The initialized basic states considered in my algorithm are the pair i and j with x[i] = x[j], between which there is no another pair m and k(
<a href="https://www.codecogs.com/eqnedit.php?latex=$k\neq&space;m$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?$k\neq&space;m$" title="$k\neq m$" /></a>
) with x[m] = x[k]. It means that when I construct the matrix[i][j] to recognize whether x[i] = x[j] or not, I should assign different values to matrix[i][j] for different cases, x[i] = x[j] or x[i] != x[j].              
               
For each pair of initialized basic states, I would detect the adjacent pair x[p] = x[q] that can wrap up the inner pair x[i] = x[j] and iterate it till no other outer pair can wrap up the current mirror sequence. Centering on different initialized basic state x[i] = x[j], the longest mirror sequences starting from x[start] and ending at x[end] would have different lengths, and the dynamic programming would help me find out the longest one and the corresponding adjacent inner pair, all of these information would be stored in another two matrices. More details can be shown in my following code:                   
```java
public class LongestMirrorSequence {
	private final int previous_start, previous_end;
	
	public LongestMirrorSequence (int start, int end) {
		previous_start = start;
		previous_end = end;
	}
	
	public int previous_start() { return previous_start;}
	public int previous_end() {return previous_end;}
	
	public static void main(String[] args) {
		
		if (((args == null)||(args.length == 0))||(args.length>1)) {
			System.out.println("please input formative one command line argument");
			return;
		}
		
        // initializing the character array, matrix[i][j] (max-length starting in label i and ending in label j) ;
		char[] c = new char[args[0].length()];	
		if (c.length == 1) {
			System.out.println("length of longest mirror sequence: 1");
			System.out.println("corresponding sequence: " + args[0]);
			return;
		}
		// to guarantee the following processes are based on c.length>=2
		
		// initializing the previousLabel matrix previousLabel[i][j], storing the starting point and ending point of the max-length subsequence inside  the the interval i and j; 
		int[][] longestLength = new int[c.length][c.length]; 	
		LongestMirrorSequence[][] previousLabel = new LongestMirrorSequence[c.length][c.length];
		int[][] equality = new int[c.length][c.length];
		// if the interval between the equality pair c[i] and c[j] has no other pair with the same character, equality[i][j] = 1, otherwise equality[i][j] = 2;  
		// for example, ABCA, equality[0][3] = 1; but for ACBFCA, equality[0][5] = 2;
		
		for (int i=0; i<c.length; i++) {
			c[i] = args[0].charAt(i);
			LongestMirrorSequence r = new LongestMirrorSequence(i,i);
			previousLabel[i][i] = r;
			longestLength[i][i] = 1;
		}		
		
		for (int i=0; i<c.length-1; i++) {
			for (int j=i+1; j<c.length; j++) {
				if (c[i]==c[j]) {
					if ((j-i)<=2) {
						equality[i][j] = 1;
					} else {
						boolean containing = false;
						for (int l=i+1; l<j-1; l++) {
							for (int h=l+1; h<j; h++) {
								if(c[l] == c[h]) {
									containing = true;
								}
							}
						}
						if (containing) {
							equality[i][j] = 2;
						}else if(!containing) {
							equality[i][j] = 1;
						}
					}
				}
			}
		}
		
		for (int i=0; i<c.length-1; i++) {
			for (int j=i+1; j<c.length; j++) {
				if (equality[i][j]==1) {
					if(j==i+1) {
						longestLength[i][j] = 2;
						LongestMirrorSequence r1 = new LongestMirrorSequence(i,j);
						previousLabel[i][j] = r1;
						
					}else {
						longestLength[i][j] = 3;
						LongestMirrorSequence r2 = new LongestMirrorSequence(i+1,i+1);
						previousLabel[i][j] = r2;
					}
					if ((i!=0)&&(j!=c.length-1)) {
						for (int a=0; a<i; a++) {
							for (int b=j+1; b<c.length; b++) {
								if (c[a]==c[b]) {
									int max_previous = longestLength[i][j];
									int previous_a = i;
									int previous_b = j;
									for (int a1=a+1;a1<=i; a1++) {
										for (int b1=b-1; b1>=j; b1--) {
											if ((c[a1]==c[b1])&&(longestLength[a1][b1]>=max_previous)) {
												max_previous = longestLength[a1][b1];
												previous_a = a1;
												previous_b = b1;
											}
										}
									}
									if ((max_previous+2) > longestLength[a][b]) {
										longestLength[a][b] = max_previous+2;
										LongestMirrorSequence r3 = new LongestMirrorSequence(previous_a, previous_b);
										previousLabel[a][b] = r3; 
									}
									
								}
							}
						}
					}
				}
			}
		}
		
		
		int maxlongestMirror = 1;
		int longestStart = 0;
		int longestEnd = 0;
		for (int i=0; i<c.length; i++) {
			for (int j=0; j<c.length; j++) {
				if (longestLength[i][j] > maxlongestMirror) {
					maxlongestMirror = longestLength[i][j];
					longestStart = i;
					longestEnd = j;
				}
			}
		}
		System.out.println("length of longest mirror sequence: " + maxlongestMirror);
		
		char[] out = new char[maxlongestMirror];
		int printCount = 0;
		while (printCount<=((maxlongestMirror-1)/2)) {
			out[printCount] = c[longestStart];
			out[out.length-1-printCount] = c[longestEnd];
			int start, end;
			start = previousLabel[longestStart][longestEnd].previous_start();
			end = previousLabel[longestStart][longestEnd].previous_end();
			longestStart = start;
			longestEnd = end;
			printCount++;
		}
		
		System.out.print("corresponding sequence: ");
		for (int i=0; i<out.length; i++) {
			System.out.print( out[i] + " ");
		}
		System.out.println("");
	}
}
```
