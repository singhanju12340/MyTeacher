---
Creation Time: Wednesday, October 16th 2024
Modified Time: Wednesday, October 16th 2024
---

![[Screenshot 2024-10-16 at 10.28.57 PM.png]]


## If nums are without duplicate

```java
public List<List<Integer>> permute(int[] nums) {  
    List<List<Integer>> list = new ArrayList<>();  
    // Arrays.sort(nums); // not necessary  
    backtrack(list, new ArrayList<>(), nums);  
    return list;  
}  
  
private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums){  
    if(tempList.size() == nums.length){  
        list.add(new ArrayList<>(tempList));  
    } else{  
        for(int i = 0; i < nums.length; i++){  
            if(tempList.contains(nums[i])) continue; // element already exists, skip  
            tempList.add(nums[i]);  
            backtrack(list, tempList, nums);  
            tempList.remove(tempList.size() - 1);  
        }  
    }  
}

```



## If nums are with duplicate

```java
public List<List<Integer>> permuteUnique(int[] nums) {  
    List<List<Integer>> list = new ArrayList<>();  
    Arrays.sort(nums);  
    backtrack(list, new ArrayList<>(), nums, new boolean[nums.length]);  
    return list;  
}  
  
private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, boolean [] used){  
    if(tempList.size() == nums.length){  
        list.add(new ArrayList<>(tempList));  
    } else{  
        for(int i = 0; i < nums.length; i++){  
            if(used[i] || i > 0 && nums[i] == nums[i-1] && !used[i - 1]) continue;  
            used[i] = true;  
            tempList.add(nums[i]);  
            backtrack(list, tempList, nums, used);  
            used[i] = false;  
            tempList.remove(tempList.size() - 1);  
        }  
    }  
}
```