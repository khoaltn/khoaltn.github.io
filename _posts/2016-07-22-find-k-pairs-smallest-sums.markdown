---
layout: post
title:  "Leetcode #373: Find K Pairs with Smallest Sums"
date:   2016-07-22 22:41:19 -0500
categories: Leetcode
tag: 
- leetcode
- algorithms
headerImage: true
blog: true
hidden: false # count this post in blog pagination
author: site.author
---

###### Problem source: <https://leetcode.com/problems/find-k-pairs-with-smallest-sums/> 
 
 
#### Problem Statement:
You are given two integer arrays `nums1` and `nums2` sorted in ascending order and an integer `k`. 
Define a pair *(u,v)* which consists of one element from the first array and one element from the second array.
Find the `k` pairs *(u<sub>1</sub>,v<sub>1</sub>),(u<sub>2</sub>,v<sub>2</sub>) ...(u<sub>k</sub>, v<sub>k</sub>)* with the smallest sums.
 
*Example*:
Given `nums1 = [1,7,11], nums2 = [2,4,6], k = 3`.
Return: `[1,2],[1,4],[1,6]`
 
 
#### Solution: 
We know from mathematics that there will be `n1 * n2` possible pairs where `n1, n2` are the sizes of `nums1` and `nums2` respectively. As a human, we will naturally match each element in `nums1` to all the elements in `nums2` to create a list of all possible pairs, sort them, and then output the k pairs. This surely results in a correct solution, but performs too many unnecessary computations if `k < n1 * n2`. So the idea for a faster solution here is to keep track of how many elements of `nums2` we have matched each element of `nums1` with, then find the next smallest pair from all possible pairs of *(u, v)* where `u` is an element of `nums1` and `v` is its next corresponding element in `nums2` until we have found k pairs or exhausted all the possible pairs.
 
Here is the complete code in C++ with detailed comments and explanations:
 
{% highlight c++ %}
vector<pair<int, int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, 
                                      int k) {
    vector<pair<int, int>> result;
    int n1 = nums1.size(), n2 = nums2.size();
    if (n1 == 0 || n2 == 0) return result;
    //sort(nums1.begin(), nums1.end());  //the lists are already sorted
	//sort(nums2.begin(), nums2.end());
	int count = 0;
    /* this array tracks the index of the next element in nums2
	to match with each element in nums1 */
    int* other = new int[n1];          
    memset(other, 0, sizeof(int) * n1);  // set all indices in other[] to 0
    /* min is the index of the current smallest element in nums1 
	that hasn't finished matching with all elements in nums2. */
    int min = 0, first, second, index;
    /* 2 cases when the loop stops: (1) we find all k pairs; or
      (2) we find every possible pair that can be made from nums1 and nums2. */
    while (count < k && count < n1 * n2) {
        /* first, second form the next smallest pair. 
		index contains the index of first in nums1 */
        first = nums1[min];
        second = nums2[other[min]];
        index = min;
        /* this loop finds all the possible candidates to be the next 
		smallest pair and compares them to the current one */
        for (int i = min + 1; i < n1; i++) {
            if (nums1[i] + nums2[other[i]] < first + second) {
                first = nums1[i];
                second = nums2[other[i]];
                index = i;
            }
        }
        //cout << first << " ; " << second << endl;
        pair<int, int> tmp(first, second);
        result.push_back(tmp);
        other[index] += 1;
        count += 1;
        // Update min if nums1[min] has matched with all the elements of nums2
        if (other[min] == n2) {
            min += 1;
        }
    }
    delete [] other;
    return result;
}
{% endhighlight %}
