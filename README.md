# Agent-Based-Modeling-of-Environmental-Enrichment-Effects-on-Synaptic-Plasticity
Agent-based computational model of environmental enrichment effects on Hebbian synaptic plasticity in artificial neural networks.

## 1.Overview
Environmental experience can modify neuronal activity, synaptic organization, and the strength of neural connection. However, understanding how local plasticity rules can generate changes at the network level is challenging.
This project uses an agent-based computational model implemented in NetLogo to investigate how different levels of environmental enrichment affect the emergent properties of an artificial neural network.
The model combines:
--Hebbian synaptic plasticity
--Neuronal activation through a sigmoid function
--Stochastic formation of new synaptic connections
--Activity-deepndent synaptic pruning
--Multiple stochastic simulation replicates

The main hypothesis is that increased environmental enrichment may enhance synaptic plasticity and promote stronger network connectivity by increasing neuronal simulation, the effective  Hebbian learning rate, and the probability of forming new connections.

**Important modeling note:** environmental enrichment is represented here as a computational abstraction rather than a direct biological simulation.  The model increases external neuronal stimulation and selected plasticity parameters as enrichment increases. Therefore, the results should be represented as predictions of the model under these assumptions rather than as a quantitative reconstruction of biological neural development.

## 2. Research Question
**How do different levels of environmental enrichment affect synaptic connectivity and synaptic strength in an artificial neural network goevrned by local plasticity rules?**
The model compares three environmental conditions:
| Environment | Level | Description |
|---|---:|---|
| Low | 0 | Baseline environmental stimulation |
| Moderate | 1 | Intermediate stimulation and plasticity |
| High | 2 | Increased stimulation and plasticity |

## 3. Model Structure
The network consists of **50 artificial neurons** represented as agents in a two-dimensional NetLogo environment. Each neuron has:
-- External input activity \(x_j\)
-- Accumulated synaptic input \(h_i\)
-- Activation \(y_i\)
Directed synaptic connections are represented using NetLogo `directed-link-breed`.

Each synapse has: 
-- Synaptic strength \(w_{ij}\)
-- Hebbian weight change \(\Delta w_{ij}\)
The network begins as a sparse stochastic directed network and evolves over time through plasticity, connection formation, and pruning.
## 4. Computational Model
### 1. Synaptic input
The input received by neuron \(i\) is calculated as:
\[
h_i = \sum_j w_{ij}x_j
\]
where:
-- \(h_i\) = accumulated input to neuron \(i\)
--\(w_{ij}\) = strength of the synapse from neuron \(j\) to neuron \(i\)
-- \(x_j\) = external activity of neuron \(j\)

### 2. Neuronal activation
The accumulated input is transformed through a sigmoid activation function:
\[
y_i = \frac{1}{1+e^{-h_i}}
\]
where \(y_i\) represents the post-synaptic neuronal activation

### 3. Hebbian plasticity
Synaptic weights are updated according to a simplified Hebbian learning rule:
\[
\Delta w_{ij} = \eta x_j y_i
\]
where:
-- \(\Delta w_{ij}\) = change in synaptic strength
-- \(\eta\) = effective Hebbian learning rate
--\(x_j\) = pre-synaptic activity
-- \(y_i\) = post-synaptic activity
The weight is then updated according to:
\[
w_{ij}(t+1)=w_{ij}(t)+\Delta w_{ij}
\]
Weights are constrained to the interval:
\[
0 \leq w_{ij} \leq 1
\]
This formulation implements a simplified correlation-based Hebbian mechanism: synapses connecting active pre- and post-synaptic units become stronger.

### 4. Environmental enrichment
Environmental enrichment is represented through three computational levels:
\[
E \in \{0,1,2\}
\]
The baseline learning rate is:
\[
\eta_0 = 0.01
\]
For the three enrichment levels:
-- Low: \(\eta = \eta_0\)
-- Moderate: \(\eta = 1.5\eta_0\)
-- High: \(\eta = 2\eta_0\)
External neuronal stimulation is also increased with enrichment.
At the individual-neuron level:
-- Low: x = baseline
-- Moderate:x = baseline + random(0, 0.3)
-- High:x = baseline + random(0, 0.6)

### 5. Formation of new connections
New synaptic connections can emerge stochastically between neurons. The baseline probability of forming a new connection is:
P_{add}=0.01

