# Image-Compression-using-Single-Value-Decomposition

The basic concept is to represent an image with size m by n as a two-dimentional m by n matrix. SVD is then applied to this matrix to obtain the U, S, and V matrices. S is a diagnoal m by n matrix whose number of non-zero elements on the diagnoal determine the rank of the original matrix. The fundamental concept of the SVD-based image compression scheme is to use a smaller number of rank to approximate the original matrix. This operation can be expressed in the following equations (from the lecture notes): 
Original Image: 	A = USVH,
			where U is m by n,
			      V is n by n,
			      and S = 
			      diag(r1, r2,...,rk,0,...,0)

Re-constructed Image:  A1 = US1VH,
			where U is m by k1,
			      V is k1 by n,
			      and S1 = 
			      diag(r1, r2,...,rk1)

			and
		        ||A - A1||2 = rk1 + 1


Quality of the compression is measured in term of Peak Signal to Noise Ratio , which is defined as: 

			PSNR = 10 log10((max. range)2 / Root Mean Square Error )	

If the original image is square, then the matrix representation would require a m2 storage space. Applying SVD on the original matrix and use only k singular values results in a 2mk + k staorage space. In order to achieve the goal of compression, the k used for the re-constructed image would have to be smaller than m2 / (1+2m). In the case of "Lena", the rank used has to be smaller than 255. 


Splitting Image into Smaller Blocks and Adaptive Rank Selection
Instead of compressing the whole image at once, all popular image compression techniques work on the sub-block of original image. The purpose of this practice is toexploit the uneven complexity of the original image. If a portion of the image is simple, then only a smaller of singular values needs to be used to achieve satisfactory approximation. On the other hand, if the image is cimplex, then more singular values would have to be used in order to maintain the image quality. A simple way to adaptively select the appropriate ranks to be used in each sub-block is to specify the percentage of the sum of the singular values insteads of a fixed number. This scheme is derived from the observation of the distribution of the singular values in each sub-block. When a sub-block contains complex image, its singular values are more "spreaded out" than the one that contain a simple image. Thus if we specify a centain percentage of the sum to be used, a higer number of ranks will be used for a complex sub-block than the one with simple image. This scheme can be expreseed as: 

Rank k1 used for each sub-block:   
					(r1+r2+...+rk1)
		Specified Percentage =------------------
					(r1+r2+...+rk)

To see the effect of this rank selection scheme, we can observe the fact that the ranks used for each sub-block in both cases are indeed correlated with the complexity of the image. The ranks used for the 64-by-64 cases is listed below:

	Ranks used for percentage = 7.750000e+01:


	RUsed =

     1     5     2     4     3     1     3     8
     2     7    10     8     4     5     6     7
     1     5    12    10     6     5     7     2
     1     6    11    13     5     8     5     1
     2    12    15    11     4     8     3     2
     2    14    13     9     5     8     1     1
     3    14    13    11     2     6     3     4
     3    15    15     6     1     3     4     7




	Ranks used for percentage = 70:

	RUsed =

     1     1     1     2     2     1     2     5
     1     2     5     4     3     3     4     5
     1     3     7     6     3     3     5     1
     1     3     8     9     3     5     3     1
     1     9    12     8     3     5     2     1
     1    10    10     5     3     5     1     1
     2    11    10     6     1     4     2     3
     2    10    12     4     1     2     3     5


Another interesting observation that can be made from Fig. 3 and 6 is that even with the same percentage used, the image quality with the 8-by-8 block-szie is much better than the image with the 64-by-64 block-size. This effect can be verified from the following table which shows the average ranks used and the average percenatge of the ranks used for both cases. Notices that in the 8-by-8 case, a higher percentage of ranks were used. 

64-by-64 block-size:
===>    Percentage of Singular     |    Average Ranks   | Avg. Percentage of
        Values Sum                         Used           Ranks Used.
                85                      10.500000          0.164062
                81.25                   7.968750           0.124512
                77.5                    6.156250           0.096191
                73.75                   4.937500           0.077148
                70                      4.046875           0.063232


8-by-8 block-size:
===>    Percentage of Singular     |    Average Ranks   | Avg. Percentage of
        Values Sum                         Used           Ranks Used.
                85                      1.540039           0.192505     
                81.25                   1.371582           0.171448     
                77.5                    1.256104           0.157013     
                73.75                   1.176514           0.147064
                70                      1.115234           0.139404    




Rank-One Update
A further reduction in the ranks used can be achieved by subtracting the mean of the original image before perfromaning the SVD. The mean is then added back to the SVD construction to obtain the the re-constructed image. This process can be expressed in the following eqaution: 

Original Image: 	A - mean = USVH,
			where U is m by n,
			      V is n by n,
			      and S = 
			      diag(r1, r2,...,rk,0,...,0)

Re-constructed Image:  A1 = US1VH + mean,
			where U is m by k1,
			      V is k1 by n,
			      and S1 = 
			      diag(r1, r2,...,rk1)



This process is called the "Rank-1 Update", it essentially compacts the distrubution of the singular values. The next effect of this process is that fewer ranks will be needed to achieve a satisfactory compression. The following table shows the average ranks and the percentage of ranks used for the 8-by-8 block-size compression with rank-1 update. Notices that when the percentage of singular values sum is 73.75 and 70, only 1 rank is used for all sub-blocks. This is the highest ratio of compression that can be achieved. The results of applying rank-1 update with the same parameters used in fig. 5 is shown in Figure 7 and 8.


8-by-8 block-size:
===>    Percentage of Singular     |    Average Ranks   | Avg. Percentage of
        Values Sum                         Used           Ranks Used.
                85                      2.311279           0.288910
                81.25                   1.371582           0.171448                     
                77.5                    1.015137           0.126892     
                73.75                   1.000000           0.125000     
                70                      1.000000           0.125000

m = 0.3884       <---- mean 


To run the code
1. Run imgsvd.m in MATLAB
2. Select image
3. Select target path
