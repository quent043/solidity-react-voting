# Dapp Voting Project

Projet d'application décentralisée de vote utilisant la box React de Truffle, devant permettre:

- l’enregistrement d’une liste blanche d'électeurs.
- à l'administrateur de commencer la session d'enregistrement de la proposition.
- aux électeurs inscrits d’enregistrer leurs propositions.
- à l'administrateur de mettre fin à la session d'enregistrement des propositions.
- à l'administrateur de commencer la session de vote.
- aux électeurs inscrits de voter pour leurs propositions préférées.
- à l'administrateur de mettre fin à la session de vote.
- à l'administrateur de comptabiliser les votes.
- à tout le monde de consulter le résultat.
---
## Sécurité

Pour garantir la sécurité du smart contract contre les éventuelles attaques, les mesures suivantes ont été prises:

- Pas de boucle for pour la comptabilisation des votes des proposals afin d'éviter une attaque DoS. 
l'enregistrement de proposals par les voters n'êtant pas capée. La proposition gagnante est mise à jour à chaque vote via les variables "winningProposalID" et "maxVotes" (ex-aequo non pris en compte).
La consommation de gas sera plus élevée, mais n'ayant pas de contexte sur l'utilisation de ce contrat, j'ai privilégié la sécurité à la consommation.
- La variable "winningProposalID" n'est plus "public" afin d'en permettre la consultation qu'à la fin du vote.

###Piste d'amélioration: 
- Selon l'utilisation de ce contrat, j'aurai plus considéré de conserver la fonction "tallyVotes()" avec boucle for et de  limitter le nombre de propositions pouvant être soumises par utilisateur.
- Ou mieux, de donner à chaque votant un nombre de tokens initial leur permettant de soumettre des propositions (1 par token) et de donner la possibilité à l'owner de leur envoyer d'autres tokens s'ils en font la demande. Comme ça l'owner a le contrôle et peut repérer les attaquants.

---

## Optimisation

Afin de réduire au maximum la consommation en gas, les aménagements suivants ont été faits:

- Groupement des Variables d'état "winningProposalID" et "maxVotes" en uint128, autorisant un nombre maximal de votes ou de proposals de (2**128) - 1, jugé suffisant.
- Utilisation de "calldata" dans les arguments de fonctions
- Changement de la variable "voteCount" du struct "Proposal" de uint256 = uint128, pour rester cohérent avec "maxVotes"

