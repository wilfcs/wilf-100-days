# [1011. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)

## Approaches ->
### _Brute Force:_ 

On observation we can conclude that the possible capacity of the ship must lie between the range of maximum element in the weights array to the summation of all elements in weights array. 

For example -> Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
Output: 15

Here the range of possible capacities will lie between 10 to 55.

Algorithm:

-  We will use a loop(say cap) to check all possible capacities.
-  Next, inside the loop, we will send each capacity to the findDays() function to get the number of days required for that particular capacity.
-  The minimum number, for which the number of days <= d, will be the answer.

Time complexity -> O(N * (sum(weights[]) – max(weights[]) + 1)),

### _Optimal Approach using binary search:_

Algorithm:
-  First, we will find the maximum element i.e. max(weights[]), and the summation i.e. sum(weights[]) of the given array.
-  Place the 2 pointers i.e. low and high: Initially, we will place the pointers. The pointer low will point to max(weights[]) and the high will point to sum(weights[]).
-  Calculate the ‘mid’
-  Eliminate the halves based on the number of days required for the capacity ‘mid’:
We will pass the potential capacity, represented by the variable ‘mid’, to the ‘findDays()‘ function. This function will return the number of days required to ship all the weights for the particular capacity, ‘mid’.
-  1. If munerOfDays <= d: On satisfying this condition, we can conclude that the number ‘mid’ is one of our possible answers. But we want the minimum number. So, we will eliminate the right half and consider the left half(i.e. high = mid-1).
-  2. Otherwise, the value mid is smaller than the number we want. This means the numbers greater than ‘mid’ should be considered and the right half of ‘mid’ consists of such numbers. So, we will eliminate the left half and consider the right half(i.e. low = mid+1).
-  Finally, outside the loop, we will return the value of low as the pointer will be pointing to the answer.

## Code ->
```cpp
class Solution {
public:
    int findDays(vector<int> weights, int capacity){
        int days = 0, sum = 0;

        for(int i=0; i<weights.size(); i++){
            sum+=weights[i];
            if(sum<=capacity) continue;
            days++;
            sum=weights[i];
        }
        return ++days;
    }

    int shipWithinDays(vector<int>& weights, int days) {
        // using stl to find high and low.
        int hi = accumulate(weights.begin(), weights.end(), 0);
        int lo = *max_element(weights.begin(), weights.end());
        int mid;

        while(lo<=hi){
            mid = (lo+hi)/2;

            int daysWithMid = findDays(weights, mid);

            // if for mid the amount of days are less than the days given then that probably means that mid is way more than it should be, that's why it was able to accomodate all weights in less number of days, hence eliminate the right half and keep searching on the left.
            if(daysWithMid<=days) hi = mid-1; 
            else lo = mid+1;
        }
        return lo; // lo will keep pointing on the most optimal mid always
    }
};
```

# [ Number of occurrence](https://www.codingninjas.com/studio/problems/occurrence-of-x-in-a-sorted-array_630456?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM)

## Approach ->
(Last occurence - First occurence + 1)
and if there is no occurence then return 0

## Code ->
```cpp
int firstOccurrence(vector<int> &arr, int n, int k) {
    int low = 0, high = n - 1;
    int first = -1;

    while (low <= high) {
        int mid = (low + high) / 2;
        // maybe an answer
        if (arr[mid] == k) {
            first = mid;
            //look for smaller index on the left
            high = mid - 1;
        }
        else if (arr[mid] < k) {
            low = mid + 1; // look on the right
        }
        else {
            high = mid - 1; // look on the left
        }
    }
    return first;
}

int lastOccurrence(vector<int> &arr, int n, int k) {
    int low = 0, high = n - 1;
    int last = -1;

    while (low <= high) {
        int mid = (low + high) / 2;
        // maybe an answer
        if (arr[mid] == k) {
            last = mid;
            //look for larger index on the right
            low = mid + 1;
        }
        else if (arr[mid] < k) {
            low = mid + 1; // look on the right
        }
        else {
            high = mid - 1; // look on the left
        }
    }
    return last;
}

int count(vector<int>& arr, int n, int x) {
	int l = lastOccurrence(arr, n, x);
	int f = firstOccurrence(arr, n, x);
	if(f==-1) return 0; // if no occurence return 0
	return l-f+1;
}
```

# [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/description/)

## Approach ->
The sqrt of a number will always lie in the range of 1 to the number itself. So we will check for mid*mid and if it is smaller than or equal to n then it might be the answer, so eleminate the left half. You can store the ans in ans variable or simply return high at end because if you think about it high will always point to the answer.

