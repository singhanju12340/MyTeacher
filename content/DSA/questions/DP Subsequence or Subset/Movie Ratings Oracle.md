---
Creation Time: Thursday, August 22nd 2024
Modified Time: Thursday, August 22nd 2024
---

- Disk Space Analysis
    
- AM
    
    Movie Ratings
    
- Coding 2
    

Start Video

- anju
    
- Mrunal Pagnis
    

# Movie Ratings

Description

Alex loves movies and maintains a list of negative and/or positive integer ratings for the movies in a collection. Alex is getting ready for a film festival and wants to choose some subsequence of movies from the collection to bring such that the following conditions are satisfied:

- The collective sum of their ratings is _maximal_.
- Alex must go through the list _in order_ and cannot skip more than one movie in a row. In other words, Alex cannot skip over two or more consecutive movies. For example, if _ratings = [-1, -3, -2]_, and must include either the second number or the first and third numbers to get a maximal rating sum of _-3_.

**Example**

_ratings = [-3, 2, 4, -1, -2, -5]._

The maximal choices are _[2, 4, -2]_ for a sum of _4_.

**Function Description**

Complete the function _maximizeRatings_ in the editor below.

maximizeRatings has the following parameter(s):

    _int ratings[n]:_  movie ratings

**Returns**

    _int:_ the maximum possible rating sum for a subsequence of movies

**Constraints**

- _1 ≤ n ≤ 105_
- _-1000 ≤ ratings[i] ≤ 1000_, where _0 ≤ i < n_

Input Format for Custom Testing

Sample Case 0

**Sample Input 0**

STDIN    Function
-----    --------
5        ratings[] size n = 5
9        ratings = [9, -1, -3, 4, 5]
-1
-3
4
5

**Sample Output 0**

17

**Explanation 0**

Alex picks the bolded items in _ratings = [`9`, `-1`, -3, `4`, `5`]_ to get _maximum rating = 9 + -1 + 4 + 5 = 17_. Sample Case 1




import java.io.*;

  ```// Complete the maximizeRatings function below.

    static int maximizeRatings(int[] ratings) {

    }

        return fun(ratings, ratings.length-1, Integer.MIN_VALUE);

    static int fun(int[] ratings, int index, int max) {

        if(index  >=  ratings.length){

    }

            return max;

        }

        for(int i=ratings.length-1; i>0;i--){

        return max;

        }

            if(index>1){

                  int pick = fun(ratings, index-1, max+ratings[index]);

            }

            int notPick = fun(ratings, index-1, max+0);

                        max = Math.max(pick, notPick);

    private static final Scanner scanner = new Scanner(System.in);
```



Line: 22 Col: 16

Run CodeRun Tests

Input / Output

Test Cases

Compilation successful. You can **Run Test Cases** now.

Input

Your Output (stdout)

- 2147483647
    

[](https://www.hackerrank.com/codepair/mbpgzuvlaecyrmhhrklmywuulyzqmsql/questions/6?b=eyJpbnRlcnZpZXdfaWQiOjUzMTc1NDksInJvbGUiOiJpbnRlcnZpZXdlciIsInNob3J0X3VybCI6Imh0dHBzOi8vaHIuZ3MvOTE5MWUzYSIsImNhbmRpZGF0ZV91cmwiOiJodHRwczovL2hyLmdzLzYwZWZiNjEifQ)

int a, Returns the greater of two int values. That is, the result is the argument closer to the value of Integer.MAX_VALUE. If the arguments have the same value, the result is that same value. * Parameters: - a an argument. - b another argument. * Returns: - the larger of a and b., hint