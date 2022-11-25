# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### Q1

Article : https://www.simscale.com/blog/nasa-mars-climate-orbiter-metric/

En septembre 1999, près de 10 mois après son voyage vers Mars, le Mars Climate Orbiter a brûlé et s'est désintégré en raison d'une unité de mesure incorrecte utilisée dans le code. L'équipe de navigation du Jet Propulsion Laboratory (JPL) a utilisé le système métrique de millimètres et de mètres dans ses calculs, tandis que Lockheed Martin Aerospace qui a conçu et construit le vaisseau spatial, a fourni des données du système en pouces et en pieds. Les ingénieurs du JPL n'ont pas tenu compte du fait que les unités avaient été converties. 

On constate donc que c’est un bug global car il intervient entre différentes équipes de production en raison d’un manque de communication ou de normes préétablies.

Ce bug est donc arrivé car les différentes équipes ont utilisé des métriques propres à leur pays d’origine, donc leurs métriques personnelles. Il manque donc un document commun qui impose des normes de métriques tout comme il existe des normes de style dans le développement d’une application. 

Le coût global de cette mission était de plus de 320 millions de dollars. C’est donc une perte nette pour la NASA mais aussi  une exposition négative au média, une perte de crédibilité face aux investisseurs et face au public.

Il aurait fallu simuler la mesure du capteur en question sur une distance connue pour pouvoir comparer les résultats du code avec les résultats connus. Ainsi, ils se seraient rendu compte qu’il y a une erreur au niveau de la métrique utilisée dans le code et aurait corrigé le bug.


### Q2

bug : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-734?filter=doneissues
L’appel à la méthode EntryIterator.remove() soulève un IllegalStateException lors d’un parcours d’un Set. 
C’est un bug local car une seule méthode est concernée.
Résolution : dans la méthode EntryIterator.remove(), la méthode currentEntry.getKey() devrait être appelée avant la méthode currentEntry.setRemoved(true);


### Q3

What are the concrete experiments they perform ?

L'ingénierie du chaos est une technique permettant de répondre à l'exigence de résilience.
Elle peut être utilisée pour lutter contre les pannes d'infrastructure, les pannes de réseau et les pannes d'application. Elle consiste à perturber régulièrement des instances aléatoires d’un système pour observer si d’autres services peuvent reprendre la charge de travail de celui qui est mis volontairement en panne.

What are the requirements for these experiments ?

Le prérequis principal est d’avoir un environnement stable que l’on peut perturber afin d’analyser sa capacité à réagir au problème. On observe la résilience d’un système face à des faiblesses inconnues.

What are the variables they observe ? 

Il faut définir précisément les variables que l’on souhaite observer : l’environnement technique, le support, la maintenance … Il faut aussi préciser les variables qui permettront de valider l’expérience et donc les résultats attendus. 


What are the main results they obtained ?

On simule donc des pannes en environnement réel avec des Chaos Monkey et on vérifie que le système continue à fonctionner. L’expérience permet donc de savoir s’il faut mettre en place de nouveaux plans d’action vis à vis d’une panne précise. S’il y a une surcharge d’un serveur alors il faut prévoir de répartir la charge sur un nouveau serveur.


Netflix n’est pas la seule entreprise à utiliser le Chaos Engineering.
En effet, Amazon a mis en place un “Gameday” pour tester la résilience du système et un Chaos Kong qui paralyse une partie du système. De même pour OUI.sncf avec son “Days of chaos''.


### Q4

Le web assembly est le premier langage web qui délivre un certains nombres d’objectifs directement à l’utilisateur : rapide, portable (plusieurs browsers, os et devices) et compacte (pour réduire le temps de chargement). 
Beaucoup de technologies existent déjà pour créer des applications web et ce papier montre la différence entre ces technologies et web assembly. 
Le but du web assembly est de pouvoir faire de la programmation bas niveau (low-level) tout en garantissant une certaine sécurité en protégeant les données utilisateurs.

Bien que le web assembly a été prouvé théoriquement, des bugs peuvent apparaître lors de l'exécution du code. C’est pourquoi, il est important de tester le langage.


### Q5

**What are the main advantages of the mechanized specification ?**
La spécification mécanisée a pour avantages de réduire le nombre de relations dans un langage afin d’obtenir un langage plus simple et facilement testable.

**What other artifacts were derived from this mechanized specification ?**
Les "soundness properties" sont des propriétés dérivées de la spécification mécanisée.

**Does this new specification remove the need for testing ?**
Les tests sont toujours nécessaires dans la spécification mécanisée, il reste toujours des erreurs à vérifier.

