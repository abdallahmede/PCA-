PCA & K-Means for Customer Profiling

Discovering personality and lifestyle profiles in survey data with Principal Component Analysis and k-Means clustering.

Course project for Computational Linear Algebra (MSc Data Science and Engineering, Politecnico di Torino, A.Y. 2025/2026). Team: Abdallah Hmedeh, Mohammad Al-Jnainati.

Problem

The Young People Survey asks respondents about their music and movie tastes, interests, phobias, spending habits, health and personality, so each person is described by roughly 150 answers. At that size it's hard to see patterns or group similar people.

Goal: reduce the data to a few interpretable dimensions, segment respondents into meaningful profiles, and check whether those profiles relate to demographics that were not used to build them.

Data
Source: the Young People Survey dataset (674 respondents, 150 columns, mostly Likert-scale answers from 1 to 5).
Personal subset: 506 respondents × 93 features, sampled with a fixed seed. The 10 demographic columns (age, gender, education, …) are kept apart as external labels for evaluation.
Files: responses_hw.csv holds the answers and columns_hw.csv maps full question text to short names.
Approach
Encoding. The 5 categorical features (Smoking, Alcohol, Punctuality, Lying, Internet usage) have a natural order, so each gets an ordinal encoding that keeps their meaning.
Scaling. MinMaxScaler maps every feature to [0, 1], so no feature dominates just because of its range. A correlation heatmap and variance plots before and after scaling show the effect.
PCA on all components. Cumulative explained-variance curves are compared for the unscaled and the scaled data.
Dimensionality reduction. The data is reduced to m = min(m′, 5) components, where m′ is the number of PCs needed to explain 33% of the variance. Each PC is interpreted from its strongest loadings and given a name.
Clustering. k-Means runs on the PCA scores, and k ∈ {3, …, 10} is selected by silhouette score.
Evaluation.
Internal: silhouette score per cluster and silhouette plot.
External: distribution of Gender, Education and Only-child status in each cluster.
Results
Explained variance
Variance threshold	PCs needed (encoded)	PCs needed (MinMax-scaled)
80%	45	46
90%	62	63
95%	73	75

The variance is spread over many components, as expected for subjective survey answers. Reaching 33% would take m′ = 8 PCs, so the cap applies: m = 5 PCs, which explain 25.7% of the variance.

Principal components
PC	Name	High scores	Low scores
PC1	Reflective vs Action	Life struggles, Romantic movies, Writing notes, fear of Snakes/Spiders	Cars, War movies, Adrenaline sports, Metal/Hard rock
PC2	Intellectual & Cultural Orientation	Classical music, Musical instruments, Swing/Jazz, Reading, History	none
PC3	Consumerism & Social Image	Hip hop/Rap, Spending on looks, Shopping, Branded clothing	Metal/Hard rock
PC4	Discipline vs Flexibility	Punctuality, Prioritising workload, Chemistry, Biology	Getting up, History, Writing, Criminal damage
PC5	Sciences vs Civic Routine	Biology, Chemistry, Fantasy/Fairy tales	Politics, Economy Management, Daily events, Punctuality
Clusters (k = 3, silhouette = 0.175)
Cluster	Profile	Silhouette
1. Thinkers	High intellectual engagement, reflective, little interest in consumption	0.158
2. Actives	Action-oriented, high consumer and lifestyle engagement	0.184
3. Reserved	Reflective, low consumer engagement, avoid thrill-seeking activities	0.179
Internal check: every cluster has a positive silhouette with almost no negative values. The clusters overlap, as is typical for survey data, but each one can be interpreted.
External check: Gender was not used for clustering, yet it separates the clusters. Cluster 1 is mostly female, Cluster 3 leans male, and Cluster 2 is balanced. Education and Only-child status also differ between clusters, which suggests the profiles reflect real behavioural structure.
<!-- Add your figures, e.g.: --> <!-- ![Cumulative explained variance](images/cumulative_variance.png) --> <!-- ![3D score plot with clusters](images/clusters_3d.png) --> <!-- ![Silhouette plot](images/silhouette.png) -->
Tech stack

Python · NumPy · Pandas · scikit-learn (MinMaxScaler, PCA, KMeans, silhouette_score) · Matplotlib · Seaborn · Jupyter

How to run
bash
git clone https://github.com/abdallahmede/<repo-name>.git
cd <repo-name>
pip install numpy pandas scikit-learn matplotlib seaborn jupyter
jupyter notebook HWpca_AlJNAINATI_HMEDEH.ipynb

Keep responses_hw.csv and columns_hw.csv in the same folder as the notebook. The fixed seed (based on our student IDs) makes the subset and the results reproducible.

Repository structure
├── HWpca_AlJNAINATI_HMEDEH.ipynb   # full analysis
├── HWpca_AlJNAINATI_HMEDEH.html    # rendered notebook (view without running)
├── responses_hw.csv                # survey answers
├── columns_hw.csv                  # question → short-name mapping
└── images/                         # exported figures used in this README
What I learned
Why scaling matters before PCA, and how to read cumulative explained-variance curves.
How to turn PCA loadings into human-readable dimensions.
How to choose k with the silhouette score, and how to validate clusters both internally and against external labels.
