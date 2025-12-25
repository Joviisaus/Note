---
tags:
  - mesh
  - Paper
---

# Approach overview
automatically decomposes a complex B-Rep solid model into a set of sub-volumes, including all of the pseudo-swept volumes of the model, to effectively support the automatic generation of a high-quality hexahedral mesh.

Problem:
1. how to effectively **recognize pseudo-swept volumes** from a solid model
2. how to effectively **separate heavily interacting pseudo-swept** volumes, so that the resulting pseudo-swept volumes are suitable for high-quality hexahedral meshing.

Step:
1. Generate the faceted representation,shown in Fig. 3a–b
2. Extract the sweep directions,shown in Fig. 3c–e
3. Determine the interacting areas and separate all of them,shown in Fig. 3f–i
4. generate its allhexahedral mesh,shown in Fig. 3j–k.
 
![[Pasted image 20250625142641.png]]

$l$ is a pseudo-swept volume with $v_s$ as its sweep direction:
- $π /2 − δ ≤ θ ≤ π /2 + δ$
- $π  − δ ≤ θ ≤ π$
- $0 ≤ θ ≤ δ$
where $θ$ denotes the angle between $v_s$ and the normal direction at any point of $f$ ,and $δ (δ < π /4)$ is a threshold angle that defines the accuracy requirement.
# Recognition of potential pseudo-swept volumes
The **input** is a B-Rep solid model, and the faceted representation of the solid model is generated first.
Then iteratively form clusters by forming one cluster at each iteration until the newly formed cluster is not valid.
The **output** is a set of clusters each of which corresponds to a potential sweep direction.
![[Pasted image 20250626105804.png]]
1. Generate the faceted representation
2. Initialize a new cluster cl by selecting a seed facet $\&$ **setting** the initial sweep direction **v**
3. Starting from the seed facet, form the component facets of $cl$
4. Assign the probability of belonging to $cl$ to each facet of it
5. Check whether the new cluster $cl$ is valid or not

## Seed facet selection
