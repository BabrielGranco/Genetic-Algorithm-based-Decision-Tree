#TODO
- TRADUZIR OS CÓDIGOS COM OS TESTES ESTATÍSTICOS (4)
- TRADUZIR OS CÓDIGOS COM OS TESTES DE DESENVOLVIMENTO (3)
- EXTRAIR OS TRECHOS DO ARTIGO QUE FALA SOBRE A GAbDT e TRADUZIR PARA ADICIONAR AQUI

This project is a Decision Tree (DT) that utilizes a genetic algorithm to select the best partitions to be made in the formation of the tree. The Genetic Algorithm (GA) is used to minimize a fitness function that calculates the majority error, or another desired evaluation metric, generated in the data from a specific split. In this way, it attempts to find the best premise to recursively separate the individuals until the decision tree is formed. It is interesting to note that the fitness function can be used to maximize or minimize different metrics that would function as a measure of the impurity of the formed sets.

Thus, each individual of the GA will be composed of a premise. The premise is a set of an attribute, an operator, and a threshold (['att', 'op', 'ths']), which will be used to split our training set at each node. The algorithm begins with a population of n premises and evolves over the generations by choosing the rules that minimize the majority error. To do this, it applies each of the premises and calculates the majority class error in both of the generated groups. Finally, it calculates the weighted sum of the resulting values based on the size of the sets, this being the fitness of each solution.

The lower the value of the fitness function, the better the premise divides the training set, and the more homogeneous the generated sets are. The next generations, within the same node, are composed of the best individual generated and of offspring formed through crossover and mutation operations between parents selected by a three-individual tournament. In the final generation, the best individual is selected to be the premise that will separate the current dataset. The same process is done for the subsequent nodes generated in each split until the groups become sufficiently homogeneous (minimized majority error) and the algorithm reaches a leaf node.

In this evolutionary decision tree algorithm, several hyperparameters can be manually tuned to influence the growth and behavior of the tree:

max_profundidade (Maximum Depth): This parameter sets the maximum depth of the tree. By limiting depth, we can prevent overfitting and control the complexity of the tree. Larger values allow more detailed splits but may lead to overfitting, while smaller values encourage simpler, more general models.

min_samples (Minimum Samples per Node): This defines the minimum number of samples required to create a split at a node. Increasing this value prevents splits on very small subsets, which can stabilize the tree and reduce overfitting.

n_geracoes (Number of Generations): This determines how many generations the genetic algorithm will run for optimizing node splits. Higher numbers give the algorithm more opportunities to find better solutions but also increase computational time.

tamanho_pop (Population Size): This is the number of candidate solutions (individuals) generated in each generation of the genetic algorithm. Larger populations increase the diversity of solutions and the likelihood of finding optimal splits, at the cost of higher computation.

taxa_mutacao (Mutation Rate): This controls the probability of random mutations in the genetic algorithm. Higher mutation rates introduce more variability in the population, helping to explore the search space, whereas lower rates favor refinement of existing solutions.

erro_threshold (Error Threshold): This value sets a stopping criterion for a branch: if the majority error in a node falls below this threshold, the node is converted into a leaf. Lower thresholds allow finer splits, while higher thresholds encourage early stopping.

atributos (Features) and intervalo_atributos (Feature Ranges): These parameters define which attributes the tree can use for splitting and the ranges for those attributes. They are crucial for guiding the genetic algorithm in generating valid candidate splits.

By adjusting these hyperparameters, the user can control the trade-off between tree complexity, generalization, and computational cost, allowing the algorithm to be tailored to different datasets and tasks.

Execution exemple:

#Parameter configuration

n_geracoes = 15
tamanho_pop = 200
taxa_mutacao = 0.2
erro_threshold = 0.125
max_profundidade=15
min_samples=10
atributos = df.columns[df.columns != "classes"].tolist()
coluna_classe="classes"
intervalo_atributos = calcular_intervalo_atributos(df, atributos)

#Initializing the evolution processes

tree = evoluir_solucoes(df, atributos, intervalo_atributos, n_geracoes=n_geracoes, tamanho_pop=tamanho_pop, min_samples=min_samples, max_profundidade=max_profundidade, taxa_mutacao=taxa_mutacao, coluna_classe="classes")

The custom decision tree also includes utility functions for both prediction and visualization. The predict_custom_tree function allows the user to classify new samples using the dictionary-based tree structure. It traverses the tree recursively, following the rules defined at each internal node, until reaching a leaf node, at which point it outputs the class with the highest frequency.

For visualization, the plot_tree_custom function provides a detailed graphical representation of the tree. Each node displays the splitting criterion, the total number of samples, and the class distribution, while leaves show the majority class and the relative proportions of all classes. Connections between nodes are plotted to reflect the tree hierarchy, and the layout dynamically adjusts based on tree depth. Together, these functions make it possible to both classify new data points and interpret the learned structure of the decision tree.