## Code ->
```cpp
class Solution {
public:
    int mySqrt(int x) {
        // using binary search
        int low = 1, high = x;
        //Binary search on the answers:
        while (low <= high) {
            // always calculate mid using this formula to avoid overflow
            long long mid = low + (high-low)/2;
            long long val = mid * mid;
            if (val <= (long long)(x)) {
                //eliminate the left half:
                low = mid + 1;
            }
            else {
                //eliminate the right half:
                high = mid - 1;
            }
        }
        return high;
    }
};
```
# [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/description/)

## Approaches ->
1. The extremely naive approach is to check all possible answers from 1 to max(a[]). The minimum number for which the required time <= h, is our answer.
2. Apply binary search 

## Code ->
```cpp
class Solution {
public:
    int calculateTotalHours(vector<int> &piles, int hourly) {
        long long totalH = 0;
        int n = piles.size();
        //find total hours
        for (int i = 0; i < n; i++) {
            totalH += ceil((double)(piles[i]) / (double)(hourly));
        }
        return totalH;
    }

    int minEatingSpeed(vector<int>& piles, int h) {
        int low = 1, high = *max_element(piles.begin(), piles.end());

        while (low <= high) {
            int mid = (low + high) / 2;
            int totalH = calculateTotalHours(piles, mid);
            // if for the given number of bananas per hour (mid), total hour is calculated. If this total hour is smaller than the given hour then that means koko is eating too fast, so we will lower the value of mid(no. of bananas per hour)
            if (totalH <= h) {
                high = mid - 1;
            }
            // else just make the value of mid more
            else {
                low = mid + 1;
            }
        }
        return low;
        // low will point the required answer because as we know the rule of returning i.e. if there is a possible answer, and we eleminate a side, we return the opposite to the eliminated side. We can store the ans in ans variable as well if confusing.
    }
};
```

# [1482. Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/description/)

## Approach->
The solution is very simple when you understand the question properly. 

Input Format: N = 8, arr[] = {7, 7, 7, 7, 13, 11, 12, 7}, m = 2, k = 3
Result: 12

Let's grasp the question better with the help of an example. Consider an array: {7, 7, 7, 7, 13, 11, 12, 7}. We aim to create bouquets with k, which is 3 adjacent flowers, and we need to make m, which is 2 such bouquets. Now, if we try to make bouquets on the 11th day, the first 4 flowers and the 6th and the last flowers would have bloomed. So, we will be having 6 flowers in total on the 11th day. However, we require two groups of 3 adjacent flowers each. Although we can form one group with the first 3 adjacent flowers, we cannot create a second group. Therefore, 11 is not the answer in this case.

So basically what we need to do is, for a given day we will keep counting the number of adjacent blooming flower and when that number is equal to k then that means that one bouqet can be made. By doing this we can find the total number of bouquets made for a given day and compare it with m to find our answer. Use binary search to do it in the most optimal way

## Code ->
```cpp
class Solution {
public:
    // returns the number of booq that can be made for mid's value (day) 
    int numberOfBouq(vector<int> bloomDay, int k, int days){
        int numBook = 0, numAdj=0;
        for(int i=0; i<bloomDay.size(); i++){
            // if the flower is blooming, increase the value of numAdj else make numAdj 0
            if(days>=bloomDay[i]) numAdj++;
            else numAdj = 0;


            // if numAdj is equal to k then that means one bouquet can be made. Reset numAdj.
            if(numAdj==k){
                numBook++;
                numAdj=0;
            }
        }

        return numBook;
    }
    int minDays(vector<int>& bloomDay, int m, int k) {
        int maxNum = *max_element(bloomDay.begin(), bloomDay.end());
        int low = 0, high = maxNum, mid, ans=-1;

        while(low<=high){
            mid = low + (high-low)/2;
            int numBouq = numberOfBouq(bloomDay, k, mid);

            // if number of bouq for a given day is greater than equal to required bouq then that day can be the potential answer.
            if(numBouq>=m){
                high = mid-1;
                ans = mid;
            }
            else low = mid+1;
        }
        return ans;
    }
};
```
# [1283. Find the Smallest Divisor Given a Threshold](https://leetcode.com/problems/find-the-smallest-divisor-given-a-threshold/description/)

## Code ->
```cpp
class Solution {
public:
    // Fn. to find the summation of division values
    int sumByD(vector<int> &arr, int div) {
        int n = arr.size();
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += ceil((double)(arr[i]) / (double)(div));
        }
        return sum;
    }
    int smallestDivisor(vector<int>& nums, int threshold) {
        // lower the divisor, higher will be the sum by divisor, so we will try to find the lowest divisor that satisfies the conditions
        int n = nums.size();
        if (n > threshold) return -1;
        int low = 1, high = *max_element(nums.begin(), nums.end());

        //Apply binary search:
        while (low <= high) {
            int mid = (low + high) / 2;
            if (sumByD(nums, mid) <= threshold) {
                // this is a possible answer but let's try reducing the divisor
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }
        return low;
    }
};
```