# egor_kharakozov-DSI_Projects


In this project I look to generate NFL player / game predictions and then use them to solve the daily fantasy optimization problem. 

I work with a dataset purchased from ArmchairAnalysis.com that contains extensive play by play, player and team level NFL game data from 2000 - 2015. 

The first notebook stores the csv files downloaded from the website and creates a PostgreSQL database to store the data. Using Pandas and SQL I create a lot of features by using running averages on player game data such as passing attempts , completion percentage, yards gained etc... The idea is that a players recent and historic perfomance is predictive of his performance in the current week. Additionally there is some player level physical data such as speed / height / weight as well as weather and vegas odds data for each game. The query produces a large frame with which I work in the following notebook. 

The second notebook creates the data processing pipeline with two functions that scale and standardize the features and create the dummy variables for categorical features. In addition I shift the "target"  and also the weather and vegas variables up a week for each player so that we are predicting next weeks fantasy score based on data up to and including current weeks stats but also using next weeks weather and vegas expectations.

The third notebook evaluates several models to use for predicting the target. Although I only get R2 scores of roughly 20-24% this is to be expected given how random any individual perfomance can be. Expecially given that I use WR position which seems like it would be more volatile than QB. Although a grid searched Elastic Net linear regression yilds slightly better results (1-2% better R2), I choose to go with an Extra Tree Random Forest model because of how much faster it fits and predicts since I will need to iteratively go through and fit/predict week by week to backtest the historic data. 
** In the future, I may want to revisit this and re-write for spark to speed up performance and then conduct the test at each position group. It would be curious to see if perhaps different models fit best to different positions. 

The next notebook I am working on is the prediction generator, basically just a loop cycling through week by week and generating predictions based on all prior data, since this is effectively how a real time implementation would work. After that I will need to back load some daily fantasy salary data to see how my model will have versus actual results.

