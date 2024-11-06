---
Creation Time: Wednesday, October 30th 2024
Modified Time: Wednesday, October 30th 2024
---
**Example 1:**

**Input:** accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
**Output:** [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
**Explanation:**
The first and second John's are the same person as they have the common email "johnsmith@mail.com".
The third John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.


```java
public List<List<String>> accountsMerge(List<List<String>> accounts) {  
    Map<String, String> parentKeyMap = new HashMap<>();  
    Map<String, String> parentMap = new HashMap<>();  
    Map<String, String> link = new HashMap<>();  
    Map<String, TreeSet<String>> result = new HashMap<>();  
  
    // parent is assigned to parent  
    int key=0;  
    for(List<String> row : accounts){  
        String parent = row.get(0);  
        parentMap.put(String.valueOf(key), String.valueOf(key));  
        parentKeyMap.put( String.valueOf(key), parent);  
  
        // decide link  
        for(int i=1; i< row.size() ; i++){  
            if( link.get(row.get(i)) == null ){  
                link.put(row.get(i), String.valueOf(key));  
            }else{  
                String parentDestination = link.get(row.get(i));  
                parentMap.put(String.valueOf(key), parentDestination);  
            }  
        }  
        key++;  
    }  
  
    // create final result  
    for(String val : link.keySet()){  
  
        String l = findParent(parentMap,link, val);  
        if(result.get(l) == null) {  
            result.put(l, new TreeSet<>());  
        }  
        result.get(l).add(val);  
    }  
  
    // convert map to List<List<String>>  
    List<List<String>> result2 = new ArrayList<>();  
  
    for(String val : result.keySet()){  
        List<String> arr = new ArrayList<>(result.get(val));  
        arr.add(0, parentKeyMap.get(val));  
        result2.add(arr);  
    }  
  
    return result2;  
}  
  
public String findParent(Map<String, String> parentMap,Map<String, String> link, String current) {  
    String newCurrent = link.get(current);  
  
    while(true){  
        if(!parentMap.get(newCurrent).equals(newCurrent)){  
            newCurrent = parentMap.get(newCurrent);  
        }else{  
            return parentMap.get(newCurrent);  
        }  
    }  
}
```