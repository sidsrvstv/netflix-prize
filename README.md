# Netflix Prize 
## Dr. Michael Lamar, Jordan Turley, Zeyang Huang
### Overview

This project proves a machine learning implementation to the [Netflix Prize](https://en.wikipedia.org/wiki/Netflix_Prize), a competition by Netflix from 2006 to 2009 allowing anyone to develop an algorithm for predicting user ratings for films.

Our implementation takes a genetic or biological approach to solve this problem. We begin by creating a random vector for each user and each movie-rating (each movie has five possible ratings, so each movie gets five vectors). We put these vectors on a high dimensional unit sphere. We go through each user-movie-rating pair and attract the two closer together. We also repel these vectors away from random vectors. The user vector is repelled from a given number of random movie-rating vectors, and the movie-rating vector is repelled from a random number of user vectors. Then, the user vector is repelled from a random user vector, and the movie-rating vector is repelled from a random movie-rating vector.

The dataset is divided randomly into three sets: training, validation, and test. The training set is used for training, the validation set is used to tune our hyperparameters, and the test set will be used to find our final performance rating for the model.

The probability is calculated for each movie-rating and the expected value is calculated, which is our final prediction. The mean squared error is calculated to see how well our algorithm is doing. Currently, the best RMSE we have achieved is 0.943463.

## How to use
First, clone this repository using the following code:
```
git clone https://github.com/ztizzlegaming/netflix-prize
```
One must also have the Netflix Prize dataset, which can be found [here](https://www.dropbox.com/s/32jbztb1evu3lk3/netflix_data_c.bin?dl=0). For ease, put this file in the same netflix-prize folder.

To compile the code, use the following command:
```
g++ main.cpp -o netflix -std=c++11 -O3
```

To run, use the following command:
```
./netflix input.txt netflix_data_c.bin 100480507
```
The first argument is the input file. The second is the dataset. The third is the number of data points to consider. 100,480,507 is the total number of data points. Fewer data points can be used for the algorithm to run more quickly.

When using the entire dataset, the program uses about 4.5 GB of RAM and takes about 20 minutes for each iteration.

## Input File
The input file is simply a text file which is used for configuring several of the hyperparameters used in the model.

dimensions  
η  
φ (users)  
φ (movie-ratings)  
iterations  
repulsion number

Dimensions is the number of dimensions that the user and movie-rating vectors are be. The model seems to perform better with more dimensions, so upwards of 40 or 50 works best. However, more dimensions does increase runtime.

η is the initial value of η. η is basically the power of the attraction and repulsion. A small η means the vectors will not move much when being attracted or repelled, but a large η means they will be moved significantly. η must be between zero and one. η = 0 means the vectors do not move at all, and η = 1 means when a vector is moved, it will move exactly to the spot of the vector it is being attracted to. We tend to keep η small: around 0.01.

φ is the rate that the vector movement slows down. Over time, the power of the attraction and repulsion decreases. Smaller values of φ cause the power to decrease very quickly, while larger values of φ cause the power to decrease more slowly. As there are 480,189 user vectors and 17,770 * 5 = 88,850 movie-rating vectors, we see that there are about 5.4 times as many users as movie-ratings. To keep the slow-down at the same rate, we tend to keep the movie-rating φ 5.4 times as large as the user φ.

Iterations is the number of passes we will take over the training data moving the vectors before terminating.

Repulsion number is the number of movie-rating vectors the user vector is repelled from and the number of user vectors the movie-rating vector is repelled from when doing attraction and repulsion. This does increase runtime, so we usually don't go above ten.

```input.txt``` contains the current best hyperparameters we have found.