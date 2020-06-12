# Speech-Gender-Classifer
\documentclass[letterpaper]{article}
\usepackage{aaai}
\usepackage{times}
\usepackage{helvet}
\usepackage{courier}
\usepackage[hyphens]{url}
\usepackage{graphicx}
\usepackage{color}
\urlstyle{rm}
\def\UrlFont{\rm}
\usepackage{graphicx}
\frenchspacing
\setlength{\pdfpagewidth}{8.5in}
\setlength{\pdfpageheight}{11in}
% Add additional packages here.
%
% The following
% packages may NEVER be used (this list
% is not exhaustive:
% authblk, caption, CJK, float, fullpage, geometry,
% hyperref, layout, nameref, natbib, savetrees,
% setspace, titlesec, tocbibind, ulem
%
%
% PDFINFO
% You are required to complete the following
% for pass-through to the PDF.
% No LaTeX commands of any kind may be
% entered. The parentheses and spaces
% are an integral part of the
% pdfinfo script and must not be removed.
%
\pdfinfo{
/Title (Gender Classification on Race: A Speech Perspective)
/Author (Harry Xiong, Hank Zhang)
/Keywords (Input your keywords in this optional area)
/Email(xiong1@uchicago.edu)
}
%
% Section Numbers
% Uncomment if you want to use section numbers
% and change the 0 to a 1 or 2
% \setcounter{secnumdepth}{0}
% Title and Author Information Must Immediately Follow
% the pdfinfo within the preamble
%
\title{Gender Classification on Race: A Speech Perspective \\ Project Report}
\author {Harry Xiong, Hank Zhang \\ xiong1@uchicago.edu, hqzhang@uchicago.edu}
\begin{document}
\maketitle

\section{Abstract}
The current research stems from previous work on speech gender classification and aims to investigate classification bias for African and Asian speakers. 7 models including k-NN, Naive Bayes, SVM, linear perceptron, logistic regression, ANN and CNN are deployed to compare accuracies, False Positive Rates (FPR) and False Negative Rates (FNR) on a data set containing 837, 525 and 189 speaking samples from White, Asian and African speakers. Feature extraction is performed to transform MP3 files to  Mel-Frequency Cepstral Coefficients (MFCCs). The results suggest that there is no observable difference in the error rate on the Asian test set compared to the baseline White test set, while the African test set achieved lower classification accuracy compared to the other two.


\section{Introduction}
Gender classification tasks in speech have been extensively studied. Various research endeavors have laid emphasis on improving accuracies. The current research, on the other hand, proposes to juxtapose gender classification results of African and Asian speakers and investigate whether any difference exists. The current study is also inspired by a growing body of literature on racial and gender stereotypes in social science fields. Bertrand and Mullainathan [1] in 2005 found that African American-name resumes received less callbacks than white-name resumes. Hall et al. [2] extended the finding to two-dimensional by exploring gender-race interplay in the recruiting process. The current resesearch aims to present difference in gender classification for African and Asian speakers.


\section{Previous Work}
Past literatures by Hu et al.[3], Djemili et al.[4], and Lee and Kwak [5] exhibited excellent accuracies above 90\% on gender classification using various models such as GMM, multilayer perceptron, SVM, and decision tree. But the data sets used typically do not contain a balanced distribution across races. The current project therefore aims to fill the gap and study the variation in gender classification performance conditioned on races. 


\section{Data}
A Kaggle data set [6] that contains 2140 same English speech samples of speakers from 177 countries with 214 different native languages is used. Since the races are not specified for the speakers, we have used the countries of origin as a proxy for races. Specifically, speaking samples of speakers from U.S, Canada and Europe are included in the White data set, whereas samples of speakers from Asia and Africa are included in the Asian and African test sets  (525 samples and 189 samples) respectively. The White data set is further split into a training set (711 samples) and a test set (126 samples). MP3 files are transformed to Mel-Frequency Cepstral Coefficients (MFCCs) using the librosa library with a sampling rate of 16000 fs. 40 MFCCs are extracted for each sample and data is padded for the CNN model.


\section{Experiments}
The experiments will use the MFCC features as the independent variables and the speaker's gender (0/1) as the dependent variable. In order to observe patterns in gender classification performance across races, we will train a number of models so that model-dependent patterns are eliminated. 
\subsection{Determining the Baseline Errors}
To arrive at the final models, the model tuning process is carried out on white-speaker data. Eventually, we aim to compare test set errors on 3 test sets, one for each race. Our hypothesis is that results for African Americans will be biased towards misclassifying more females to males relative to the White control group and for Asians it will be the opposite.\\ \\To obtain the test set error for White speakers, a test set is held out under a 85-15 train-test split. And the training set will be further partitioned (for 10-fold cross-validation in most models and for simple cross-validation when tuning the CNN model) in order to select the best hyperparameters for each model. Finally, the test set error for White speakers will be used as the baseline error rate for the trained model. The data for African and Asian speakers will be used solely as test sets.

\subsection{Model Selection and Tuning}
We train a total of 7 models in order to ensure any difference in error rate, if any, would be indeed persistent and not coincidental. In the extreme case, performance gaps might even be model-independent, but we do not necessarily anticipate such clean results. \\ \\The models that we experiment on will be k-NN, Naive Bayes, SVM, linear perceptron, logistic regression, artificial neural networks (ANN), and convolutional neural networks (CNN). For each model, we tune the hyperparameters, if any, until a validation set (consisting of White speakers) accuracy of near 90\% is reached.\\ \\Table 1 presents the model details for the first 6 models, and details for the CNN model can be found in figure 1.\\
    \begin{center}
    \begin{tabular}{ |c|c| } 
    \hline
    \textbf{Models} & \textbf{Model Details}\\ 
    \hline \hline
    k-NN & $k=8$\\ 
    \hline
    Naive Bayes & Gaussian Naive Bayes \\ 
    \hline
    SVM & degree-3 polynomial kernel\\
     & L2 regularization: $\alpha = .1$\\
    \hline
    Perceptron & linear perceptron\\
    & L2 regularization: $\alpha = .00005$\\
    \hline
    Logistic regression & L2 regularization: $\alpha = 10$\\
    \hline
    ANN & 1 hidden layer of 100 nodes \\
     & tanh activation\\
    \hline
    \end{tabular}
    \caption{Table 1: model details}
    \label{table:1}
    \end{center}
\subsection{CNN-based Model}
For the CNN model, we tuned the model based on validation set performance while fixing the model layers fixed. We were able to find the optimal number of channels to include in each convolutional layer to be 16 and 32 respectively. The convolutional layers are followed by a fully connect layer and finally a softmax layer. The model achieved over 97\% accuracy on the validation set and as we will see, around 93\% accuracy on the test set.\\
    \begin{figure}[!htb]
        \centering
        \includegraphics[width=0.7\columnwidth]{figures/cnn_diagram.png}
        \caption{CNN model diagram}
        \label{fig:my_label}
    \end{figure}

\section{Results}
After using the trained models on the 3 test sets, we are able to observe the difference in error rates. It would not be surprising if the test set error rates are higher on both the Asian and the African test sets because after all, the models were tuned on the White training and validation sets.\\ \\Interestingly, what we observe is that in general, there is no observable difference in the error rate on the Asian test set compared to the baseline White test set. For some models, the performance on the Asian test set is even better. On the other hand, all models predict less accurately on the African test set, compared to on the Asian counterpart. Further, except for the SVM model, the test set accuracy is lower for African speakers compared to the baseline White speakers. \\ \\
For detailed test set accuracies, please see table 2. Further breakdown of the precisions, recalls and errors for the first 6 models in false positive rates (FPR) and false negative rates (FNR) are provided in the Appendix.\\
    \begin{center}
    \begin{tabular}{ |c|c|c|c| } 
    \hline
    \textbf{Models} & \textbf{Baseline} & \textbf{African}& \textbf{Asian}\\ 
    \hline \hline
    k-NN & .825 & .730 & .787\\ 
    \hline
    Naive Bayes & .889 & .873 & .901\\ 
    \hline
    SVM & .889 & .910 & .939\\
    \hline
    Perceptron & .825 & .788 & .842\\
    \hline
    Logistic regression & .929 & .910 & .928\\
    \hline
    ANN & .913 & .910 & .935\\
    \hline
    CNN & .937 & .878 & .930\\
    \hline
    \end{tabular}
    \caption{Table 2: test set accuracies}
    \label{table:1}
    \end{center}
Our initial hypothesis that the FPR for African speakers would be higher than the Asian speakers and that the FNR would be the opposite is not confirmed by the experiment results. Specifically, compared to Asian speakers, both FPR and FNR are higher on the African test set. Nevertheless, the consistently higher error rates on the African test set across all models is a strong indication that gender classification may be conditioned on race.
\section{Discussion}
 There is room for improvement in terms of the data set used. Our data set does not have information on race, and countries of origin can be an inaccurate proxy.\\ \\Secondly, we are not able to control for accents. Ideally, the speakers in the data set should all be native English speakers of a single type of accent. Otherwise, phonetic differences could in fact be the reason causing the lower accuracy on the African test set.\\ \\Arguably, the gender balance between male and female speakers can be improved. However, since the FPR and FNR are mostly both higher on the African test set, gender balance is unlikely to have caused biased error rates in our experiments.\\ \\Overall, for the consistently lower prediction accuracy on the African test set, our study suggests that training gender classification models on predominantly White English speakers could cause undesirable performance issue for African speakers. This is an indication that gender classification is indeed conditioned on race. Consequently, social implications could be further explored to potentially address the gendered stereotypes in race. \\ \\

% References and End of Paper
% These lines must be placed at the end of your paper

\begin{thebibliography}{9}
    \bibitem{latexcompanion} 
    [1] M. Bertrand and S. Mullainathan, “Are Emily and Greg more employable than Lakisha and Jamal? A field experiement on labor market discrimination,”
    \textit{American Economic Review}, 
    94.4 (2004): 991-1013.
    
    \bibitem{latexcompanion} 
    [2] E. Hall, A. Galinsky, and K Phillips, "Gender profiling: A gendered race perspective on person-position fit," 
    \textit{Personality and Social Psychology Bulletin}, 
    41.6 (2015): 853-868.
    
    \bibitem{latexcompanion} 
    [3] Y. Hu, D. Wu, and A. Nucci, “Pitch-based gender identification with two-stage classification,”
    \textit{Security and Communication Networks}, 
    vol. 5, no. 2, pp. 211–225, 2012.
    
    \bibitem{latexcompanion} 
    [4] R. Djemili, H. Bourouba, and M. C. A. Korba, “A speech signal based gender identification system using four classifiers,” in
    \textit{Proceedings of the 2012 International Conference on Multimedia Computing and Systems}, 
    pp. 184–187, Tangiers, Morocco, May 2012.
    
    \bibitem{latexcompanion} 
    [5] M.-W. Lee and K.-C. Kwak, “Performance comparison of gender and age group recognition for human-robot in- teraction,” 
    \testit{IJACSA International Journal of Advanced Computer Science and Applications},
    vol. 3, no. 12, 2012.
    
    \bibitem{latexcompanion} 
    [6] Kaggle Speech Accent Archive. Parallel English speech samples from 177 countries.
    \textit{https://www.kaggle.com/rtatman/speech-accent-archive?select=speakers\_all.csv}.
    
\end{thebibliography}


\section{Appendix}
   \begin{center}
    \begin{tabular}{ |c|c|c|c| } 
    \hline
    \textbf{Models} & \textbf{Baseline} & \textbf{African}& \textbf{Asian}\\ 
    \hline \hline
    k-NN & .766 & .794 & .741\\ 
    \hline
    Naive Bayes & .855 & .897 & .892\\ 
    \hline
    SVM & .867 & .917 & .933\\
    \hline
    Perceptron & .936 & .975 & .972\\
    \hline
    Logistic regression & .909 & .931 & .925\\
    \hline
    ANN & .894 &.946 & .940\\
    \hline
    \end{tabular}
    \caption{Table 3: test set precisions}
    \label{table:1}
   \end{center}
   
   \begin{center}
    \begin{tabular}{ |c|c|c|c| } 
    \hline
    \textbf{Models} & \textbf{Baseline} & \textbf{African}& \textbf{Asian}\\ 
    \hline \hline
    k-NN & .937 & .761 & .858\\ 
    \hline
    Naive Bayes & .937 & .897 & .917\\ 
    \hline
    SVM & .921 & .940 & .941\\
    \hline
    Perceptron & .698 & .675 & .692\\
    \hline
    Logistic regression & .953 & .923 & .925\\
    \hline
    ANN & .937 &.906 & .925\\
    \hline
    \end{tabular}
    \caption{Table 4: test set recalls}
    \label{table:1}
   \end{center}
   
   \begin{center}
    \begin{tabular}{ |c|c|c|c| } 
    \hline
    \textbf{Models} & \textbf{Baseline} & \textbf{African}& \textbf{Asian}\\ 
    \hline \hline
    k-NN & .286 & .319 & .279\\ 
    \hline
    Naive Bayes & .159 & .167 & .103\\ 
    \hline
    SVM & .143 & .139 & .063\\
    \hline
    Perceptron & .048 & .028 & .018\\
    \hline
    Logistic regression & .095 & .111 & .070\\
    \hline
    ANN & .111 & .083 & .055\\
    \hline
    \end{tabular}
    \caption{Table 5: test set false positive rates}
    \label{table:1}
   \end{center}

   \begin{center}
    \begin{tabular}{ |c|c|c|c| } 
    \hline
    \textbf{Models} & \textbf{Baseline} & \textbf{African}& \textbf{Asian}\\ 
    \hline \hline
    k-NN & .063 & .239 & .119\\ 
    \hline
    Naive Bayes & .063 & .102 & .083\\ 
    \hline
    SVM & .079 & .060 & .059\\
    \hline
    Perceptron & .302 & .325 & .308\\
    \hline
    Logistic regression & .048 & .077 & .075\\
    \hline
    ANN & .063 & .094 & .075\\
    \hline
    \end{tabular}
    \caption{Table 6: test set false negative rates}
    \label{table:1}
   \end{center}
\end{document}
