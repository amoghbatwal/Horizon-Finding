# CSCI-B 551 Elements of Artificial Intelligence

### Horizon-Finding

### Problem Statement
1. Perhaps the simplest approach would be to use the Bayes Net
2. A better approach would use the Viterbi algorithm to solve for the maximum a posterior estimate
3. A simple way of improving the results further would be to incorporate some human feedback in the process. Assume that a human gives you a single (x,y) point that is known to lie on the ridgeline. Modify your answer to step 2 to incorporate this additional information. Show the detected boundary in green superimposed on the input image in an output ﬁle called output human.jpg. Hint: you can do this by just tweaking the HMM’s probability distributions – no need to change the algorithm itself.

Your program should be run like this:
./horizon.py input_file.jpg row_coord col_coord
where row coord and col coord are the image row and column coordinates of the human-labeled pixel.
Run your code on our sample images (and feel free to try other sample images of your own) and include some examples in your report.

### Implementation
In the first part of the code, the Bayes nets simply picks up the maximum value from the edge strength map for that column. This leads to very poor results of the output. But, that is expected from a Bayes net where there is no intelligence used in selecting the ridge points.

In the second part of the code, we are asked to implement Viterbi algorithm since it’s a good method to find the most likely sequence of the hidden variables (which are basically the points lying on the ridge) on the basis of transition probabilities (data point lying on the ridge in current column given the data point on previous column), the probability of the point lying at current coordinate of the map and the emission probability (probability of data point lying in the current column given the edge strength on that column). We have considered the first column as the normalized values of the edge strength in that column. After that, the Viterbi formula is used to calculate the probability. This process is repeated until it reaches the last column. For transition probability, we are only exploring 10% of the rows (close vicinity) which lie in the next column from the current maximum data point (calculated from Viterbi). This reduces our unnecessary search for ridge points far from the current data point as they definitely would not lie that far. The downside of this approach is it fails to detect horizon when the edge strength of the mountain is low compared to the other strong points which have a high gradient. This is because the initial point in the first column is itself the point which has the highest gradient. This can be resolved if the starting point is chosen appropriately. Also, few points have emission which dominate over better ridge contestants.

In the third part of the code, given the human input coordinates which lie on the ridge, we started our exploration of the ridge forward and backward from that data point. The human coordinate gets initial state distribution probability as 1 since it is definite to lie on a ridge. The same logic is used as Viterbi in part 2. The human input provides a lot of improvement as compared to the second part. It acts as a helping hand to the search even to the faint mountain images. 
