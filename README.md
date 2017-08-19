# Clustering-Coefficient
Udacity Clustering Coefficient

## Code
```
import UIKit

typealias Segment = (String,String)
typealias NeighborType = Dictionary<String, [String]>

var pairs =  [("ORD", "SEA"), ("ORD", "LAX"), ("ORD", "DFW"), ("ORD", "PIT"), ("SEA", "LAX"), ("LAX", "DFW"), ("ATL", "PIT"), ("ATL", "RDU"), ("RDU", "PHL"), ("PIT", "PHL"), ("PHL", "PVD"), ("DFW", "ATL")]

let neighbors: NeighborType = getNeighbors(from: pairs)

var total: Float = 0.0

neighbors.keys.forEach {
    let ccv = clusteringCoefficient(of: $0, in: neighbors)
    print("\($0) -> \(ccv)")
    total += ccv
}

let average = total / Float(neighbors.keys.count)

print("Total: \(total)")
print("Average: \(average)")



func getNeighbors(from pairs: [Segment]) -> NeighborType {
    var dict = NeighborType()

    for pair in pairs {
        if dict[pair.0] == nil {
            dict[pair.0] = []
        }
        
        if dict[pair.1] == nil {
            dict[pair.1] = []
        }
        
        dict[pair.0]!.append(pair.1)
        dict[pair.1]!.append(pair.0)
    }
    
    return dict
}

func clusteringCoefficient(of node: String, in neighborhood: NeighborType) -> Float {
    
    guard let neighborsOfNode = neighborhood[node] else {
        return 0.0
    }
    
    let kV: Float = Float(neighborsOfNode.count)
    var nV: Float = 0.0
    
    neighborsOfNode.forEach {
        guard let secondDegreeNeighbor = neighborhood[$0] else {
            return
        }
        
        secondDegreeNeighbor
            .filter { $0 != node }
            .forEach {
                
                guard let thirdDegreeNeighbor = neighborhood[$0] else {
                    return
                }
                
                if thirdDegreeNeighbor.contains(node) {
                    nV += 0.5
                }
        }
    }
    
    return kV > 1 ? 2 * nV / (kV * (kV - 1) ) : 0.0
}


```

## Output 
```
PVD -> 0.0
PHL -> 0.0
ATL -> 0.0
RDU -> 0.0
LAX -> 0.666667
ORD -> 0.333333
DFW -> 0.333333
SEA -> 1.0
PIT -> 0.0
Total: 2.33333
```
Average: 0.259259