### 6. Synaptic Pruning
The model also includes stochastic synaptic pruning. The probability of pruning a synapse is:
P_{prune}=P_w\exp\left(-\frac{w_{min}}{w_{ij}}\right)
with: P_w=0.05   and: w_{min}=0.20
Weak synapses therefore have a higher probability of being eliminated, while stronger synapses are less likely to be pruned. And synapses with non-positive strength are removed directly.

## 5. Simulation Procedure
Each simulation proceeds for 1000 ticks.
At each tick:
1) Generate environmental input
2) Calculate synaptic input
3) Update neuronal activation
4) Apply Hebbian learning
5) Update synaptic weights
6) Add new connections
7) Apply synaptic pruning
8) Calculate network-level metrics
9)Record the simulation state
For each environmental condition, 20 independent stochastic replicates were performed, which resulted in 60 simulations. Also, each replicate uses a different random seed, which allows the variability produced by stochastic network dynamics to be evaluated.

## 6. Network-Level Measurements
Three primary network properties were evaluated.
1) Total Number of Synapses
2) Connection Density
3) Mean Synaptic Weight

## 7. Results
The simulations showed different responses among the network properties.
### 1. Synaptic Number
The total number of synapses did not differ significantly among environmental conditions:
$$ p=0.474 $$
### 2. Network Density
Connection density similarly showed no statistically significant difference:
$$ p=0.474 $$
### 3. Mean Synaptic Weight
In contrast, mean synaptic weight increased strongly with environmental enrichment:
| Environment | Mean Synaptic Weight |
| :--- | :--- |
| Low | 0.138 ± 0.0015 |
| Moderate | 0.207 ± 0.0037 |
| High | 0.316 ± 0.008 |

The difference among conditions was statistically significant: **p<0.0001**
These results suggest that, under the assumptions of this model, environmental enrichment primarily affects synaptic strength rather than the overall number or density of connections.

## 8. Interpretation
The model produces an interesting distinction between structural connectivity and synaptic strength.
Increasing environmental enrichment does not necessarily result in a substantially denser network. Instead, the stronger environmental input and increased effective Hebbian learning rate lead existing synapses to accumulate greater strength.
Also, this produces a network in which enrichment is reflected primarily through changes in the distribution and magnitude of synaptic weights, rather than simply through an increase in the number of connections.
This result demonstrates how relatively simple local learning rules can generate measurable differences in emergent network properties.

## 9. Limitations
This model is intentionally simplified and should not be interpreted as a biologically complete model of environmental enrichment or neural development:
### 1.Abstract neuronal dynamics
Neurons are represented as simple computational agents with sigmoid activation rather than biologically detailed neuronal models.
### 2. Simplified Hebbian learning
The plasticity rule does not explicitly model spike timing, calcium dynamics, receptor mechanisms, or experimentally measured synaptic plasticity.
### 3. Abstract environmental enrichment
Environmental enrichment is operationalized through increased external input, learning rate, and connection-formation probability rather than being modeled from a specific experimental paradigm.
### 4. Simplified structural plasticity
Synapse formation and pruning are stochastic processes governed by predefined probabilities.
### 5. Homogeneous neurons
The model does not explicitly distinguish neuronal subtypes, excitatory and inhibitory populations, or anatomical regions.
### 6. No spatial constraints on connectivity
Synaptic formation is not based on anatomical distance or realistic cortical geometry.
### 7. Model-dependent conclusions
The observed effects arise from the chosen parameterization and rules. They should therefore be interpreted as computational predictions and hypotheses rather than direct biological evidence.

## 10. Project Context
This project was developed as an independent computational neuroscience research project and presented as a scientific poster.
*Author*: Angela Lama Vargas
*Affiliation*: Faculty of Biological Sciences, Universidad Nacional Mayor de San Marcos, Lima, Peru

## 11. References
- van Praag, H., Kempermann, G., & Gage, F. H. (2000). Neural consequences of environmental enrichment. Nature Reviews Neuroscience, 1, 191–198.
- Nithianantharajah, J., & Hannan, A. J. (2006). Enriched environments, experience-dependent plasticity and disorders of the nervous system. Nature Reviews Neuroscience, 7, 697–709.
- Chauhan, K., Nehal, A. B., & Tassi, P. A. (2024). Synaptic reorganization for neural network plasticity: weight and structural plasticity. PLoS Computational Biology.
